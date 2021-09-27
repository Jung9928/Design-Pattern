
# 1. Immutable 패턴 이란?
- 한번 만든 객체를 변형하지 않고, 변형이 발생할 때마다 새로운 객체를 만드는 방식.
- 적절하게 사용하면 안정성과 유지보수성이 비약적으로 향상된다.
- ex) JAVA를 사용하면서 우리도 모르게 Immutable 패턴을 사용하고 있는데 대표적인 예가 String 클래스이다.


### 1) Immutable 패턴 예
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
