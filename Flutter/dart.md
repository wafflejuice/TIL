# 2022-10-06
## `use_build_context_synchronously` lint with an `async` keyword
> **BAD:**
```dart
void onButtonTapped(BuildContext context) async {
  await Future.delayed(const Duration(seconds: 1));
  Navigator.of(context).pop();
}
```

> **GOOD:**
```dart
void onButtonTapped() async {
  await Future.delayed(const Duration(seconds: 1));

  if (!mounted) return;
  Navigator.of(context).pop();
}
```

By `if (!mounted) return;` line, you can avoid an error which occurs when the context you want to use is **invalid**.  
**"Invalid"** means: If the widget is removed from the tree, the build context becomes invalid, even if its value is still available in context variable.

references:
- [dart linter](https://dart-lang.github.io/linter/lints/use_build_context_synchronously.html)
- [stackoverflow](https://stackoverflow.com/questions/73945088/need-some-explanations-about-use-build-context-synchronously)
