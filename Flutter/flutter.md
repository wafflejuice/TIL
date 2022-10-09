# 2022-10-09
## Android's SDK version information location
Android's SDK version information does not locate inside 
`android/app/build.gradle`, if it's using predefined flutter information.

You can find it inside here:
```
{flutter-directory}\packages\flutter_tools\gradle\flutter.gradle
```
![flutter_version](flutter_version.PNG)
