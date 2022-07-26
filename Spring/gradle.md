# 2022-07-26
## gradle to kotlin DSL example
`build.gradle`

```gradle
buildscript {
    ext {
        springBootVersion = '2.1.7.RELEASE'
    }
    // ...
    dependencies {
        classpath("org.springframework.boot:spring-boot-gradle-plugin:${"springBootVersion"}")
    }
}
```

`build.gradle.kts`

```gradle
buildscript {
    extra["springBootVersion"] = "2.1.7.RELEASE"
    // ...
    dependencies {
        classpath("org.springframework.boot:spring-boot-gradle-plugin:${project.extra["springBootVersion"]}")
    }
}
```
ext->extra  
extra->project.extra

## `mavenCentral` vs. `jcenter`
`jcenter` has been shutdown.
[JCenter shutdown](https://blog.gradle.org/jcenter-shutdown)

use `mavenCentral`.

## use implementation rather than compile
`compile` is deprecated. Should use `implementation`.
- `compile` to `implementation`
- `testCompile` to `testImplementation`
- `debugCompile` to `debugImplementation`
- `androidTestCompile` to `androidTestImplementation`

[doc](https://docs.gradle.org/current/userguide/dependency_management_for_java_projects.html#sec:configurations_java_tutorial)

[stack overflow](https://stackoverflow.com/questions/44493378/whats-the-difference-between-implementation-api-and-compile-in-gradle)
