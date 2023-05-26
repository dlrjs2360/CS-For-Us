# 🧹 가비지 컬렉션 (Garbage Collection, GC)

자바의 메모리 관리 방법 중 하나로, 힙 영역에서 동적으로 할당했던 메모리 영역 중 **불필요한 객체들을 주기적으로 삭제**하는 프로세스를 말한다. 메모리 영역 청소기다.

다만 가비지 컬렉션은 단점이 존재한다.
- 메모리가 언제 해제되는지 정확하게 알 수 없는 점
- GC가 동작하는 동안 다른 동작을 멈춰서 오버헤드가 발생한다는 점

이를 **STW(Stop The World)** 라고 한다. 이름에서 유추할 수 있듯 GC를 수행하기 위해 JVM이 애플리케이션 실행을 멈추는 현상을 뜻하며, GC를 실행하는 스레드를 제외한 모든 스레드가 멈추게 된다.

따라서 이 시간을 줄이는 것이 중요하기 때문에 GC 튜닝을 수행한다.
이제 GC에 대해 본격적으로 알아보자!

---

## 🧹 1. GC의 대상

가비지 컬렉터는  어떤 객체가 수레기인지 판단하기 위해 **Reachable, Unreachable**이라는 상태를 사용한다.

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbW5c5r%2FbtrvAb4nrdH%2FlYHuQZya8ECvEndRkQchjk%2Fimg.png">

이 사진에서 파란색은 Reachable, 빨간색은 Unreachable하다고 본다. 힙 영역의 객체를 참조하는 변수가 사라지게 될 경우, 누구도 참조하고 있지 않은 객체(빨간색)가 생기게 된다. 이렇게 덩그러니 남은 객체들을 가비지 컬렉터가 제거해준다.

---

## 🧹 3. Mark And Sweep 알고리즘

이제 GC가 어떻게 청소를 하는지 알아볼 차례다.
GC 알고리즘인 Mark And Sweep의 과정으로는 크게 두 가지, Mark와 Sweep이 있다.

- **Mark**: 뭐를 청소할지 알아보기
- **Sweep**: 청소하기

이런 느낌이며, 가비지 컬렉터의 종류에 따라서는
- **Compact**: 분산된 객체 모으기

과정이 있는 경우도 있다.

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbGghBW%2FbtrvvDgIHRO%2FHxoX3w9skgah3xFVhfEgD0%2Fimg.png">



### Mark
GC Root에서부터 연결된 객체를 찾아내어 각각 어떤 객체를 참조하고있는지 찾아내서 마킹한다.
    - 루트가 참조하는 객체 -> 그 객체가 참조하는 객체들 ... 그래프 순회로 탐색

### Sweep
누군가가 참조하지 않는(Unreachable) 객체를 제거한다.

### Compact
Sweep 후 분산된 객체들을 힙 영역의 시작 주소로 모아 압축한다.


참고로 GC Root는 외부에서 힙에 대한 레퍼런스를 갖고 있는 변수나 오브젝트를 말한다. 실행 중인 스레드, 정적 변수, 로컬 변수, JNI 레퍼런스 등이 해당될 수 있다.

[JNI는 뭐야! 라고 생각했다면](링크)

---

## 🧹 4. 힙 영역의 구조

GC의 동작 과정을 자세히 살펴보기 전에 먼저 힙 영역의 구조에 대해 알아봐야 한다.
힙 영역은

- 대부분의 객체는 금방 접근 불가능한 상태가 된다.
- 오래된 객체에서 새로운 객체로의 참조는 아주 적게 존재한다.

이 두 가지 전제를 바탕으로 설계되었다.
따라서 효율적인 메모리 관리를 위해 **Young 영역, Old 영역**으로 나뉘게 되었다. 이 때 Young 영역은 다시 **Eden, Survival 0, Survival 1**로 나뉜다.

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fbti1oP%2FbtrvtcdoBC9%2FupBBOdB4mJF6tfyhL8GPbK%2Fimg.png">

- **Young 영역**: 새롭게 생성된 객체가 할당된다.
  - 대부분 금방 사라지기 때문에 영역이 작다.
  - Young에 대한 GC를 Minor GC라고 부른다.

* **Eden:** new를 통해 새로 생성된 객체가 위치한다.
* **Survivor 0, Survivor 1:** 최소 1번 이상의 GC에서 살아남은 객체가 존재하는 영역
  * 둘 중 하나는 꼭 비어 있어야 한다는 규칙이 있다.

- **Old 영역:** 살아남은 객체가 복사되는 영역
  - Young 영역보다 크게 할당된다.
  - Old 영역에 대한 GC를 Major GC(Full GC)라고 부른다.

+) 이외에도 **Permanent 영역**이 있었지만, Java 8부터 Native Method Stack에 편입되면서 사라졌다.


---

## 🧹 5. GC 동작 과정

이제 본격적으로 GC 동작 과정을 자세히 살펴보도록 하자. 앞선 설명에서 Young에서는 Minor GC, Old에서는 Major GC가 일어난다고 했다.

### 🧹 5.1 Minor GC
처음 생성된 객체는 Eden 영역에 위치하는데, 객체가 계속 생성되어 Eden 영역이 꽉차면 Minor GC가 실행된다. 실행 속도는 빠르다.

1. Mark 단계에서 Reachable 객체 탐색
2. Reachable한 객체는 1개의 Survivor 영역으로 이동
3. UnReachable한 객체들의 메모리 해제(Sweep)
4. 살아남은 객체들의 age 값이 1씩 증가
   - **age 값**: 객체가 살아남은 횟수를 의미하는 값이며 임계값에 다다를 경우 _Old 영역으로 이동(Promotion이라고 한다.)_ 될 수 있다.
5. 반복🗑️🗑️
   - 이 때 1개의 Survivor 영역이 가득 차면 다른 Survivor 영역으로 이동시킨다. 따라서 둘 중 하나는 반드시 비어있게 된다.


### 🧹 5.2 Major GC

객체들이 계속 Promotion 되어 Old 영역이 꽉 차면 Major GC가 발생한다.

Young 영역보다 크기가 크기 때문에 시간이 오래 걸리므로, 앞서 설명했던 Stop The World 문제가 발생할 수 있다.


---

## 🧹 6. GC 알고리즘 종류 (후추추)

### 6.1 Serial GC
- 가장 단순한 GC로, 서버의 CPU 코어가 1개일 때 사용했다.
- 싱글 스레드 방식이므로 STW 시간이 길다.
- Minor GC에 Mark-Sweep, Major GC에 Mark-Sweep-Compact를 사용한다.


### 6.2 Parallel GC
- Serial GC와 비슷하지만, Minor GC를 멀티 스레드로 수행한다는 차이점이 있다.
- Serial GC에 비해 STW 시간이 감소했다.


### 6.3 CMS GC

### 6.4 G1 GC

### 6.5 Shenandoah GC

### 6.6 ZGC

### 6.7 Epsilon GC






---

### 참고 자료
> [가비지 컬렉션 동작 원리 & GC 종류 💯 총정리](https://inpa.tistory.com/entry/JAVA-%E2%98%95-%EA%B0%80%EB%B9%84%EC%A7%80-%EC%BB%AC%EB%A0%89%EC%85%98GC-%EB%8F%99%EC%9E%91-%EC%9B%90%EB%A6%AC-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%F0%9F%92%AF-%EC%B4%9D%EC%A0%95%EB%A6%AC#garbage_collectiongc_%EC%9D%B4%EB%9E%80?)

> [[Java] 가비지 컬렉션(GC, Garbage Collection) 총정리](https://coding-factory.tistory.com/829)

