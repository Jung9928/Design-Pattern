# Singleton 패턴
- 생성자를 호출하여 객체의 인스턴스를 최초로 생성 시 전역 공간에 생성한 후, 추후에 생성자를 호출하여 같은 객체에 대한 인스턴스를 생성할 경우 이미 전역공간에 생성된 인스턴스를 반환하는 방식. 
- 다르게 말하면, 객체의 인스턴스를 단 1개만 생성하도록 하는 방식.

### :star2: 장점
- 매번 인스턴스를 생성할 때마다 heap공간을 할당받을 필요가 없이 기존에 생성된 인스턴스를 사용하므로 메모리 낭비 방지
- 최초 생성 이후, new 키워드를 사용하지 않으니 JVM에 의해 수행되는 JIT 컴파일 과정에서 Native 아키텍처(인텔이면 x86 or x64)의 메모리 할당 인스트럭션이 생성되지 않으므로 성능 향상.
- 전역공간에 생성하기때문에 다른 클래스의 인스턴스들이 데이터 공유가 가능.
- DBCP처럼 공통된 객체를 생성해서 사용해야할 경우 많이 사용(쓰레드 풀, 캐시, 레지스트리 설정, 로그 기록 객체 등)
<br/><br/>


### :collision: 단점
- 싱글톤 인스턴스가 너무 많은 일을 하거나 많이 참조(공유)될 경우 다른 클래스와의 coupling(결합도)가 높아져 "개방-폐쇄 원칙"을 위배하게 된다. ----> 테스트하기 어려워지고 유지보수하기 까다로워진다.
- 멀티쓰레드 환경에서 동기화처리를 안하면 인스턴스가 여러 개 생성되는 경우가 발생할 수 있다.
<br/><br/>

# Singleton 코드 예시
- Company.Java
```Java
package singleton;

public class Company {
    
    // 전역공간에 최초로 한번 생성된 이후, 계속해서 사용될 싱글톤 인스턴스
    private static Company instance = new Company();
    
    private Company() {}  // 접근지정자를 private으로 설정하여 외부에서 생성자 호출을 통해 인스턴스 생성을 못하도록 설정    

    // Company 클래스 내부에서 생성된 instance를 외부에서 사용할 수 있도록 하는 메소드
    public static Company getInstance() {
        
        // 혹시나 null이 반환될 경우를 대비(null-safe) 하기 위해 조건 추가
        if(instance == null)
            instance = new Company();
            
        return instance;
    }
}
```
- CompanyTest.Java
```Java
public class CompanyTest {
    
    public static void main(String[] args) {
        
        Company c1 = Company.getInstance();
        Company c2 = Company.getInstance();
        
        // 출력해보면 c1, c2인스턴스는 동일한 인스턴스 참조값이 출력된다.
        System.out.println(c1);
        System.out.println(c2);
    }
}
```
- 자바에서 쓰이는 Calender도 Singleton 패턴이 적용되어있어 new 키워드를 사용하여 생성자를 호출하는 것이 아니라 클래스명으로 접근하여 사용한다.
```Java
import java.util.Calender;

public class CompanyTest {
    
    public static void main(String[] args) {
        
        Calender cal = Calender.getInstance();
    }
}
```
