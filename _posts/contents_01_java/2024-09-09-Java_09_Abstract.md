---
layout: single
title: "Java_09_추상클래스와 인터페이스"
date: 2024-09-09
categories:
  - Java
tags:
  - Java
  - 추상클래스
  - 인터페이스
---

<br>

---

<br>

### 추상 클래스 <br>

추상적이라는 말은 실체가 없다는 말과 같다. <br>
즉, 객체 혹은 메서드의 실체 (기능 구현의 코드)가 정의되어 있지 않은 상태를 추상이라고 한다. <br>
***abstract*** 키워드를 통해 정의할 수 있다. <br>

- 추상 메서드는 코드 블럭이 정의되어 있지 않다.
<pre>
public abstract void run();
</pre>

이는 상속을 위해 존재하는 메서드로서 **부모의 추상 메서드는 자식이 반드시 재정의해야 한다**. <br>
이렇게 반드시 구현해야 하는 추상 메서드를 가지고 있는 클래스는 반드시 추상 클래스가 된다. <br>

- **추상 클래스는 인스턴스를 생성할 수 없**으며 **자식 클래스에 기능을 상속하는 용도로만 사용**된다. <br>
자식 클래스간 필드명, 메서드명을 통일하는 용도로 사용할 수 있다. <br>

이렇게 인스턴스를 생성할 수 없고, <br>
자식 클래스에 기능을 상속하는 용도로 사용되는 또 다른 클래스가 있는데 <br>
바로 인터페이스이다.


### 인터페이스 <br>

인터페이스는 순수 추상 클래스와 유사하다. <br>
인터페이스가 포함하고 있는 메서드는 모두 추상 메서드이며, <br>
인터페이스를 구현하는 클래스는 반드시 재정의(오버라이딩) 해야한다. <br>

인터페이스는 기능을 중심으로 분리하여 하나의 클래스가 여러 인터페이스를 구현할 수 있다. <br>
= 다중 구현이 가능.  ","로 연결하여 인터페이스를 나열하여 작성. <br>

상속의 키워드 extends와는 달리 구현의 의미로 implements 라는 키워드를 사용한다. <br>

<hr>

**인터페이스 내에 선언되는 변수는 모두 public, static, final 로 선언된다.** <br>

인터페이스는 공개가 목적이기 때문에 <br>
최대한 많은 클래스에서 접근하고 구현할 수 있도록 **public 접근 제한자가 기본값**이다. <br>

인터페이스는 인터페이스를 상속할 수 있다. <br>
이 경우 "extends" 를 이용하여 상속받는다. ***(기능의 확장)*** <br>


<pre><code>
interface Inter1 {
    /* public final static */ int a = 10; 

    /* 메서드도 변수와 동일하게 public 으로 설정. */
    /* abstract */ void inter1Method(); 
}

interface Inter2 extends Inter1 {
    int b = 20;
}

interface Inter3 /* extends Inter2 */ {
    int c = 30;
}

class InterfaceTest1 implements Inter2, Inter3 {

    /* 메서드 오버라이딩을 하면 접근 제한의 범위가 좁아질 수 없다. */
    /* 클래스 내 접근 제한자의 생략은 "default" _ "public" 으로 명시적으로 작성해야 한다. */
    public void inter1Method() {
        System.out.println("메서드 오버라이딩");
    }

    public static void main(String[] args) {
        /* 객체를 생성하지 않고 인터페이스의 필드(정적 변수)에 접근할 수 있다. */
        System.out.println(Inter1.a);   

        // Inter1.a = 20; // 수정이 안 된다. : final

        // 구현한 이후에는 클래스명을 명시하지 않아도 된다.
        System.out.println(a);
        System.out.println(b);
        System.out.println(c);
    }
}
</code></pre>
<hr>


<br>
<div class="image-container">
    <img class="image-medium" src="/assets/image/2024-08-21-Java-Interface-01.png">
</div>
> 

<br>
<div class="image-container">
    <img class="image-medium" src="/assets/image/2024-08-21-Java-Interface-02.png">
</div>
> 추상 메서드 오버라이딩

<br>
<div class="image-container">
    <img class="image-medium" src="/assets/image/2024-10-12-ClassDiagram_02_Animal_01.png">
    <!-- <img class="image-medium" src="/assets/image/2024-10-12-ClassDiagram_02_Animal_02.png"> -->
</div>
> 클래스 관계도

<br>

인터페이스 중에는 클래스로서의 역할보다 메서드의 기능 구현에 중심을 둔 인터페이스가 있는데 <br>
이를 함수형 인터페이스 **@FunctionalInterface**라고 한다. <br>

<pre><code>
@FunctionalInterface
public interface Runnable {
    public void run();
}
</code></pre>
> 클래스 선언 시 어노테이션(@FunctionalInterface)을 작성하면 메서드를 1개만 가질 수 있고, <br>
람다 표현식을 통해 메서드를 호출할 수 있다. <br>

<pre><code>
Runnable runnable = () -> {
    System.out.println("람다 표현식으로 작성");
}.run();
</code></pre>


위 내용은 [람다와 익명 클래스](2024/10/14/Java_11)에서 다루려고 한다.

<hr>

### "할 수 있는" 인터페이스
    1. Iterable<E>
        반복할 수 있는 _ 구현한 클래스는 "순회"할 수 있다. "iterator() 재정의"
            iterator() 를 통해 Iterator<E> 클래스를 반환할 수 있다.
        * Iterator<E> 클래스 * 
            - hasNext()
            - next()
            - remove()  : 참조한 자료구조의 데이터를 직접 제거.
        List, Set, Queue

    2. Comparable<E>
        비교할 수 있는 _ 구현한 클래스는 "정렬" 시 비교 기준이 생긴다.
            compareTo(Comparable c) 를 통해 정렬 기준을 재정의.
        * Comparator<E> 클래스를 구현함으로써 정렬 기준을 새로 만들 수 있다. *
            - compare(Comparable c1, Comparable c2)
                배열에 값을 담아서 정렬할 수 있지만 
                Arrays.sort(comparable구현 클래스의 객체);
                    : Arrays 클래스의 메서드를 이용해서 값을 정렬할 때 comparable 을 구현한 클래스를 기준으로 정렬한다.
                Arrays.sort(comparable구현 클래스의 객체, Comparator를 구현한 클래스의 생성자 호출 ex. new ComparatorTest()); 
                    : 인자로 Comparator를 구현한 클래스의 생성자를 호출하면 compare() 메서드가 호출되어 정렬 기준을 변경할 수 있다.
        일반적으로 List에 쓰인다. 
            Set구조나 Map 구조는 순서를 보장하지 않기 때문에 사용할 수 없다. 
            TreeSet, TreeMap은 기본적으로 크기 순으로 정렬한다.

    3. Closable
        종료할 수 있는 _ 구현한 클래스는 예외 발생 시 try ~ catch 문을 통해 "예외를 처리"할 때
            close() 메서드를 호출하여 반드시 실행해야 하는 "종료 메서드"등을 실행시킬 수 있다.
        * AutoClosable : Closable의 부모 인터페이스 *
            try문이 종료되면 자동으로 close() 호출.
            try-with-resources문 적용 가능.
                "try-with-resourses"
                    : try~catch 문 내에서만 사용될 변수를 (try문 내에 선언하면 지역변수가 되기 때문에)
                        두 번 선언하게 되는 번거로움을 덜어줄 수 있는 유기력한 예외처리 방법.
            try (BufferedReader BR = new BufferedReader(new InputStreamReader(System.in));) {
                System.out.println(BR.readLine());
            } catch (IOException e) { e.printStackTrace(); }

    4. Serializable
        직렬화할 수 있는 _ 구현한 클래스는 "객체 입출력 스트림"으로 읽거나 쓸 수 있다. 
        별도의 메서드를 포함하고 있지 않는 상징적인 인터페이스.
        "객체를 파일로 저장하거나 네트워크로 전송할 때 사용".
            ObjectOutputStream
            ObjectInputStream
        cf. 직렬화에서 제외할 필드는 transient 키워드 사용.

    5. Runnable
        실행 가능한 _ 구현한 클래스는 "스레드를 생성하고 실행"할 수 있다.
            run() 메서드를 재정의하여 실행할 프로그램 코드를 작성한다.
            start() 를 직접 호출할 수 없기 때문에 Thread 클래스로 포장해야 한다.
                new Thread (Runnable runnable) 
                : 스레드 클래스의 생성자에 Runnable 타입의 객체를 전달하여 스레드 클래스 타입으로 생성. 
        cf. Callable 인터페이스와 함께 사용
            : 값을 반환할 수 있는 비동기 작업을 작업할 때 사용.

    6. Clonable
        복제할 수 있는 _ 구현한 클래스는 객체를 복제할 수 있다.
            clone() 재정의.

    * Throwable 은 인터페이스가 아니라 Error와 Exception의 부모 클래스 *




<br>
<hr>
<br>

[참고 사이트]<br>