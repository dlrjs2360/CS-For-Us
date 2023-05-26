## 🐦 JNI (Java Native Interface)

Java 코드를 C, C++, 어셈블리어와 같은 다른 프로그래밍 언어로 작성된 애플리케이션 및 라이브러리와 상호 운용할 수 있게 만들어진 기본 프로그래밍 인터페이스다.

Java만으로 응용 프로그램의 요구 사항을 충족하지 못할 경우, JNI를 사용하여 Java Native Method를 작성할 수 있다.

예를 들어,
- 표준 Java 클래스 라이브러리가 애플리케이션에 필요한 플랫폼 종속 기능을 지원하지 않을 때
- 다른 언어로 작성된 라이브러리가 이미 있고, JNI를 통해 Java 코드에 액세스할 수 있도록 만들고 싶을 때
- 어셈블리어와 같은 저수준 언어로 시간이 중요한 코드의 작은 부분을 구현하려고 할 때
이런 경우에 사용할 수 있다.

이를 통해 Java 객체 생성 및 업데이트, Java 메서드 호출, 예외 Catch & Throw, 클래스 로드 등의 작업을 수행할 수 있다.

---

### 참고 자료

> https://docs.oracle.com/javase/7/docs/technotes/guides/jni/spec/intro.html#wp16684