# 챕터 5 CPU 스케줄링

![Untitled](/image/os/chapter5/Untitled.png)

프로그램은 CPU 쓰는단계와 I/O를 하는 단계가 번갈아가며 실행된다.

프로그램은 CPU burst와 I/O burst의 연속이지만 프로그램의 종류에 따라서 빈도나 길이가 다르다.

![Untitled](/image/os/chapter5/Untitled1.png)

CPU 스케줄링이 필요한 이유 : 사람과 계속 interaction하는 interactive job이 빈번한 I/O burst로 CPU를 내어주게 되고 CPU를 오래쓰는 작업들이 CPU를 잡고 놓지 않으면 interactive job들은 CPU를 오래 기다려야 한다. ⇒ 사람이 답답하다. 가능하면 사람과 interaction하는 작업한테 CPU를 우선적으로 주는게 필요하다. 

![Untitled](/image/os/chapter5/Untitled2.png)

![Untitled](/image/os/chapter5/Untitled3.png)

## 선점형 스케줄링, 비선점형 스케줄링

현대적인 CPU 스케줄링은 대부분 선점형 스케줄링을 쓰고 있다.

성능 척도 : 스케줄링 알고리즘이 어떤 게 좋은 지 평가할 수 있는 방법

![Untitled](/image/os/chapter5/Untitled4.png)

시스템 입장에서의 성능 척도: 이용률, 처리량

프로세스 입장에서의 성능 척도: 시간과 관련된 성능 척도, 소요시간, 대기시간, 응답 시간

- 이용률 : 전체 시간중에서 CPU가 일한 시간의 비율 (가능한 바빠야 한다.)
- 처리량 : 주어진 시간 동안 일을 얼마나 처리했는가
- 소요시간 : CPU를 쓰러 들어와서 다 쓰고 나갈때까지(프로세스 종료 아님) 걸린 시간
- 대기 시간: 레디큐에서 대기한 시간
- 응답 시간: 최초로 CPU를 얻기까지 기다린 시간

## CPU 스케줄링 알고리즘

![Untitled](/image/os/chapter5/Untitled5.png)

### FCFS(firtst-come firtst-serve)

먼저 온 고객을 먼저 서비스해준다.

비선점형 스케줄링

CPU를 오래쓰는 프로세스가 먼저 온다면 뒤에는 하염없이 기다린다 ⇒ 비효율적

![Untitled](/image/os/chapter5/Untitled6.png)

![Untitled](/image/os/chapter5/Untitled7.png)

앞에 어떤 프로세스가 오냐에 따라서 기다리는 시간에 많은 영향을 미친다. 

### SJF(Shortest-job-first)

CPU를 짧게 쓰는 프로세스한테 CPU를 먼저 준다.

평균 대기 시간(average waiting time)이 가장 짧다.

![Untitled](/image/os/chapter5/Untitled8.png)

![Untitled](/image/os/chapter5/Untitled9.png)

![Untitled](/image/os/chapter5/Untitled10.png)

SJF는 두 가지 문제점 존재

- starvation(기아 현상)문제
    - CPU 사용이 긴 프로세스는 영원히 CPU를 못 받을 수 있음…
- CPU 사용시간을 미리 알 수 없다.
    - 실제 시스템에서 SJF를 사용할 수 없다.
    - 추정할 수는 있음. (과거에 CPU를 얼마나 썼는가, 사용한 흔적을 통해서 )
    - 정확하지는 않겠지만 대부분 프로그램들이 비슷한 패턴을 나타내기 때문에 예측이 가능

![Untitled](/image/os/chapter5/Untitled11.png)

### Priority 스케줄링

우선순위 스케줄링

높은 우선순위를 가진 프로세스에게 CPU 할당한다.

![Untitled](/image/os/chapter5/Untitled12.png)

우선순위가 낮은 프로세스가 지나치게 오래 기다려서 CPU를 얻지 못하는 상황이 발생할 수 있다는 문제가 존재한다.

⇒ 해결방법: 에이징(노화) : 오래 기다리면 우선순위를 조금씩 높힌다.

### Round Robin(RR)

현대적인 컴퓨터 시스템에서 사용하는 CPU 스케줄링은 라운드 로빈 기반이다.

![Untitled](/image/os/chapter5/Untitled13.png)

라운드 로빈 장점

- 응답시간이 빨라진다. 누구든 짧은시간에 CPU를 한번 맛 볼 수 있는 기회가 생긴다.
- 누가 CPU를 오래 쓸지 모르는 상황에서 굳이 예측할 필요 없이 CPU를 짧게 쓰는 프로세스가 빨리 CPU를 쓰고 나갈 수 있다.

![Untitled](/image/os/chapter5/Untitled14.png)

 

### Multilevel Queue

Ready Queue가 하나가 아닌 여러개.

![Untitled](/image/os/chapter5/Untitled15.png)

큐 별로 특성에 맞는 스케줄링 알고리즘 채택 해야한다.

또한 큐들에 대한 우선순위를 설정해야한다.

- 고정 된 우선순위 스케줄링 방식
    - 높은 우선순위가 비었을때만 낮은 우선순위 진행 ⇒ 기아 현상 발생
- 타임 슬라이스
    - 각 큐별로 CPU 시간을 나누어 할당

⇒ Multilevel Queue는 우선순위를 극복하지 못하는 문제가 존재

 

### Multilevel feedback queue

우선순위 변경 가능 큐

![Untitled](/image/os/chapter5/Untitled16.png)

빨간글씨는 고려해야 할 사항

보통 어떤 식으로 운용하냐면

처음 들어온 프로세스는 우선순위가 가장 높은 큐에 넣는다. ⇒ 우선순위 높은 큐는 라운드로빈 방식으로 타임 퀀텀을 짧게 준다. 밑으로 갈수록 퀀텀 시간 길게 주고 제일 아래 큐는 FCFS 방식. ⇒ 해당 큐에서 해결 못하면 점점 밑의 큐로 내려간다.

![Untitled](/image/os/chapter5/Untitled17.png)

![Untitled](/image/os/chapter5/Untitled18.png)

CPU 사용 시간 짧은 프로세스가 우대받을 수 있다.

# 특이한 케이스의 CPU 스케줄링

### CPU가 여러개인 상황

![Untitled](/image/os/chapter5/Untitled19.png)
### Real-Time 스케줄링

real-time job은 데드라인이 있는 잡. 정해진 시간 안에 실행이 되야만 하는 잡.

![Untitled](/image/os/chapter5/Untitled20.png)

### 쓰레드 스케줄링

![Untitled](/image/os/chapter5/Untitled21.png)
로컬 스케줄링: 운영체제가 쓰레드를 모르기 때문에 운영체제는 프로세스한테 CPU를 주고 프로세스 내부에서 어떤 쓰레드에게 CPU를 줄지 결정하는것. OS가 하는 것이 아니고 사용자 프로세스가 직접 어느 쓰레드 한테 CPU 줄지 결정하는 것.

글로벌 스케줄링: 운영체제가 쓰레드 존재를 알고 있기 때문에 프로세스 스케줄링 하듯이 운영체제가 어떤 쓰레드에게 CPU를 줄지 결정.

 

# 스케줄링 알고리즘 평가

평가 방법

- 큐잉 모델
    - 이론적인 방법
    - 프로세스가 큐에 들어오는 도착률과 CPU의 처리 능력인 처리율(단위시간당 몇개 처리하는지)이 확률 분포로 주어질 때 계산을 통해 성능 척도 결과를 도출
- 구현 & 성능 측정
    - 실제 시스템에 구현해서 성능 측정
- 시뮬레이션
    - 실제로 돌리는 것이 아니고 시뮬레이션 프로그램을 만들어 테스트
    - 트레이스는 시뮬레이션 프로그램에 들어갈 인풋 데이터 (임의로 만들수도 있고 실제 프로그램에서 추출해도 된다.)
    - 실제 프로그램을 통해 추출한 인풋 데이터를 바탕으로 트레이스 만듬.(실제 프로그램의 프로세스들을 추출(언제 시작하고 CPU를 얼만큼의 시간동안 사용하고 그런 정보들))

![Untitled](/image/os/chapter5/Untitled22.png)
