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
