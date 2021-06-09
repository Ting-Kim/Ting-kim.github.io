---
layout: post
title: "[Spring Boot]멀티 모듈 프로젝트 구성 방법"
tags: [Backend, SpringBoot, MultiModule]
comments: true
---

### Spring Boot 멀티 모듈 프로젝트 설정(root, 각 모듈 build.gradle 설정)

root 프로젝트의 `build.gradle` (소스 출처 : [https://daddyprogrammer.org/post/13156/spring-boot-change-multi-module/](https://daddyprogrammer.org/post/13156/spring-boot-change-multi-module/))

```
buildscript {
    ext {
        springBootVersion = '2.1.4.RELEASE'
    }
    repositories {
        mavenCentral()
    }

    dependencies {
        classpath("org.springframework.boot:spring-boot-gradle-plugin:${springBootVersion}")
        classpath "io.spring.gradle:dependency-management-plugin:0.6.0.RELEASE"
    }
}

subprojects {
    apply plugin: 'java'
    apply plugin: 'org.springframework.boot'
    apply plugin: 'io.spring.dependency-management'

    sourceCompatibility = 1.8

    repositories {
        mavenCentral()
    }

    configurations {
        compileOnly {
            extendsFrom annotationProcessor
        }
    }

    // 모든 모듈에서 사용하는 라이브러리
    dependencies {
        implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
        implementation 'org.springframework.boot:spring-boot-starter-freemarker'
        implementation 'org.springframework.boot:spring-boot-starter-web'
        implementation 'org.springframework.boot:spring-boot-starter-actuator'
        implementation 'org.springframework.boot:spring-boot-starter-data-redis'
        //embedded-redis
        implementation 'it.ozimov:embedded-redis:0.7.2'
        implementation 'io.springfox:springfox-swagger2:2.6.1'
        implementation 'io.springfox:springfox-swagger-ui:2.6.1'
        implementation 'net.rakugakibox.util:yaml-resource-bundle:1.1'
        implementation 'com.google.code.gson:gson'
        compileOnly 'org.projectlombok:lombok'
        runtimeOnly 'com.h2database:h2'
        runtimeOnly 'mysql:mysql-connector-java'
        annotationProcessor 'org.projectlombok:lombok'
        testImplementation 'org.springframework.security:spring-security-test'
        testImplementation 'org.springframework.boot:spring-boot-starter-test'
    }
}

project(':module-common') {
    // common 모듈에만 필요한 라이브러리가 발생하면 이곳에 추가한다.
    dependencies {
        implementation 'org.springframework.boot:spring-boot-starter-security'
        implementation 'io.jsonwebtoken:jjwt:0.9.1'
    }
}

project(':module-user') {
    // user 모듈에만 필요한 라이브러리가 발생하면 이곳에 추가한다.
    dependencies {
        compile project(':module-common')
    }
}

project(':module-board') {
    // board 모듈에만 필요한 라이브러리가 발생하면 이곳에 추가한다.
    dependencies {
        compile project(':module-common')
    }
}
```

멀티모듈로 만들어진 프로젝트들은 각각 독립적인 프로세스를 가진다.

- `dependencies{}` 에 `compile project()` 포함한 경우는, 포함한 프로젝트에 대한 코드(`application.yml` 포함)를 **함께 컴파일**하는 것 같다.
  - 위의 `build.gradle`를 예시로 들면, `module-user` 모듈을 구동할 때
    - `module-common` 모듈의 코드는 함께 실행되고, `module-board`는 실행 되지 않는다.
- `root-build.gradle`의 `dependencies{}`에 포함되는 **라이브러리**는 모든 모듈 내부 코드에서 사용가능하고, `module-common`의 `dependencies{}`에 포함되는 라이브러리는 `module-common`모듈 내 코드에서만 사용 가능하다!

**추가로 알게 된 것..**

- [**Gradle 에서 compile과 implementation의 차이점**](https://bluayer.com/13)
  - **compile**은 모듈과 **직접, 간접적으로 의존하고 있는 모든 라이브러리나 모듈을 재빌드**하며, 연결된 **모든 모듈의 API가 `exposed(노출)`** 된다.
  - **implementation**은 모듈을 **직접 의존하고있는 모듈만을 재빌드**하며, **연결된 API는 노출되지 않는다. (`not exposed`)**

---

**Reference**

- [스프링 부트 어드민 사용해보기](https://andole98.github.io/spring/spring-boot-admin/#)
- [Spring Boot | 멀티 모듈 프로젝트 (Multi Module)](https://gaemi606.tistory.com/entry/Spring-Boot-%EB%A9%80%ED%8B%B0-%EB%AA%A8%EB%93%88-%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8-Multi-Module)
- [스프링 부트로 멀티모듈 셋팅하기](https://taetaetae.github.io/2020/01/19/spring-boot-maven-multi-module/)
- [Spring Boot - Intellij Gradle Multi module](https://daddyprogrammer.org/post/13156/spring-boot-change-multi-module/)
- [멀티모듈 설계 이야기 with Spring, Gradle](https://woowabros.github.io/study/2019/07/01/multi-module.html)
