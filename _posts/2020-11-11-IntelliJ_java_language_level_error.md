# IntelliJ Java Language Level 설정 에러

스프링 강의를 듣던 중, 분명히 예제 코드와 동일하게 했고 인텔리제이에서 JDK 1.8을 쓰도록 설정했는데 에러가 발생했다.

```java
@OneToMany
@JoinColumn(name="TEAM_ID")
private List<Member> members = new ArrayList<>(); // 이 부분

// 에러 문구
Error:(18,50) java: diamond operator is not supported in -source 1.5
```

```java
private List<Member> members = new ArrayList<Member>(); // 이 부분
```

1.5버젼에서 지원하지 않으니, 뒤의 제네릭에 이렇게 자료형을 넣어주라는 얘기였다.<br>
확인해보니 pom.xml, persistence.xml 모두 java 버젼 설정에 대한 내용은 없었고,<br>
인텔리제이 내부에서도 JDK 1.8을 설정했다.<br>

근데 계속해서
<img src="https://github.com/Ting-Kim/Ting-kim.github.io/blob/main/images/intellij_java_language_level_error_1.PNG?raw=true" style="width:600px"/>
<img src="https://github.com/Ting-Kim/Ting-kim.github.io/blob/main/images/intellij_java_language_level_error_2.PNG?raw=true" style="width:600px"/>

이 부분들이 1.5 버젼으로 바뀌는 걸 확인했다.<br>
몇시간 동안 해결방법을 찾고 조치해도 저절로 다시 1.5로 설정되어서 답답한 상태였는데..<br>
(해결 되는 것 같다가도 갑자기 인코딩에러가 뜨고, 컴파일 에러 뜨고 이것저것 난리치다가 결국 다시 버전 에러로 회귀...)

maven 프로젝트 생성 시 딱히 default로 버전을 설정한 기억도 없어서, maven 설정을 넣어보기로 했다.<br>

```xml
<properties>
       <maven.compiler.source>1.8</maven.compiler.source>
       <maven.compiler.target>1.8</maven.compiler.target>
</properties>
```

<br>
결론) 이렇게 추가했더니 해결 됐다..
