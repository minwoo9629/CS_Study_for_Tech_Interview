## Processλ

<aside>
π‘ νλ‘μΈμ€λ μ€ν μ€μΈ νλ‘κ·Έλ¨μΌλ‘ λμ€ν¬λ‘λΆν° λ©λͺ¨λ¦¬μ μ μ¬λμ΄ CPU μ ν λΉμ λ°μ μ μλ κ²μ λ§νλ€.
μ΄μμ²΄μ λ‘λΆν° μ£Όμ κ³΅κ°, νμΌ, λ©λͺ¨λ¦¬ λ±μ ν λΉλ°μΌλ©° μ΄κ²λ€μ μ΄μΉ­νμ¬ νλ‘μΈμ€λΌκ³  νλ€.
μ¦, μ΄μμ²΄μ λ‘λΆν° μμμ ν λΉλ°μΒ **μμ**μ λ¨μ

</aside>

## **νλ‘μΈμ€ μ μ΄ λΈλ‘(Process Control Block, PCB)**

`PCB` λ νΉμ Β **νλ‘μΈμ€μ λν μ€μν μ λ³΄λ₯Ό μ μ₯**Β νκ³  μλ μ΄μμ²΄μ μ μλ£κ΅¬μ‘°μ΄λ€. μ΄μμ²΄μ λ νλ‘μΈμ€λ₯Ό κ΄λ¦¬νκΈ° μν΄Β **νλ‘μΈμ€μ μμ±κ³Ό λμμ κ³ μ ν PCB λ₯Ό μμ±**Β νλ€.
νλ‘μΈμ€λ `CPU` λ₯Ό ν λΉλ°μ μμμ μ²λ¦¬νλ€κ°λ νλ‘μΈμ€ μ νμ΄ λ°μνλ©΄ μ§ννλ μμμ μ μ₯νκ³  `CPU`λ₯Ό λ°νν΄μΌ νλλ°, μ΄λ μμμ μ§ν μν©μ λͺ¨λ `PCB` μ μ μ₯νκ² λλ€. κ·Έλ¦¬κ³  λ€μ `CPU` λ₯Ό ν λΉλ°κ² λλ©΄ `PCB` μ μ μ₯λμ΄μλ λ΄μ©μ λΆλ¬μ μ΄μ μ μ’λ£λλ μμ λΆν° λ€μ μμμ μννλ€.

## Threadλ

<aside>
π‘ νλμ νλ‘μΈμ€ λ΄λΆμμ λλ¦½μ μΌλ‘ μ€νλλ νλμ μμ λ¨μλ₯Ό λ§νλ©°, μΈλΆμ μΌλ‘λ μ΄μμ²΄μ μ μν΄ κ΄λ¦¬λλ νλμ μμ νΉμ νμ€ν¬λ₯Ό μλ―Ένλ€.
μ¦, νλ‘μΈμ€κ° ν λΉλ°μ μμμ μ΄μ©νλΒ **μ€ν νλ¦**μ λ¨μ

</aside>

### 0 ~ 10μ΅ κΉμ§μ μ μμμ μ§μλ§ λͺ¨λ λνμ λμ κ°μ?

```java
long startTime = System.currentTimeMillis();
long sum = 0;
for (long i = 0; i <= 1000000000; i++) {
		if (0 == i % 2) {
				sum += i;
    }
}
// κ³μ°κ²°κ³Ό : 250000000500000000
System.out.println("κ³μ°κ²°κ³Ό : " + sum);
long endTime = System.currentTimeMillis();
// μ°μ°μκ° : 926ms
System.out.println("μ°μ°μκ° : " + (endTime - startTime));
```

λλ΅ 1μ΄μ μκ°μ΄ κ±Έλ¦°λ€ β λ§€μ° μκ°μ΄ μ€λ κ±Έλ¦°λ€λ λ»μ΄λ€.

μ΄λ₯Ό `Thread`λ₯Ό μ΄μ©νμ¬ μ²λ¦¬ν΄λ³΄κΈ°μ μμ `Thread`λ₯Ό κ°λ¨νκ² μ¬μ©ν΄λ³΄μ

```java
// Threadλ Runnable μΈν°νμ΄μ€λ₯Ό κ΅¬νν κ°μ²΄λ₯Ό μ€ννλ€.
public class Calculator implements Runnable {
    public String calcName;

    public Calculator(String calcName) {
        this.calcName = calcName;
    }

    // νμμ μΌλ‘ Override ν΄μΌνλ€.
    // run methodλ Threadκ° μ€ννλ methodμ΄λ€.
    @Override
    public void run() {
        int sum = 0;
        for (int i = 0; i < 1000; i++) {
            sum += i;
        }
        System.out.println(calcName + "μ κ²°κ³Ό : " + sum);
    }
}
```

```java
public class Main {
    public static void main(String[] args) {
        Thread threadA = new Thread(new Calculator("calcA"));
        Thread threadB = new Thread(new Calculator("calcB"));
        // Threadμ start μΈμ€ν΄μ€ methodλ₯Ό μ¬μ©νμ¬ Threadλ₯Ό μ€ννλ€.
        threadA.start();
        threadB.start();
        System.out.println("Main Classμ Main Method μ€ν μ’λ£");
        // λͺ¨λ  Threadμ μμμ΄ μ’λ£λλ©΄ Processκ° μ’λ£λλ€.
		}
}

// μ€νκ²°κ³Ό 1
calcAμ κ²°κ³Ό : 499500
Main Classμ Main Method μ€ν μ’λ£
calcBμ κ²°κ³Ό : 499500

// μ€νκ²°κ³Ό 2
calcBμ κ²°κ³Ό : 499500
calcAμ κ²°κ³Ό : 499500
Main Classμ Main Method μ€ν μ’λ£

// μ€νκ²°κ³Ό 3
Main Classμ Main Method μ€ν μ’λ£
calcBμ κ²°κ³Ό : 499500
calcAμ κ²°κ³Ό : 499500
```

### π€Β μ μ΄λ κ² μ€νκ²°κ³Όκ° λ€λ₯΄κ² λμ¬κΉ?

### λκΈ° vs λΉλκΈ°

- λκΈ° : μ΄μ  μμμ΄ μ’λ£λ  λ κΉμ§ κΈ°λ€λ¦° ν λ€μ μμμ μννλ κ²μ **λκΈ°μ **μ΄λΌ νλ€.
- λΉλκΈ° : μ΄μ  μμμ΄ μ’λ£λ  λ κΉμ§ κΈ°λ€λ¦¬λ κ²μ΄ μλ μ€νλͺλ Ήλ§ λ΄λ¦¬κ³  λ€μ μμμ μννλ κ²μ΄λ€.
    - ν΄λΉ μμ μ μμμ΄ λμμ μ€νλκ³  μλ€.
    - μ€ν νλ¦μ΄ λμμ μ€νλλ κ²μ **λ³λ ¬μ **μ΄λΌκ³  νλ€.

![image](https://user-images.githubusercontent.com/46440898/163712947-407f47e6-eae9-4f99-94e0-216a7bb625e1.png)

### 0 ~ 10μ΅ κΉμ§μ μ μμμ μ§μλ§ λͺ¨λ λνμ λμ κ°μ Threadλ₯Ό μ΄μ©ν΄ μ²λ¦¬ν΄λ³΄μ

```java
// Threadλ Runnable μΈν°νμ΄μ€λ₯Ό κ΅¬νν κ°μ²΄λ₯Ό μ€ννλ€.
public class BigCalculator implements Runnable {
    private long sum = 0;
    private final long from;
    private final long to;

    public BigCalculator(long from, long to) {
        this.from = from;
        this.to = to;
    }

		// νμμ μΌλ‘ Override ν΄μΌνλ€.
    // run methodλ Threadκ° μ€ννλ methodμ΄λ€.
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
    // Threadκ° join()λ©μλλ₯Ό λ§λλ©΄ λ€μ κ΅¬λ¬Έμ μ€ννμ§ μκ³ 
    // ν΄λΉ Thread μ μμμ΄ λλ λ κΉμ§ κΈ°λ€λ¦°λ€.
    thread1.join();
    thread2.join();
    System.out.println(bigCalculator1.getSum() + bigCalculator2.getSum());
    long endTime = System.currentTimeMillis();
    System.out.println("μνμκ° : " + (endTime - startTime));

    // join() λ©μλλ InterruptedExceptionμΈ
    // Checked Exception λ©μλλ‘ μ μΈλμ΄ μκΈ° λλ¬Έμ try-catchκ΅¬λ¬Έμ μμ±ν΄μΌνλ€.
    } catch (InterruptedException e) {
        e.printStackTrace();
}
// 250000000500000000
// μνμκ° : 401
```

## Threadμ Memoryκ΄κ³

- `Thread` λ `Stack` μ μ μΈν `Heap` λ©λͺ¨λ¦¬μ `Method Area` λ©λͺ¨λ¦¬ κ³΅κ°μ κ³΅μ νλ€.
- κ° `Thread` λ μκΈ° μμ μ `Stack Memory` λ₯Ό κ°μ§κ³  μμΌλ©°, `Thread` λΌλ¦¬ `Stack Memory` κ³΅κ°μ μΉ¨λ²ν  μ μλ€.
- μΈμ€ν΄μ€ λ³μλ μ μ  λ³μλ `Thread` κ° κ³΅μ ν  μ μμΌλ, `method` λ΄μ λ‘μ»¬ λ³μ(μ§μ­λ³μ)λ `Thread Stack` μμ μ΄μ©λλ―λ‘ λ€λ₯Έ `Thread` μ κ³΅μ  λ  μ μλ€.