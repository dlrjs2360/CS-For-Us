# 🥦 JDK, JRE, JVM

---

현실에 빗대어 살펴보면 이해하기 쉽다.

| 현실에서는        | 자바에서는                                |
|--------------|--------------------------------------|
| 소프트웨어 개발 도구  | JDK(자바 개발 도구)<br/>: JVM용 소프트웨어 개발 도구 |
| 운영체제         | JRE(자바 실행 환경) <br/>: JVM용 운영체제       |
| 하드웨어(물리적 컴퓨터) | JVM(자바 가상 기계)<br/>: 가상의 컴퓨터          |

JDK를 이용해 프로그램을 개발하고, JRE에 의해 JVM 상에서 프로그램이 구동된다. JDK는 JRE를, JRE는 JDK를 포함한다.

> JDK ⊃ JRE ⊃ JVM

<BR>

### 🥦 자바 개발 도구 (JDK, Java Development Kit)
- 프로그램을 개발할 때 필요하다.
- 자바 소스 컴파일러(javac.exe)를 포함한다.

<br>

### 🥦 자바 실행 환경 (JRE, Java Runtime Environment)
- 개발한 자바 프로그램을 실행할 수 있다.
- 자바 프로그램 실행기(java.exe)를 포함한다.

<br>

### 🥦 자바 가상 머신 (JVM, Java Virtual Machine)

다른 언어들의 경우, 플랫폼에 따라 프로그램을 다르게 컴파일한다.
- CPU에 따라 사용하는 기계어 종류가 다르고,
- 운영체제마다 사용하는 API, 실행 파일 형식, 메모리 관리 방식이 다르기 때문

그런데 자바 프로그램은 JVM을 통해 **플랫폼에 관계 없이** 실행할 수 있다.

동작하는 과정을 간단하게 보면 이런 흐름이다.

> 자바 소스 파일(.java 파일)
>
> → [컴파일러] 에서 컴파일 →
>
> 자바 목적 파일(바이트 코드, .class 파일)
>
> → 1) [맥 OS용 JRE 위에서 돌아가는 JVM]에서는 맥 OS에 맞는 실행 파일 생성
>
> → 2) [윈도우용 JRE 위에서 돌아가는 JVM]에서는 윈도우에 맞는 실행 파일 생성

기종이 몇 개든 하나의 소스 파일(코드), 하나의 목적 파일(JVM용 기계어), 하나의 컴파일러만 필요하다. 기종별로 JRE만 세팅되어 있으면 JVM이 바이트 코드를 각 운영체제에 맞는 실행 파일로 바꿔준다.
따라서 자바는 이식성이 높으며, 'Write Once Run Anywhere'인 언어다.

<br>
+) C언어의 경우, 소스 코드를 작성하면 윈도우용 컴파일러, 리눅스용 컴파일러가 필요하다.


<BR>


[JVM의 자세한 구조 알아보기](https://github.com/dlrjs2360/CS-For-Us/blob/master/Java/JVM%EC%9D%98%20%EB%8F%99%EC%9E%91%20%EB%B0%A9%EC%8B%9D%EA%B3%BC%20%EA%B5%AC%EC%A1%B0.md)

---

### 참고 자료
> [스프링 입문을 위한 자바 객체 지향의 원리], 김종민, 위키북스, 2015

> https://coding-factory.tistory.com/827
