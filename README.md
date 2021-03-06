# GarbageCollector
GC에 대하여

# 가비지 콜렉터(Garbage Collector)란?
* 가비지 콜렉터를 알아보기 전에, 여기서 말하는 가비지가 무엇인지부터 알아보자. 가비지는 '정리되지 않은 메모리', '유효하지 않은 메모리 주소'를 말한다. 다음 코드를 통해 살펴보자

````java
String[] array = new String[2];
array[0] = '0';
array[1] = '1';
array = new String[] {'G', 'C' };
````

* 위 코드에서 String 배열이 할당되기 전에 할당한 0과 1은 어디로 갔을까? 이렇게 주소를 잃어버려서 사용할 수 없는 메모리가 '정리되지 않은 메모리'이다. 프로그래밍 언어에서는 Danling Object, 자바에서는 Garbage라고 부른다.
* 추가로 앞으로 사용하지 않고 메모리를 가지고 있는 객체 역시 Garbage에 포함된다.
* 가비지는 메모리가 부족할 때 이런 가비지들을 메모리에서 해제 시켜 다른 용도로 사용 할 수 있게 해주는 프로그램을 말한다.
* C++와 같은 다른 언어에서는 사용하지 않을 객체의 메모리를 직접 해제해주어야 하지만 자바는 GC가 잡아주니 개발자 입장에서는 편리하다. 다만 모든 메모리 누수를 잡아주는 것은 아님으로 메모리 누수에 대한 경계를 늦추어서는 안된다.

### Stop The World
* Stop-the-world는 GC 실행을 위해 JVM이 애플리케이션 실행을 멈주는 것이다. GC가 실행 될 때는, GC를 실행하는 쓰레드를 제외한 모든 스레드들이 작업을 멈춘다. GC 작업이 완료한 이후에야 중단했던 작업을 다시 시작한다. 대개의 경우 GC 튜닝이란 이 stop-the-world 시간을 줄이는 것을 말한다.

### GC 과정
* GC의 과정을 Mark and Sweep이라고도 한다. GC가 스택의 모든 변수 또는 Reachable 객체를 스캔하면서 각각 어떤 객체를 참조하고 있는지 찾는 과정이 Mark라고 한다. 이 과정에서 Stop the world가 발생한다. 이후 Mark 되어있지 않은 객체들을 힙에서 제거하는 과정이 Sweep이다.

### Minor GC 와 Major GC
* JVM의 Heap은 Young, Old, Perm 세 영역으로 나뉜다. Young 영역에서 발생한 GC를 Minor GC, 나머지 두 영역에서 발생한 GC를 Major GC(Full GC)라고 한다.
  - Young 영역 : 새롭게 생성한 객체가 위치, 대부분의 객체가 금방 unreachable 상태가 되기 때문에 많은 객체가 Young 영역에 생성되었다가 사라진다.
  - Old 영역 : Young 영역에서 reachable 상태를 유지해 살아남은 객체가 여기로 복사된다. 대부분 Young 영역보다 크게 할당하며, 크기가 큰 만큼 Young 영역보다 GC는 적게 발생한다.
  - Perm 영역 : Method Area라고도 한다. 클래스와 메소드 정보와 같이 자바 언어 레벨에서는 거의 사용되지 않는 영역이다.
  
출처: https://velog.io/@litien/%EA%B0%80%EB%B9%84%EC%A7%80-%EC%BB%AC%EB%A0%89%ED%84%B0GC

