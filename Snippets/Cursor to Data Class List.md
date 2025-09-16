---
title: Cursor to Data Class List
date:  2025-09-16
time:  17:04:35
tags:
  - kotlin
  - android
---
```kotlin
// 파라미터 별로 컬럼 정보 및 getter 구성  
data class ColMeta(val index: Int, val getter: (Cursor.(Int) -> Any?)?, val param: KParameter)  
  
/**  
 * 리플렉션 기반으로 Cursor의 각 row를 지정된 데이터 클래스 인스턴스로 변환하는 함수.  
 * * - 데이터클래스의 각 파라미터에 대해 DB 컬럼값을 자동 매핑함.  
 * - 컬럼이 없는 경우, id 필드는 UUID로, 그 외는 기본값(빈 문자열, 0 등)으로 자동 세팅.  
 * - SerialName 애노테이션을 컬럼명에 반영함.  
 * * @return List<T> : 각 row가 T 데이터클래스 형태로 매핑된 리스트  
 */  
inline fun <reified T : Any> Cursor.toDataClassList(): List<T> {  
    val kClass = T::class  
    val ctor = kClass.primaryConstructor ?: error("No primary constructor for ${kClass.simpleName}")  
    val propsByName = kClass.memberProperties.associateBy { it.name }  
  
    // 컬럼 이름/인덱스/파라미터 정보 미리 캐싱  
    val paramInfo: List<ColMeta> = ctor.parameters.map { param ->  
        val prop = propsByName[param.name!!]!!  
        val colName = prop.findAnnotation<SerialName>()?.value ?: prop.name // SerialName 어노테이션이 컬럼명으로 사용  
        val colIdx = getColumnIndex(colName)  
        // 타입별 Cursor getter 지정  
        val getter: (Cursor.(Int) -> Any?)? = when {  
            colIdx == -1 -> null  
            param.type.classifier == Long::class -> { i -> getLong(i) }  
            param.type.classifier == Int::class -> { i -> getInt(i) }  
            param.type.classifier == String::class -> { i -> getString(i) }  
            param.type.classifier == Double::class -> { i -> getDouble(i) }  
            param.type.classifier == Float::class -> { i -> getFloat(i) }  
            param.type.classifier == Boolean::class -> { i -> getInt(i) != 0 }  
            else -> error("Unsupported type for $colName")  
        }  
  
        ColMeta(colIdx, getter, param)  
    }  
    LogHelper.v("paramInfo: $paramInfo")  
  
//    val list = mutableListOf<T>()  
    val list = ArrayList<T>(count.takeIf { it > 0 } ?: 10) // 예상 row 갯수로 사이즈 확보  
    while (moveToNext()) {  
        val args = buildMap {  
            for (meta in paramInfo) {  
                val p = meta.param  
                val ktype = p.type  
  
                // 1) DB에서 값 읽기 (getter가 있고 컬럼 인덱스가 유효할 때만)  
                val dbValue: Any? = when {  
                    meta.getter != null && meta.index >= 0 && !isNull(meta.index) -> {  
                        // meta.getter는 (Cursor, Int) -> Any? 형태라고 가정  
                        meta.getter.invoke(this@toDataClassList, meta.index)  
                    }  
                    else -> null  
                }  
  
                // 2) NULL/결측 처리 규칙 적용  
                val finalValue: Any? = when {  
                    dbValue != null -> dbValue  
  
                    // 기본값이 있는 파라미터라면 맵에 "넣지 않는 것"이 핵심!  
                    // callBy가 빠진 파라미터에 대해 기본값을 자동 적용합니다.  
                    p.isOptional -> continue  
  
                    // nullable 파라미터면 그대로 null 전달  
                    ktype.isMarkedNullable -> null  
  
                    // 여기서부터는 non-null & 기본값 없음 → 타입별 안전 기본값(또는 에러)  
                    ktype.classifier == String::class && p.name == "uuid" -> UUID.randomUUID().toString()  
                    ktype.classifier == String::class -> ""  
                    ktype.classifier == Int::class -> 0  
                    ktype.classifier == Long::class -> 0L  
                    ktype.classifier == Double::class -> 0.0  
                    ktype.classifier == Float::class -> 0f  
                    ktype.classifier == Boolean::class -> false  
  
                    else -> error("필수 파라미터 '${p.name}'에 값이 없고 기본값도 없습니다.")  
                }  
  
                put(p, finalValue)  
            }  
        }  
  
        // 3) 인스턴스 생성  
        val item = ctor.callBy(args)  
        list += item  
    }  
  
    LogHelper.i("toDataList(): $list")  
    return list  
}

```