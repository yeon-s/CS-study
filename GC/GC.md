# GC

JAVA의 가비지 컬렉터는 그 동작 방식에 따라 매우 다양한 종류가 있지만 공통적으로 크게 다음 2가지 작업을 수행한다고 볼 수 있다.

- 힙 내의 객체 중 가비지를 찾아낸다.
- 찾아낸 가비지를 처리해서 힙의 메모리를 회수한다.

최초의 JAVA에서는 이들 가비지 컬렉션 작업에 애플리케이션의 사용자 코드가 관여하지 않도록 구현되어 있었다. 그러나 위 2가지 작업에서 좀 더 다양한 방법으로 객체를 처리하려는 요구가 있었다. 이에 따라 JDK 1.2부터는 java.lang.ref 패키지를 추가해 제한적이나마 사용자 코드와 GC가 상호작용할 수 있게 되었다.

![Untitled](/image/gc/Untitled.png)

## Minor GC

새로 생성된 대부분의 객체는 Eden 영역에 위치한다. Eden 영역에서 GC가 한번 발생한 후 살아남은 객체는 Survivor 영역 중 하나로 이동된다. 이 과정을 반복하다가 계속해서 살아남아 있는 객체는 일정시간 참조되고 있다는 뜻이므로 Old 영역으로 이동시킨다.

## Major GC

Old영역에 있는 모든 객체들을 검사하여 참조되지 않은 객체들을 한꺼번에 삭제한다. 시간이 오래 걸리고 실행 중 프로세스가 정지된다. 이것을 ‘stop-the-world’라고 하는데 Major GC가 발생하면 GC를 실행하는 스레드를 제외한 나머지 스레드는 모두 작업을 멈춘다. GC 작업을 완료한 이후에야 중단했던 작업을 다시 시작한다.

### 가비지 컬렉션은 어떤 원리로 소멸시킬 대상을 선정하는가?

알고리즘에 따라 동작 방식이 매우 다양하지만 공통적인 원리가 있다. 가비지 컬렉터는 힙 내의 객체 중에서 가비지를 찾아내고 찾아낸 가비지를 처리해서 힙의 메모리를 회수한다. 참조되고 있지 않은 객체를 가비지라고 하며 객체가 가비지인지 아닌지 판단하기 위해서 reachability라는 개념을 사용한다. 어떤 힙 영역에 할당된 객체가 유효한 참조가 있으면 reachability, 없다면 unreachability로 판단한다. 하나의 객체는 다른 객체를 참조하고, 다른 객체는 또 다른 객체를 참조할 수 있기 때문에 참조 사슬이 형성이 되는데, 이 참조 사슬 중 최초에 참조한 것을 **Root Set**이라고 칭한다. 힙 영역에 있는 객체들은 총 4가지 경우에 대한 참조를 하게 된다.

1 .힙 내의 다른 객체에 의한 참조

2. JAVA 스택, 즉 JAVA 메소드 실행 시에 사용하는 지역변수와 파라미터들에 의한 참조

3. 네이티브 스택에 의해 생성된 객체에 대한 참조

4. 메서드 영역의 정적 변수에 의한 참조

이들 중 2,3,4는 Root Set으로 reachability를 판가름하는 기준이 된다.

![Untitled](/image/gc/Untitled1.png)

객체의 reachability를 조절하기 위해 java.lang.ref 패키지의 SoftReference, WeakReference, PhantomReference,ReferenvceQueue 등을 사용한다.

인스턴스가 가비지 컬렉션의 대상이 되었다고 해서 바로 소멸되는 것은 아니다. 빈번한 가비지 컬렉션의 실행은 시스템에 부담이 될 수 있기에 성능에 영향을 미치지 않도록 가비지 컬렉션 실행 타이밍은 **별도의 알고리즘**을 기반으로 계산이 되며, 이 계산결과를 기반으로 가비지 컬렉션이 수행된다.

## 가비지 컬렉션 과정

stop-the-world란, GC를 실행하기 위해 JVM이 애플리케이션 실행을 멈추는 것이다. stop-the-world가 발생하면 GC를 실행하는 쓰레드를 제외한 나머지 쓰레드는 모두 작업을 멈춘다. GC 작업을 완료한 이후에야 중단했던 작업을 다시 시작한다. 어떤 GC 알고리즘을 사용하더라도 stop-the-world는 발생한다. 대개의 경우 GC 튜닝이란 이 stop-the-world 시간을 줄이는 것이다.

JAVA는 프로그램 코드에서 메모리를 명시적으로 지정하여 해제하지 않는다. 가끔 명시적으로 해제하려고 해당 객체를 null로 지정하거나 System.gc() 메서드를 호출하는 개발자가 있는데 null로 지정하는 것은 큰 문제가 안되지만, System.gc() 메서드를 호출하는 것은 시스템의 성능에 매우 큰 영향을 끼치므로 System.gc()는 절대 사용해서는 안된다.

JAVA에서는 개발자가 프로그램 코드로 메모리를 명시적으로 해제하지 않기 때문에 가비지 컬렉터가 더 이상 필요 없는(쓰레기) 객체를 찾아 지우는 작업을 한다. 이 가비지 컬렉터는 두 가지 가정 하에 만들어졌다.

- 대부분의 객체는 금방 접근 불가능 상태(unreachable)가 된다.
- 오래된 객체에서 젊은 객체로의 참조는 아주 적게 존재한다.

이러한 가설을 ‘weak generational hypothesis’라 한다. 이 가정의 장점을 최대한 살리기 위해 HotSpot VM에서는 크게 2개로 물리적 공간을 나누었다. Young영역과 Old 영역이다.

- Young 영역: 새롭게 생성한 객체의 대부분이 여기에 위치한다. 대부분의 객체가 금방 접근 불가능 상태가 되기 때문에 매우 많은 객체가 Young영역에 생성되었다가 사라진다. 이 영역에서 객체가 사라질때 Minor GC가 발생한다고 말한다.
- Old 영역 : 접근 불가능 상태로 되지 않아 Young영역에서 살아남은 객체가 여기로 복사된다. 대부분 Young 영역보다 크게 할당되며, 크기가 큰 만큼 Young 영역보다 GC는 적게 발생한다. 이 영역에서 객체가 사라질 때 Major GC(Full GC)가 발생한다고 말한다.

영역별 데이터 흐름을 그림으로 살펴보면 다음과 같다.

![Untitled](/image/gc/Untitled2.png)

위 그림의 Perm 영역(Permanent Generation)은 Method Area라고 한다. 객체의 억류된 문자열 정보를 저장하는 곳이며, Old 영역에서 살아남은 객체가 영원히 남아있는 곳은 절대 아니다. 이 영역에서 GC가 발생할 수도 있는데, 여기서 GC가 발생해도 Major GC에 포함된다.

그렇다면 ‘Old영역에 있는 객체가 Young 영역의 객체를 참조하는 경우에는 어떻게 처리될까?’ 이러한 경우를 처리하기 위해 Old 영역에는 512 바이트의 덩어리로 되어있는 카드 테이블이 존재한다.

카드 테이블에는 Old 영역에 있는 객체가 Young 영역의 객체를 참조할 때마다 정보가 표시된다. Young 영역의 GC를 실행할 때에는 Old 영역에 있는 모든 객체의 참조를 확인하지 않고, 이 카드 테이블만 뒤져서 GC 대상인지 식별한다.

![Untitled](/image/gc/Untitled3.png)

카드 테이블은 write barrier를 사용하여 관리한다. write barrier는 Minor GC를 빠르게 할 수 있도록 하는 장치이다. write barrier 때문에 약간의 오버헤드는 발생하지만 전반적인 GC 시간은 줄어들게 된다.

# Young 영역

- Eden
- Survivor 영역 (2개)

각 영역의 처리 절차를 순서에 따라 기술하면 다음과 같다.

- 새로 생성한 대부분의 객체는 Eden 영역에 위치한다.
- Eden 영역에서 GC가 한번 발생한 후 살아남은 객체가 Survivor 영역 중 하나로 이동된다.
- Eden 영역에서 GC가 발생하면 이미 살아남은 객체가 존재하는 Survivor 영역으로 객체가 계속 쌓인다.
- 하나의 Survivor 영역이 가득 차게 되면 그중에서 살아남은 객체는 다른 Survivor 영역으로 이동한다. 그리고 가득 찬 Survivor 영역은 아무 데이터도 없는 상태가 된다.
- 이 과정을 반복하다가 계속해서 살아남은 객체는 Old 영역으로 이동하게 된다.

Survivor 영역 중 하나는 반드시 비어있는 상태로 남아있어야 한다. 만약 두 Survivor 영역에 모두 데이터가 존재하거나, 두 영역 모두 사용량이 0이라면 시스템은 정상적인 상황이 아닌 것이다.

이렇게 Minor GC를 통해 Old 영역까지 데이터가 쌓인 것을 간단히 나타내면 다음과 같다.

![Untitled](/image/gc/Untitled4.png)

참고로, HotSpot VM에서는 보다 빠른 메모리 할당을 위해 두 가지 기술을 사용한다. 하나는 bump-the-pointer라는 기술이며, 다른 하나는 TLABs(Thread-Local Allocation Buffers)라는 기술이다.

bump-the-pointer는 Eden 영역에 할당된 마지막 객체를 추적한다. 마지막 객체는 Eden 영역의 맨 위에 있다. 그리고 그 다음에 생성되는 객체가 있으면 해당 객체의 크기가 Eden 영역에 넣기 적당한지만 확인한다. 만약 해당 객체의 크기가 적당하다고 판정되면 Eden 영역에 넣고, 새로 생성된 객체가 맨 위에 있게 된다. 따라서, 새로운 객체를 생성할 때 마지막에 추가된 객체만 점검하면 되므로 매우 빠르게 메모리 할당이 이루어진다.

그러나 멀티 스레드 환경을 고려하면 얘기가 달라진다. Thread-Safe 하기 위해서 만약 여러 스레드에서 사용하는 객체를 Eden 영역에 저장하려면 lock이 발생할 수 밖에 없고, lock-contention 때문에 성능은 매우 떨어지게 될 것이다. HotSpot-Vm에서 이를 해결한 것이 TLABs이다.

각각의 스레드가 각각의 몫에 해당하는 Eden 영역의 작은 덩어리를 가질 수 있도록 하는 것이다. 각 쓰레드는 자기가 갖고 있는 TLAB에만 접근할 수 있기 때문에, bump-the-pointer 기술을 사용하더라도 아무런 락이 없이 메모리 할당이 가능하다.

bump-the-pointer와 TLABs는 몰라도 되지만 Eden 영역에 최초로 객체가 만들어지고 Survivor 영역을 통해 Old 영역으로 오래 살아남은 객체가 이동한다는 사실은 꼭 기억하자.

# Old 영역에 대한 GC

Old 영역은 기본적으로 데이터가 가득 차면 GC를 실행한다. GC 방식에 따라서 처리 절차가 달라지므로 어떤 GC 방식이 있는지 살펴보자. GC 방식은 JDK 7을 기준으로 5가지 방식이 있다.

- Serial GC
- Parallel GC
- Parallel Old GC
- Concurrent Mark& Sweep GC(CMS)
- G1(Garbage First) GC

이 중에서 운영 서버에서 절대 사용하면 안 되는 방식이 Serial GC이다. Serial GC는 데스크톱의 CPU 코어가 하나만 있을 때 사용하기 위해 만든 방식이다. Serial GC를 사용하면 애플리케이션의 성능이 많이 떨어진다.

## Serial GC

Old 영역의 GC는 mark-sweep-compact라는 알고리즘을 사용한다. 이 알고리즘의 첫 단계는 Old 영역에 살아 있는 객체를 식별(Mark)하는 것이다. 그 다음에는 힙의 앞 부분부터 확인하여 살아 있는 것만 남긴다.(sweep) 마지막 단계에서는 각 객체들이 연속되게 쌓이도록 힙의 가장 앞 부분부터 채워서 객체가 존재하는 부분과 객체가 없는 부분으로 나눈다.(Compaction)

Serial GC는 적은 메모리와 CPU 코어 개수가 적을 때 적합한 방식이다.

## parallel GC

Parallel GC는 Serial GC와 기본적인 알고리즘은 같다. 그러나 Serial GC를 처리하는 스레드가 하나인 것에 비해, Parallel GC는 GC를 처리하는 쓰레드가 여러개이다. 그렇기 때문에 Serial GC보다 빠르게 객체를 처리할 수 있다. Parallel GC는 메모리가 충분하고 코어의 개수가 많을 때 유리하다.

![Untitled](/image/gc/Untitled5.png)

## Parallel Old GC

Parallel GC 와 비교하여 Old 영역의 GC 알고리즘만 다르다. 이 방식은 Mark-Summary-Compaction 단계를 거친다. Summary 단계는 앞서 GC를 수행한 영역에 대해서 별도로 살아 있는 객체를 식별한다는 점에서 Mark-Sweep-Compaction 알고리즘의 Sweep 단계와 다르며, 약간 더 복잡한 단계를 거친다.

## CMS GC

다음 그림은 Serial GC와 CMS GC의 절차를 비교한 그림이다. 그림에서 보듯이 CMS GC는 지금까지 설명한 GC보다 더 복잡하다.

![Untitled](/image/gc/Untitled6.png)

초기 이니셜 마크 단계에서는 클래스 로더에서 가장 가까운 객체 중 살아 있는 객체만 찾는 것으로 끝낸다. 따라서, 멈추는 시간은 매우 짧다. 그리고 Concurrent Mark 단계에서는 방금 살아있다고 확인한 객체에서 참조하고 있는 객체들을 따라가면서 확인한다. 이 단계의 특징은 다른 스레드가 실행 중인 상태에서 동시에 진행된다는 것이다.

그 다음 Remark 단계에서는 Concurrent Mark 단계에서 새로 추가되거나 참조가 끊긴 객체를 확인한다. 마지막으로 Concurrent Sweep 단계에서는 쓰레기를 정리하는 작업을 실행한다. 이 작업도 다른 스레드가 실행되고 있는 상황에서 진행한다.

이러한 단계로 진행하는 GC 방식이기 때문에 stop-the-world가 매우 짧다. 모든 애플리케이션의 응답 속도가 매우 중요할 때 CMS GC를 사용하며, Low Latency GC라고도 부른다.

그런데 CMS GC는 다음과 같은 단점이 존재한다.

- 다른 GC 방식보다 메모리와 CPU를 더 많이 사용한다.
- Compaction 단계가 기본적으로 제공되지 않는다.

따라서, CMS GC 를 사용할 때에는 신중히 검토한 후에 사용해야 한다. 그리고 조각난 메모리가 많아 Compaction 작업을 실행하면 다른 GC 방식의 stop-the-world 시간보다 stop-the-world 시간이 더 길기 때문에 Compaction 작업이 얼마나 자주, 오랫동안 수행되는지 확인해야 한다.

## G1 GC

![Untitled](/image/gc/Untitled7.png)

G1 GC는 바둑판의 각 영역에 객체를 할당하고 GC를 실행한다. 그러다가, 해당 영역이 꽉 차면 다른 영역에서 객체를 할당하고 GC를 실행한다. 즉, young 영역에서 데이터가 Old 영역으로 이동하는 단계가 사라진 GC 방식이라고 보면 된다. G1 GC는 CMS GC를 대체하기 위해 만들어 졌다.

G1 GC의 가장 큰 장점은 성능이다. 다른 어떤 GC 방식보다 빠르다.

[https://d2.naver.com/helloworld/1329](https://d2.naver.com/helloworld/1329)