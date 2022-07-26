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
![supertype_covariant.png](./supertype_covariant.png)

# 2022-07-21
## Array\<Int\> vs. IntArray
Boxed type vs. primitive type
|Kotlin|Java|
|---|------|
|Array\<Int\>|Integer[]|
|IntArray|int[]|

# 2022-07-22
## null safe examples
```Kotlin
println(null == "a") // false
println(null === "a") // false
println(null is String) // false
```

# 2022-07-23
## visibility
|visibility|class member|top level declaration|
|---|---|---|
|public (default)|everywhere|everywhere|
|internal|same module|same module|
|protected|sub-class|N/A|
|private|same class|same file|

## loop [break/continue]
Can state a loop to [break/continue].
```Kotlin
for (i in 1..3) {
    println("i=$i")
    jloop@for (j in 1..3) { // break this loop
        println("\tj=$j")
        for (k in 1..3) {
            println("\t\tk=$k")
            if (k == 2) {
                break@jloop;
            }
        }
    }
}
```
result
```Kotlin
i=1
	j=1
		k=1
		k=2
i=2
	j=1
		k=1
		k=2
i=3
	j=1
		k=1
		k=2
```

# 2022-07-24
## try-catch-finally
The value of `finally` can't be the value of the try expression.

>The value of the try-expression is the same as the value of the last expression of the try body (if no exception was thrown) or the value of the last expression of the matching catch block (if an exception was thrown and matched). [kotlin doc](https://kotlinlang.org/spec/expressions.html#try-expressions)

```Kotlin
fun getNumber(str: String): Int {
    return try {
        Integer.parseInt(str)
    } catch (e: NumberFormatException) {
        0
    } finally {
        println("finally")
        -1 // no value will be returned from finally block
    }
}

println(getNumber("5"))
```
result
```Kotlin
finally
5
```

# 2022-07-26
## Type erasure of generics
Obviously this code below works.
```Kotlin
val listAny = listOf("first", "second", "third")
val strList = listAny as List<String>
println(strList[1].replace("second", "two"))
```
result
```Kotlin
two
```

JVM doesn't know the data type of the generic at runtime.

```Kotlin
val listAny = listOf(1, 2, 3)

// No error, because the data type of generic is erased.
val strList = listAny as List<String>

println(strList[1].replace("second", "two")) // error occurs
```

## reified parameter type inside a inline function
Use `inline`, `reified` to check type within a function in runtime.
```kotlin
inline fun <reified T> function(arg: T) {
    ...
}
```
