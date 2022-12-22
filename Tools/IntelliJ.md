# 2022-04-17
`Ctrl`+`Shift`+`Enter` : complete line  
`Ctrl`+`Alt`+`V` : extract variable  
`Ctrl`+`E` -> Enter : go back to previous file  
`Shift`+`F10` : run  

# 2022-04-21
`Ctrl`+`Click` : find the usage of the clicked method

# 2022-04-22
`Ctrl`+`Alt`+`N` : inline variable

# 2022-04-23
`Alt`+`F12` : switch to terminal window
`Alt`+`4` : switch to run window

# 2022-04-24
`Ctrl`+`Shift`+`T` : generate test codes

# 2022-07-26
## test method명 한글 깨짐
Help > Edit Custom VM Options ... 에서 `-Dfile.encoding=UTF-8` 추가

[참고](https://itchipmunk.tistory.com/421)

# 2022-09-29
## 자동완성
`Ctrl`+`Space` / `Command`+`Space`

# 2022-11-16
## live template
`File>Settings>Editor>Live Templates`

test template example:

```kotlin
@Test
fun $TEST$() {
    // Arrange
    val expected = $EXPECTED$
    
    // Act
    val actual = $ACTUAL$
    
    // Assert
    assertEquals(expected, actual)
}
```

# 2022-11-28
## Project '{project-name}' not found in root project '{project-name}'.
If following error occurs,

```
Project '{project-name}' not found in root project '{project-name}'.
```

delete the `.gradle` folder.

[stackoverflow](https://stackoverflow.com/questions/16753798/android-project-myproject-not-found-in-root-project-myproject)

# 2022-12-10
## Extract the method
`Ctrl`+`Alt`+`M` : Extract the method

## Parametarize the value
`Ctrl`+`Alt`+`P` : Parametarize the value

# 2022-12-23
## jdk version 오류가 발생하면 확인해볼 것
1. Run/Debug Configurations : Build and run
2. Project structure > Project Settings > Project : SDK
