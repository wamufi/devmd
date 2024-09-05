---
title: 암호화된 Json value 복호화
date: 2024-08-09
time: 12:05:25
tags:
- swift
- kotlin
---
## Swift
Decodable.swift
```swift
struct Person: Decodable {
    var name: String?
    var age: Int?
    var addr: String?
    
    enum CodingKeys: CodingKey {
        case name
        case age
        case addr
    }
    
    init(from decoder: any Decoder) throws {
        let container = try decoder.container(keyedBy: CodingKeys.self)
        
        let encryptedName = try container.decodeIfPresent(String.self, forKey: .name)
        let encryptedAge = try container.decodeIfPresent(Int.self, forKey: .age)
        
        self.name = decrypt(cipherText: encryptedName)
        if let age = decrypt(cipherText: encryptedAge) {
            self.age = Int(age)
        }
        self.addr = try container.decodeIfPresent(String.self, forKey: .addr)
    }
}

fileprivate func decrypt(cipherText: String?) -> String? {
    guard let cipherText = cipherText else { return cipherText }
    let aes = AES()
    return aes?.decrypt(data: Data(base64Encoded: cipherText))
}
```

## Kotlin
### Gson
DataClass.kt
```kotlin
data class Person(
	@SerializedName("name")
    val name: String?,
    @SerializedName("age")
    val age: Int?,
    @SerializedName("addr")
    val addr: String?
)
```

JsonDeserializer.kt
```kotlin
import com.google.gson.*
import java.lang.reflect.Type

class PersonDeserializer : JsonDeserializer<Person> {

    override fun deserialize(json: JsonElement, typeOfT: Type, context: JsonDeserializationContext): Person {
        val jsonObject = json.asJsonObject

        val encryptedName = jsonObject.get("name")?.asString
        val encryptedAge = jsonObject.get("age")?.asString
        val addr = jsonObject.get("addr")?.asString

        val name = decrypt(encryptedName)
        val age = decrypt(encryptedAge).toIntOrNull()

        return Person(name, age, addr)
    }

    private fun decrypt(cipherText: String?): String? {
        return decrypt(cipherText?.toString())
    }
}
```

AsdfService.kt
```kotlin
interface AsdfService {

	companion object {  
	    private const val BASE_URL = "https://asdf.com/"  
	  
	    fun create(): AsdfService {  
	        val logger = HttpLoggingInterceptor().apply { level = HttpLoggingInterceptor.Level.BODY }  
	  
	        val client = OkHttpClient.Builder().addInterceptor(logger).build()  
	        val gson = GsonBuilder().registerTypeAdapter(DataClass::class.java, JsonDeserializer()).create()  
	  
	        return Retrofit.Builder().baseUrl(BASE_URL).client(client)  
	            .addConverterFactory(GsonConverterFactory.create(gson))  
	            .build().create(AsdfService::class.java)  
	    }
	}
}
```

### Kotlin Serialization
DataClass.kt
```kotlin
@Serializable
data class Person(
	@Serializable(with = EncryptedStringSerializer::class)
    val name: String?,
    @Serializable(with = EncryptedStringSerializer::class)
    val age: Int?,
    val addr: String?
)
```

EncryptedStringSerializer.kt
```kotlin
object EncryptedStringSerializer : KSerializer<String> {
    override val descriptor: SerialDescriptor = PrimitiveSerialDescriptor("EncryptedString", PrimitiveKind.STRING)

    override fun serialize(encoder: Encoder, value: String) {
        encoder.encodeString(value)
    }

    override fun deserialize(decoder: Decoder): String {
        val encryptedString = decoder.decodeString()
        return EncryptionUtil.decrypt(encryptedString)
    }
}
```

AsdfService.kt
```kotlin
interface AsdfService {

	companion object {  
	    private const val BASE_URL = "https://asdf.com/"  
	  
	    fun create(): AsdfService {  
	        val logger = HttpLoggingInterceptor().apply { level = HttpLoggingInterceptor.Level.BODY }  
	  
	        val client = OkHttpClient.Builder().addInterceptor(logger).build()  
			
	        return Retrofit.Builder().baseUrl(BASE_URL).client(client)  
	            .addConverterFactory(Json.asConverterFactory("application/json; charset=UTF8".toMediaType())) 
	            .build().create(AsdfService::class.java)  
	    }
	}
}
```