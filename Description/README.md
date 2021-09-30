
# 1. Immutable 패턴 이란?
- 한번 만든 객체를 변형하지 않고, 변형이 발생할 때마다 새로운 객체를 만드는 방식.
- 적절하게 사용하면 안정성과 유지보수성이 비약적으로 향상된다.
- ex) JAVA를 사용하면서 우리도 모르게 Immutable 패턴을 사용하고 있는데 대표적인 예가 String 클래스이다.
- 멀티쓰레드 환경에서 발생하는 문제를 원천적으로 해결.(동시성 해결)<br/><br/>


### 1) Immutable 패턴 예 1
```Java
String a = "WHO";
String b = "ARE YOU";
String c = a.concat(b); // ---> Immutable 패턴이 적용된 concat 메소드 실행으로 새로운 String 인스턴스가 생성되어 c에 인스턴스 참조 값이 저장. 
```
- 위 코드를 실행하면 concat 메소드 실행 시, 새로운 String 인스턴스가 생성된다. 
- 즉, 기존의 String 변수 a나 b의 값이 변형되는 것이 아닌 새로운 String 인스턴스를 생성하고 값을 저장한다는 것이 핵심이다.<br/><br/>



### 2) Immutable 패턴 예 2
- 아래의 코드는 JDK의 String 클래스의 소스코드에 포함되어 있는 concat과 substring 메소드이다.
```Java
public String concat(String str) {
    int otherLen = str.length();
    if (otherLen == 0) {
        return this;
    }
    char buf[] = new char[count + otherLen];
    getChars(0, count, buf, 0);
    str.getChars(0, otherLen, buf, count);
    
    return new String(0, count + otherLen, buf);  // ---> 새롭게 만들어진 문자들이 할당된 String 인스턴스를 새로 생성하여 반환.
}

public String substring(int beginIndex, int endIndex) {
    if (beginIndex < 0) {
        throw new StringIndexOutOfBoundsException(beginIndex);
    }
    if (endIndex > count) {
        throw new StringIndexOutOfBoundsException(endIndex);
    }
    if (beginIndex > endIndex) {
        throw new StringIndexOutOfBoundsException(endIndex - beginIndex);
    }
    
    return ((beginIndex == 0) && (endIndex == count)) ? this : new String(offset + beginIndex, endIndex - beginIndex, value);   // ---> 새롭게 만들어진 문자들이 할당된 String 인스턴스를 새로 생성하여 반환.
}
```
- 코드에서 알 수 있듯이, 문자열을 변형시키는 2가지 메소드인 concat과 substring은 내부적으로 새로운 문자열 버퍼를 구성하고 스트링 인스턴스를 생성과 문자열 변형 작업을 수행한 뒤에 마지막 return 시, 반드시 새로운 String 인스턴스를 생성하고 변형된 문자열을 저장하여 반환한다. 


- 이러한 Immutable 패턴 방식은 멀티쓰레드 환경에서 발생하는 다양한 문제들(ex : 공용 인스턴스의 값 변화로 인해 다른 쓰레드에 원치않는 영향을 끼치는)을 원천적으로 해결할 수 있다.<br/><br/><br/><br/>


# 2. Strategy 패턴 이란?
- 행위를 클래스로 캡슐화하여 동적으로 행위를 자유롭게 변환할 수 있도록 하는 방식.
- 즉, 하나의 추상적인 접근 통로를 만들고 이를 통해 여러 로직을 서로 교환 가능하도록 하는 패턴.


# 3. Adapter 패턴 이란?
- 인터페이스 구현을 통해 클라이언트가 private한 클래스 인스턴스에 접근이 가능하도록 하는 방식
- 즉, 객체를 인터페이스와, 구현 클래스로 분리해서 여러 개의 객체들이 하나의 인터페이스에 의해 사용될 수 있도록 정의하는 방식.
- 캡슐화 효과를 체감할 수 있는 방식이다. 

