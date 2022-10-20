# ❗ Querydsl 설정 방법

### Version

```
- Querydsl 5.0
- Spring boot 2.7.3
```

### 1. build.gradle 설정

```bash

plugins {
    id "com.ewerk.gradle.plugins.querydsl" version "1.0.10"
}


dependencies {

    // querydsl
    implementation "com.querydsl:querydsl-jpa:5.0.0"
    implementation "com.querydsl:querydsl-apt:5.0.0"
}


// Qtype 생성 경로
def querydslDir = "$buildDir/generated/querydsl"
querydsl {
    jpa = true
    querydslSourcesDir = querydslDir
}
sourceSets {
    main.java.srcDir querydslDir
}
compileQuerydsl{
    options.annotationProcessorPath = configurations.querydsl
}
configurations {
    compileOnly {
        extendsFrom annotationProcessor
    }
    querydsl.extendsFrom compileClasspath
}
```
