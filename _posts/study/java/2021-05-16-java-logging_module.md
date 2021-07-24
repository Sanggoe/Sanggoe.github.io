---
layout: post
title:  "[Java] Logging Module"
subtitle:   "log4j, logback, log4j2"
date: 2021-05-16 00:22:01 +0900
categories: study
tags: java
comments: true
---
# Logging Module

## log4j

* 프로그램을 작성하면서 **로그를 남기기 위해 사용되는 자바 기반의 logging utility** 이다.
* log4j는 Log for Java 라는 의미를 줄여놓은 것으로, 디버그용 도구로 사용한다.
* 더이상 System.out.println 을 사용할 필요 없이, 개발자들만의 로깅 방식를 통일하는 방법이 될 수 있다.
* 하지만, log4j는 15년에 더이상 개발을 중단한다는 소식을 발표한 Apache의 꽤 오래 된 프레임워크이다.
* 따라서 기존의 시스템이 아닌 이상 이제는 거의 사용하지 않는다고 한다.
  * 아래에서 나올 logback이나 log4j2를...

<br/>

### Logging Level?

* 다음과 같이 로그의 레벨을 설정하여 수준에 따른 로그를 출력하도록 할 수 있다.
* 로깅 레벨을 설정한다면, 그 이상의 레벨에 해당하는 로그만 출력하게 된다.
* 예를 들어 ERROR를 설정하면, FATAL과 ERROR만 출력되고 그 이하의 WARN, INFO, DEBUG 등의 수준은 출력되지 않는다.

<br/>

### log4j의 logging level

* FATAL : 가장 치명적인 에러 수준
* ERROR : 일반적인 에러 수준
* WARN : 에러는 아니지만, 주의가 필요한 수준
* INFO : 일반적인 정보의 수준
* DEBUG : 일반적인 정보를 상세하게 나타내는 수준
* TRACE : debug 보다 좀 더 구체적인 정보를 나타내는 수준

<br/>

### slf4j?

* slf4j는 logging에 대한 추상체를 제공한다.
* 자바 입장에서 보면, interface 집합체라고 볼 수 있다.
* 구현체를 손쉽게 교체할 수 있도록 도와주는 프레임워크이다.
* 실제 로그를 어떤 구현체로 출력할건지를 선택하기 위해 logback 또는 log4j2를 사용하는 것이다.

<br/>

## logback?

* log4j와 유사하고 여러 기능들을 추가시켜 성능을 향상시킨 버전이다.
* slf4j도 지원하며, 필터링 옵션과 자동 reload도 가능하다.

<br/>

### logback의 logging level

* log4j와 다르게, 5단계로 구성된다.
* ERROR, WARN, INFO, DEBUG, TRACE 가 존재한다.

<br/>

## log4j2?

* 앞선 log4j와 logback과 비교하면, 가장 최신 버전의 기술이다.
* logback과 동일하게 자동 reload 기능과 필토링 옵션을 제공한다.
* logback의 아키텍처에서 발생하는 문제점을 수정하였고, 멀티 스레드 환경에서 비동기 로거의 경우 몇 배의 처리량을 보이는 등 조금 더 빠르다고 한다.
* 람다 표현식, 사용자 정의 로그 레벨도 지원한다.

<br/>

### log4j2 사용방법

<br/>

#### 1. 먼저 의존성을 추가해준다.

* 기본적으로 스프링은 slf4j 로깅 프레임워크를 사용한다.
* Gradle 프로젝트의 경우 build.gradle에 dependencies를 추가해줘야 한다.
* Maven 프로젝트의 경우 pom.xml에 dependencies를 추가해줘야 한다.
* 다음은 스프링 부트에서 log4j2를 추가한 코드 예시

```groovy
dependencies {
    ...
    implementation 'org.springframework.boot:spring-boot-starter-log4j2'
    testImplementation 'org.springframework.boot:spring-boot-starter-log4j2'
    implementation 'org.apache.logging.log4j:log4j-web:2.12.1'
	...
    }
}
```

* log4j

<br/>

2. 설정파일도 필요하다.
   * log4j2에서는 Json, xml, yml 등 여러 가지 방식으로 사용할 수 있다.
   * 태그 중 logger의 name에 path를 입력하면 해당 경로의 파일은 입력한 level 수준으로 출력하도록 설정.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Configuration status="INFO">
    <Appenders>
        <Console name="Console" target="SYSTEM_OUT">
            <PatternLayout pattern="[pattern]"/>
        </Console>
    </Appenders>
    <Loggers>
        <Root level="debug">
            <AppenderRef ref="Console"/>
        </Root>
        <logger name="[path1]" level="info"/>
        <logger name="[path2]" level="info"/>
        <logger name="[path3]" level="info"/>
        <logger name="[path4]" level="error" additivity="false">
            <AppenderRef ref="Console"/>
        </logger>
    </Loggers>

</Configuration>
```

<br/>

3. 실제 사용하는 코드 예시는 이렇다.
   * 설정한 레벨 이상에 해당하는 로그만 출력하게 된다.

```java
log.trace("trace level");
log.debug("debug level");
log.info("info level");
log.warn("warn level");
log.error("error level");
```

<br/>

[log4j, logback, log4j2](https://madplay.github.io/post/log4j-logback-log4j2)

[log4j 및 logback](https://goddaehee.tistory.com/45)

[log4j2 제대로 사용하기](https://velog.io/@bread_dd/Log4j-2-%EC%A0%9C%EB%8C%80%EB%A1%9C-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0)

[slf4j](https://inyl.github.io/programming/2017/05/05/slf4j.html)

[slf4j와 log4j2 연동하기](http://dveamer.github.io/java/SLF4J-Log4J2.html)

[스프링 부트에 log4j2 적용하기](https://twofootdog.tistory.com/52)