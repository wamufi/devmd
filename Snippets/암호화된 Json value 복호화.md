---
title: 암호화된 Json value 복호화
date: 2024-08-09
time: 12:05:25
tags:
- swift
- kotlin
---
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

```kotlin
data class Person(
    val name: String?,
    val age: Int?,
    val addr: String?
)

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