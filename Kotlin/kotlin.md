# 2022-07-15
## 확장함수는 override할 수 없다.
확장함수는 override할 수 없다. 부모-자식 간 동일한 시그니처를 가진 함수라도 컴파일 시점에 결정된다.
```kotlin
fun View.showOff() = println("It is a view.")
fun Button.showOff() = println("It is a button.")

val view: View = Button()
view.showOff() // It is a view.
```
내부적으로 확장 함수는 수신 객체를 첫 번째 인자로 받는 **정적** 메서드이기 때문이다.

# 2022-07-16
## equality & reference comparison
|   |Java|Kotlin|
|---|------|---|
|equality|equals|==|
|reference|==|===|

# 2022-07-20
## supertype vs. covariant
![supertype_covariant.png](./supertype_covariant.PNG)
