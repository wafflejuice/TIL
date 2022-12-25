# 2022-10-09
## Android's SDK version information location
Android's SDK version information does not locate inside 
`android/app/build.gradle`, if it's using predefined flutter information.

You can find it inside here:
```
{flutter-directory}\packages\flutter_tools\gradle\flutter.gradle
```
![flutter_version](flutter_version.PNG)

# 2022-10-10
## rename App ID
1. install `rename` package.

`dart pub global activate rename`

2. run `rename` package.

`dart pub global run rename --bundleId {App-ID}`

# 2022-10-11
## Function.apply(function, [param1, param2, ...]) vs. function.call(param1, param2, ...)
```dart
Function(String name, int vintage) printWineDetails = ((String name, int vintage){
    print('Name: $name, Vintage: $vintage');
});


void main() {
    // Function.apply(function, [param1, param2, ...])
    Function.apply(printWineDetails, ['Cabernet Sauvignon', 2018]);
    
    // function.call(param1, param2, ...)
    printWineDetails.call('Cabernet Sauvignon', 2018);
}
```

# 2022-12-23
## async & FutureBuilder
async function은 항상 Future를 반환한다.  
해당 async function이 종료되었을 때를 탐지하기 위해서는 FutureBuilder를 사용한다.

```dart
...
child: FutureBuilder<SomeFuture>(
    future: someAsyncFunction(),
    builder: (BuildContext context, AsyncSnapshot snapshot) {
        if (snapshot.hasData == false) {
            // placeholder
        } else if (snapshot.hasError) {
            // error handling
        } else {
            // main logic
        }
    }),
...
```

# 2022-12-24
## MediaQuery inside initState
Executing the below code will produce errors.
```dart
  @override
  void initState() {
    super.initState();

    offsetX = MediaQuery.of(context).size.width * 0.5;
    offsetY = MediaQuery.of(context).size.height * 0.5;
    preX = offsetX;
    preY = offsetY;
  }
```

errors:
```
======== Exception caught by widgets library =======================================================
The following assertion was thrown building KeyedSubtree-[GlobalKey#45265]:
dependOnInheritedWidgetOfExactType<MediaQuery>() or dependOnInheritedElement() was called before DraggableObjectPageState.initState() completed.
```

Using `Future.delayed` function would solve the problem.
```dart
  @override
  void initState() {
    super.initState();

    Future.delayed(Duration.zero, () {
      offsetX = MediaQuery.of(context).size.width * 0.5;
      offsetY = MediaQuery.of(context).size.height * 0.5;
      preX = offsetX;
      preY = offsetY;
    });
  }
```

# 2022-12-26
## sqflite: onCreate will be executed only when there is no database file
sqflite: `onCreate` will be executed **only when there is no database file**. Not when there is no table in the db.
