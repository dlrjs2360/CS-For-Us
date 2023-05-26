# 🦜 JVM의 동작 방식과 구조

**키워드**
> 자바 컴파일러, 클래스 로더, 런타임 데이터 영역, 실행 엔진, 가비지 컬렉터, 인터프리터, JIT 컴파일러
---

## 🦜 1. 동작 방식

1. 소스 코드(.java)를 **자바 컴파일러(javac)** 를 통해 **바이트 코드(.class)** 로 컴파일한다.


2. 바이트 코드를 JVM의 **클래스 로더**에게 전달하면, 클래스 로더는 **런타임 데이터 영역(Runtime Data Area)** 으로 바이트 코드를 로딩한다.


3. **실행 엔진(Execution Engine)** 에서 런타임 데이터 영역에 로딩 된 바이트 코드를 하나씩 가져와서 해석하고 실행한다.


JVM의 구조를 보면서 더 자세히 살펴보겠다.

--- 

## 🦜 2. 구조

<img src="https://wikidocs.net/images/page/102803/jvm.png">


런타임 데이터 영역, 클래스 로더, 가비지 컬렉터, 실행 엔진으로 이루어져있다.

하나씩 살펴보자!


### 🦜 2.1 런타임 데이터 영역(Runtime Data Area)

JVM이 운영체제에게 할당 받은 메모리 영역으로, 데이터 저장 영역이다. 메서드 영역, 힙 영역, 스택 영역, PC 레지스터, 네이티브 메서드 스택으로 이루어져있다.

#### 2.1.1 메서드 영역(스태틱 영역)
- 모든 스레드가 공유하는 영역, 클래스를 위한 공간
- JVM 시작 시 생성된다.
- 필드 및 메서드 데이터, 클래스 멤버 변수의 이름, 런타임 상수 풀(Runtime Constant Pool), final class 변수 등이 생성된다.
  - **런타임 상수 풀**: 심볼 테이블(변수 목록 테이블) 기능을 하며, 각 클래스, 인터페이스의 메서드와 필드에 대한 실제 메모리 상의 주소를 참조할 수 있다.

#### 2.1.2 힙 영역(Heap Area)
- 객체를 위한 공간. new 키워드로 생성된 객체와 배열이 생성되는 영역이다.
- 가비지 컬렉터에 의해 관리 된다.


#### 2.1.3 스택 영역(Stack Area)
- 지역 변수, 파라미터, 리턴 값, 연산에 사용되는 임시 값 등 생성
- 스택 프레임(Stack Frame)으로 할당된다.

해당 메모리 구조는 중요하기 때문에 더 자세히 공부해보도록 하겠다.

[JVM과 메모리 구조에 대해 더 알아보기](링크)


#### 2.1.4 PC 레지스터
현재 수행 중인 명령의 주소를 가진다. 각 스레드마다 하나씩 존재하며, 스레드가 시작될 때 생성된다.

#### 2.1.5 Native Method Stack
JNI(Java Natice Interface)를 통해 호출한 JAVA 외의 언어로 작성된 코드를 수행하기 위한 스택이다.

[JNI는 뭐야! 라고 생각했다면](링크)


---

### 🦜 2.2 클래스 로더 (Class Loader)

자바는 동적으로 클래스를 읽어 오기 때문에, 런타임 중에 코드가 JVM과 연결된다. 즉 실행 시에 필요한 클래스를 동적으로 로드하여 사용할 수 있다. 이렇게 동적으로 클래스를 런타임 데이터 영역으로 로딩해주는 역할을 해주는 것이 클래스 로더다.

#### 2.2.1 계층구조

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Ft1.daumcdn.net%2Fcfile%2Ftistory%2F99DBA04D5B73A37321">


- **Bootstrap Class Loader**
  - 최상위 클래스 로더
  - 네이티브 코드(프로세서가 실행하는 기계어 코드)로 구현 되어 있다.
  - java.lang 패키지의 클래스 등 JVM 자체를 실행하는 데 필요한 클래스를 로드한다.


- **Platform Class Loader(Extension Class Loader)**
  - 자바로 구현 되어 있다.
  - Java 9부터는 모듈 시스템 도입으로 인해 Platform Class Loader로 명칭이 바뀌었다.
  - 자바에서 기본적으로 제공해주는 클래스를 로딩한다.
  - URL Class Loader 타입이다.
  - JVM의 확장 디렉토리(lib/ext)의 확장 파일을 로딩해 새로운 기능이나 확장된 라이브러리를 제공한다.


- **System Class Loader**
  - 자바로 구현 되어 있다.
  - URL Class Loader 타입이다.
  - 유저가 작성한 클래스를 로딩한다.

<br>

또한 클래스 로더는 몇가지 특징(원칙)이 존재한다.

#### 2.2.2 가시성 원칙(Visibility)
하위 클래스로더는 상위 클래스로더가 로드한 클래스를 볼 수 있지만, 상위 클래스로더는 하위 클래스로더가 로드한 클래스를 볼 수 없다.

#### 2.2.3 유일성 원칙(Uniqueness)
상위 클래스로더가 로드한 클래스를 하위 클래스로더가 다시 로드하지 않아야 하며, 이미 로드한 클래스를 다시 로드해서는 안된다. 즉 클래스는 한 번만 로드할 수 있다.

#### 2.2.4 언로딩 금지
클래스로더는 클래스를 로드할 수는 있지만, 이미 로드한 클래스를 언로드(Unload)할 수는 없다.

#### 2.2.5 위임 원칙(Delegation)
클래스로더는 클래스 로딩 요청을 받았을 때, 상위 클래스 로더에게 책임을 위임한다.

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FePoEHO%2FbtrCcFsXQeU%2FPJNuytE1DvZEwOyHa7UEW0%2Fimg.png">



---
### 🦜 2.3 가비지 컬렉터 (Garbage Collector, GC)
내용이 길어 다른 문서로 이사갔다.

자세한 내용은 [가비지 컬렉션(GC) 공부하러 가기](링크) 에서 알아보자!

---

### 🦜 2.4 실행 엔진 (Execution Engine)

클래스 파일을 실행시키는 방법을 두 가지로 분류할 수 있다.

- **인터프리터**: 기본적으로 바이트 코드 명령어를 하나씩 읽어서 해석하고 실행한다.

그러나 인터프리터 방식은 전체적인 실행 속도가 느리기 때문에, 이를 보완하기 위해 JIT 컴파일러가 도입되었다.


- **JIT(Just In Time) 컴파일러**: 자주 실행되는 바이트 코드를 런타임 중에 기계어로 컴파일한다.

<img src="https://velog.velcdn.com/images/kimunche/post/96589981-7eb7-452a-882d-cf6f69185af5/image.png">



[+) 인터프리터 언어 vs 컴파일러 언어의 차이점에 대해 알아보기](링크)


---

### 참고 자료

> [[Java] 자바 JVM 내부 구조와 메모리 구조에 대하여](https://coding-factory.tistory.com/828)

> [[JAVA] JVM 동작원리 및 기본개념](https://steady-snail.tistory.com/67)

> [JVM 구조와 JAVA의 동작 원리](https://velog.io/@sgwon1996/JAVA%EC%9D%98-%EB%8F%99%EC%9E%91-%EC%9B%90%EB%A6%AC%EC%99%80-JVM-%EA%B5%AC%EC%A1%B0)

> 자바 헤엄치기[기초부터 알고리즘까지](https://wikidocs.net/102803)

> The Java® Virtual Machine Specification

> [Java] ClassLoader 구조 및 동작 원리(https://wpioneer.tistory.com/255)

> [JVM. 클래스로더 서브시스템(Class Loader Subsystem)](https://blog.hexabrain.net/397)

> [JVM 실행 엔진(Execution Engine)](https://junhyunny.github.io/information/java/jvm-execution-engine/)
