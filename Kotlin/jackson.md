# 2023-09-22
## Custom JsonSerializer
### **1. Serializer**

custom JsonSerializer를 정의하면 번거로운 YN <-> boolean 변환을 피할 수 있습니다.

`BooleanYnSerializer`라고 명명해보겠습니다.

```kotlin
class BooleanYnSerializer : StdSerializer<Boolean>(Boolean::class.java) {
    override fun serialize(value: Boolean, gen: JsonGenerator, serializers: SerializerProvider) {
        when (value) {
            true -> gen.writeString("Y")
            false -> gen.writeString("N")
        }
    }
}
```

그러면 `@JsonSerialize` annotation을 붙여주면 자동으로 변환해줍니다.

```kotlin
    @JsonSerialize(using = BooleanYnSerializer::class)
    val isFirstBuyer: Boolean,
```

### **2. jsonMapper**

앞서 만든 serializer를 jsonMapper에 등록하면 좀더 편해집니다.

기존 primary jsonMapper bean을 살짝 변형하여 `scrapingJsonMapper`를 만들어보겠습니다.

```kotlin
    @Bean
    fun scrapingJsonMapper(): JsonMapper {
        val builder = JsonMapper.builder()

        builder.addModule(KotlinModule.Builder().build())
        builder.addModule(JavaTimeModule())

        builder.propertyNamingStrategy(PropertyNamingStrategies.SNAKE_CASE)

        builder.serializationInclusion(JsonInclude.Include.NON_NULL)

        builder.configure(SerializationFeature.WRITE_DATES_AS_TIMESTAMPS, false)
        builder.configure(DeserializationFeature.FAIL_ON_UNKNOWN_PROPERTIES, false)
        builder.configure(DeserializationFeature.FAIL_ON_IGNORED_PROPERTIES, false)

        // 추가되는 부분 시작
        val simpleModule = SimpleModule()
        simpleModule.addSerializer(BooleanYnSerializer())

        val jsonMapper = builder.build()
        jsonMapper.registerModule(simpleModule)
        // 추가되는 부분 종료

        return jsonMapper
    }
```

이제 `@JsonSerialize` annotation 없이도 자동으로 변환됩니다.

### 3. **Test**

간단하게 Test API를 만들어 테스트해봤습니다.

```kotlin
@RequestMapping("/test/serialization")
@RestController
class TestController(
    private val jsonMapper: JsonMapper,
    @Qualifier("scrapingJsonMapper") private val scrapingJsonMapper: JsonMapper,
) {
    @GetMapping("/annotation")
    fun testAnnotation() {
        val request = TestAnnotationRequest(
            intVariable = 34,
            longVariable = 646345L,
            booleanVariable = true,
            stringVariable = "abc",
        )
        println(jsonMapper.writeValueAsString(request))
    }

    @GetMapping("/json-mapper")
    fun testJsonMapper() {
        val request = TestJsonMapperRequest(
            intVariable = 34,
            longVariable = 646345L,
            booleanVariable = true,
            stringVariable = "abc",
        )
        println(scrapingJsonMapper.writeValueAsString(request))
    }
}
```

출력 결과는 다음과 같습니다.

- GET {{mortgage-loan-url}}/test/serialization/annotation

`{"int_variable":34,"long_variable":646345,"boolean_variable":"Y","string_variable":"abc"}`

- GET {{mortgage-loan-url}}/test/serialization/json-mapper

`{"int_variable":34,"long_variable":646345,"boolean_variable":"Y","string_variable":"abc"}`

# 2023-10-23
## Deserializer for primitive-boxed types
다음 Custom Deserializer를 등록하였으나 Boolean 값으로 변환하지 못 하고 오류가 발생한다.

```kotlin
import com.fasterxml.jackson.core.JsonParser
import com.fasterxml.jackson.databind.DeserializationContext
import com.fasterxml.jackson.databind.JsonDeserializer

class NiceBooleanDeserializer : JsonDeserializer<Boolean?>() {
    override fun deserialize(parser: JsonParser, context: DeserializationContext?): Boolean? =
        when (parser.text) {
            "1" -> true
            "0" -> false
            else -> null
        }
}
```

```kotlin
val simpleModule = SimpleModule()
simpleModule.addDeserializer(Boolean::class.java, NiceBooleanDeserializer())
```

```
Cannot deserialize value of type `java.lang.Boolean` from String "1": only "true" or "false" recognized
com.fasterxml.jackson.databind.exc.InvalidFormatException: Cannot deserialize value of type `java.lang.Boolean` from String "1": only "true" or "false" recognized
```

Java Boolean의 경우, primitive와 Boxed type이 둘다 존재하기 때문에 다음과 같이 둘다 등록해주어야 한다.

```kotlin
val simpleModule = SimpleModule()
simpleModule.addDeserializer(Boolean::class.java, NiceBooleanDeserializer())
simpleModule.addDeserializer(Boolean::class.javaObjectType, NiceBooleanDeserializer())
```

reference: https://github.com/FasterXML/jackson-module-kotlin/issues/320#issuecomment-611783188
