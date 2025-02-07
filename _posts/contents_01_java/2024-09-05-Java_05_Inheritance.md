---
layout: single
title: "Java_05_상속과 참조관계"
date: 2024-09-05
categories:
  - Java
tags:
  - Java
  - 상속
  - 참조관계
---

<br>

---

<br>

## 상속

클래스는 다양한 데이터를 저장할 수 있으며 <br>
저장된 데이터를 바탕으로 많은 기능을 수행할 수 있다. <br>
하지만 **하나의 클래스는 하나의 기능만을 수행**하도록 <br>
기능이 다를 경우 **별도의 클래스로 구분**하는 것이 <br>
단일 책임 원칙을 따른다고 할 수 있다. <br>
<br>

하지만 유사한 기능을 수행하는 클래스의 경우 공통되는 요소가 있을 수 있다. <br>
이 경우 공통 기능을 수행하는 객체를 **상속**함으로써 공통 기능을 물려받을 수 있다. <br>


> 예를 들어, <br>
TV와 Radio, Speaker라는 세 종류의 전자기기가 있을 경우 <br>
전원 작동과 음향 조절이라는 공통 기능을 수행한다고 하면 <br>

<pre><code>
interface Power {
    public void powerOn();
    public void powerOff();
}
interface Volume {
    public void volumeUp();
    public void volumeDown();
}
</code></pre>
처럼 공통 기능을 별도의 객체로 분리할 수 있으며

<pre><code>
class ElecDevice implements Power, Volume {
    boolean isPowered = false;
    int volumn = 0;

    public void powerOn() {
        isPowered = true;
    }
    public void powerOff() {
        isPowered = false;
    }

    public void volumeUp() {
        volume += 1;
    }
    public void volumeDown() {
        volume -= 1;
    }
}
</code></pre>
과 같이 공통 기능을 수행할 객체를 마련할 수 있다. <br>


이 때 TV, Radio, Speaker라는 구체적인 객체는 전자기기라는 객체를 상속하게 되면 <br>
전원 작동과 음향 조절의 기능을 그대로 물려받아 사용할 수 있다.<br>

상속의 키워드는 ***extends***로 기능이 확장된다는 **개방 폐쇄의 원칙**을 따른다. <br>

<pre><code>
class TV extends ElecDevice {};
class Radio extends ElecDevice {};
class Speaker extends ElecDevice {};
</code></pre>

<br><br>
<div class="image-container" style="border: 2px solid white;">
    <img class="image-medium" src="/assets/image/2024-10-08-Java-ClassDiagram_01_ElecDevice.png">
</div>
> 클래스간 관계도 (UML)



<br>
--- 
<br>

## 참조 관계
객체는 메모리 상에 저장된 참조 주소를 변수에 저장한다. <br>



상속 관계에서 부모 타입의 객체는 자식의 참조값을 저장할 수 있다. <br>
이를 자동(묵시적) 형변환 ***Promotion***이라고 한다. <br>
자식 객체에 부모 타입의 데이터를 저장하기 위해서는 강제로 타입을 변환해야 하는데 <br>
이를 강제(명시적) 형변환 ***Casting***이라고 한다. <br>