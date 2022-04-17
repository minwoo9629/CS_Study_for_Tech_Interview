## Process란

<aside>
💡 프로세스는 실행 중인 프로그램으로 디스크로부터 메모리에 적재되어 CPU 의 할당을 받을 수 있는 것을 말한다.
운영체제로부터 주소 공간, 파일, 메모리 등을 할당받으며 이것들을 총칭하여 프로세스라고 한다.
즉, 운영체제로부터 자원을 할당받은 **작업**의 단위

</aside>

## **프로세스 제어 블록(Process Control Block, PCB)**

`PCB` 는 특정 **프로세스에 대한 중요한 정보를 저장** 하고 있는 운영체제의 자료구조이다. 운영체제는 프로세스를 관리하기 위해 **프로세스의 생성과 동시에 고유한 PCB 를 생성** 한다.
프로세스는 `CPU` 를 할당받아 작업을 처리하다가도 프로세스 전환이 발생하면 진행하던 작업을 저장하고 `CPU`를 반환해야 하는데, 이때 작업의 진행 상황을 모두 `PCB` 에 저장하게 된다. 그리고 다시 `CPU` 를 할당받게 되면 `PCB` 에 저장되어있던 내용을 불러와 이전에 종료됐던 시점부터 다시 작업을 수행한다.

## Thread란

<aside>
💡 하나의 프로세스 내부에서 독립적으로 실행되는 하나의 작업 단위를 말하며, 세부적으로는 운영체제에 의해 관리되는 하나의 작업 혹은 태스크를 의미한다.
즉, 프로세스가 할당받은 자원을 이용하는 **실행 흐름**의 단위

</aside>

### 0 ~ 10억 까지의 정수에서 짝수만 모두 더했을 때의 값은?

```java
long startTime = System.currentTimeMillis();
long sum = 0;
for (long i = 0; i <= 1000000000; i++) {
		if (0 == i % 2) {
				sum += i;
    }
}
// 계산결과 : 250000000500000000
System.out.println("계산결과 : " + sum);
long endTime = System.currentTimeMillis();
// 연산시간 : 926ms
System.out.println("연산시간 : " + (endTime - startTime));
```

대략 1초의 시간이 걸린다 → 매우 시간이 오래 걸린다는 뜻이다.

이를 `Thread`를 이용하여 처리해보기에 앞서 `Thread`를 간단하게 사용해보자

```java
// Thread는 Runnable 인터페이스를 구현한 객체를 실행한다.
public class Calculator implements Runnable {
    public String calcName;

    public Calculator(String calcName) {
        this.calcName = calcName;
    }

    // 필수적으로 Override 해야한다.
    // run method는 Thread가 실행하는 method이다.
    @Override
    public void run() {
        int sum = 0;
        for (int i = 0; i < 1000; i++) {
            sum += i;
        }
        System.out.println(calcName + "의 결과 : " + sum);
    }
}
```

```java
public class Main {
    public static void main(String[] args) {
        Thread threadA = new Thread(new Calculator("calcA"));
        Thread threadB = new Thread(new Calculator("calcB"));
        // Thread의 start 인스턴스 method를 사용하여 Thread를 실행한다.
        threadA.start();
        threadB.start();
        System.out.println("Main Class의 Main Method 실행 종료");
        // 모든 Thread의 작업이 종료되면 Process가 종료된다.
		}
}

// 실행결과 1
calcA의 결과 : 499500
Main Class의 Main Method 실행 종료
calcB의 결과 : 499500

// 실행결과 2
calcB의 결과 : 499500
calcA의 결과 : 499500
Main Class의 Main Method 실행 종료

// 실행결과 3
Main Class의 Main Method 실행 종료
calcB의 결과 : 499500
calcA의 결과 : 499500
```

### 🤔 왜 이렇게 실행결과가 다르게 나올까?

### 동기 vs 비동기

- 동기 : 이전 작업이 종료될 때 까지 기다린 후 다음 작업을 수행하는 것을 **동기적**이라 한다.
- 비동기 : 이전 작업이 종료될 때 까지 기다리는 것이 아닌 실행명령만 내리고 다음 작업을 수행하는 것이다.
    - 해당 시점에 작업이 동시에 실행되고 있다.
    - 실행 흐름이 동시에 실행되는 것을 **병렬적**이라고 한다.

![image](https://user-images.githubusercontent.com/46440898/163712947-407f47e6-eae9-4f99-94e0-216a7bb625e1.png)

### 0 ~ 10억 까지의 정수에서 짝수만 모두 더했을 때의 값을 Thread를 이용해 처리해보자

```java
// Thread는 Runnable 인터페이스를 구현한 객체를 실행한다.
public class BigCalculator implements Runnable {
    private long sum = 0;
    private final long from;
    private final long to;

    public BigCalculator(long from, long to) {
        this.from = from;
        this.to = to;
    }

		// 필수적으로 Override 해야한다.
    // run method는 Thread가 실행하는 method이다.
		@Override
    public void run() {
        for (long i = from; i <= to; i++) {
            if (0 == i % 2) {
                sum += i;
            }
        }
    }
		public long getSum(){
        return sum;
    }
}
```

```java
try {
    long startTime = System.currentTimeMillis();
    BigCalculator bigCalculator1 = new BigCalculator(0, 500000000);
    Thread thread1 = new Thread(bigCalculator1);
    BigCalculator bigCalculator2 = new BigCalculator(500000001, 1000000000);
    Thread thread2 = new Thread(bigCalculator2);
    thread1.start();
    thread2.start();
    // Thread가 join()메소드를 만나면 다음 구문을 실행하지 않고
    // 해당 Thread 의 작업이 끝날때 까지 기다린다.
    thread1.join();
    thread2.join();
    System.out.println(bigCalculator1.getSum() + bigCalculator2.getSum());
    long endTime = System.currentTimeMillis();
    System.out.println("수행시간 : " + (endTime - startTime));

    // join() 메소드는 InterruptedException인
    // Checked Exception 메소드로 선언되어 있기 때문에 try-catch구문을 작성해야한다.
    } catch (InterruptedException e) {
        e.printStackTrace();
}
// 250000000500000000
// 수행시간 : 401
```

## Thread와 Memory관계

- `Thread` 는 `Stack` 을 제외한 `Heap` 메모리와 `Method Area` 메모리 공간을 공유한다.
- 각 `Thread` 는 자기 자신의 `Stack Memory` 를 가지고 있으며, `Thread` 끼리 `Stack Memory` 공간을 침범할 수 없다.
- 인스턴스 변수나 정적 변수는 `Thread` 가 공유할 수 있으나, `method` 내의 로컬 변수(지역변수)는 `Thread Stack` 에서 운용되므로 다른 `Thread` 와 공유 될 수 없다.