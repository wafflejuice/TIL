# 2022-06-28
## Integer.parseInt(...) vs. Integer.valueOf(...)
`parseInt(...)` returns `int`, the primitive type.  
`valueOf(...)` returns `Integer`, a wrapper of `int`.

# 2022-06-30
## TreeMap
### BOJ _7662_이중_우선순위_큐
Java에 Binary Search Tree는 존재하지 않지만, 더 좋은 TreeMap이 있다(Red Black Tree).  
TreeMap 값 수정 시 주의해야할 점 : TreeMap 내부 함수를 이용해 얻은 entry에  `setValue` 함수를 적용하면 안 된다! 이렇게 얻은 entry는 단순한 snapshot이기 때문이다.

*All Map.Entry pairs returned by methods in this class and its views represent snapshots of mappings at the time they were produced. They do not support the Entry.setValue method. (Note however that it is possible to change mappings in the associated map using put.)*  
[Java11 TreeMap Doc 링크](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/TreeMap.html)

# 2022-07-07
## null-safe
null은 값이 아니다. T를 반환하는 method는 null을 반환하기보다는 empty T를 반환하는 것이 좋다.

## final vs. immutable
- final : Object의 reference를 바꾸지 못 한다.  
- immutable : Object의 value를 바꾸지 못 한다.  

# 2022-11-28
## Locale.KOREA vs. Locale.KOREAN
Inside `Locale.java` file:

- `Locale.KOREAN` and `Locale.KOREA`
```java
static {
    ...
    KOREAN = createConstant(BaseLocale.KOREAN);
    ...
    KOREA = createConstant(BaseLocale.KOREA);
    ...
}
```

- `createConstant` function
```java
private static Locale createConstant(byte baseType) {
    BaseLocale base = BaseLocale.constantBaseLocales[baseType];
    Locale locale = new Locale(base, null);
    CONSTANT_LOCALES.put(base, locale);
    return locale;
}
```

Inside `BaseLocale.java` file:

- `constantBaseLocales`
```java
public static @Stable BaseLocale[] constantBaseLocales;
public static final byte ENGLISH = 0,
        FRENCH = 1,
        GERMAN = 2,
        ...
        KOREAN = 5,
        ...
        KOREA = 13,
        ...
        ROOT = 18,
        NUM_CONSTANTS = 19;
static {
    CDS.initializeFromArchive(BaseLocale.class);
    BaseLocale[] baseLocales = constantBaseLocales;
    if (baseLocales == null) {
        baseLocales = new BaseLocale[NUM_CONSTANTS];
        baseLocales[ENGLISH] = createInstance("en", "");
        baseLocales[FRENCH] = createInstance("fr", "");
        baseLocales[GERMAN] = createInstance("de", "");
        ...
        baseLocales[KOREAN] = createInstance("ko", "");
        ...
        baseLocales[KOREA] = createInstance("ko", "KR");
        ...
    }
}
```

- `createInstance` function
```java
private static BaseLocale createInstance(String language, String region) {
    return new BaseLocale(language, "", region, "", false);
}
```

### Conclusion: `Locale.KOREAN` for language, `Locale.KOREA` for language & region.
