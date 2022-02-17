## Serializable(직렬화)
  - Serializable의 인터페이스를 보면 메소드가 하나도 없는것을 볼 수 있다. 아무런 구현해야할 메소드도 없는 이 인터페이스는 왜 있는것인가
### 개발을 하다보면 아래와 같은 경우가 존재한다.
  - 생성한 객체를 파일로 저장할 일이 있을 수도 있다.
  - 저장한 객체를 읽을 일이 생길 수도 있다
  - 다른 서버에서 생성한 객체를 받을 일도 생길 수 있다.
이럴 때 꼭 필요한 것이 Serializable 이다. 우리가 만든 클래스가 파일에 읽거나 쓸 수 있도록 하거나, 다른 서버로 보내거나 받을 수 있도록 하려면 반드시 이 인터페이스를 구현해야한다
Serializable 인터페이스를 구현하면 JVM에서 해당 객체는 저장하거나 다른 서버로 전송할 수 있도록 해준다.

### 직렬화란? 
  - 자바 직렬화란 자바 시스템 내부에서 사용되는 객체 또는 데이터를 외부의 자바 시스템에서도 사용할 수 있도록 바이트(byte) 형태로 데이터 변환하는 기술,
  - 바이트로 변환된 데이터를 다시 객체로 변환하는 기술(역직렬화)을 아울러서 이야기함
  - 시스템적으로 이야기하자면 JVM(Java Virtual Machine 이하 JVM)의 메모리에 상주(힙 또는 스택)되어 있는 객체 데이터를 바이트 형태로 변환하는 기술과
    직렬화된 바이트 형태의 데이터를 객체로 변환해서 JVM으로 상주시키는 형태를 같이 이야기합니다.
### 어떻게 사용하는가?
````java
// 직렬화 조건
public class Member implements Serializable {
        private String name;
        private String email;
        private int age;

        public Member(String name, String email, int age) {
            this.name = name;
            this.email = email;
            this.age = age;
        }

        // Getter 생략

        @Override
        public String toString() {
            return String.format("Member", name, email, age);
        }
    }
////////////////////////////////////////////////////////////////
// 직렬화 방법
// java.io.ObjectOutputStream 이용

Member member = new Member("김배민", "deliverykim@baemin.com", 25);
    byte[] serializedMember;
    try (ByteArrayOutputStream baos = new ByteArrayOutputStream()) {
        try (ObjectOutputStream oos = new ObjectOutputStream(baos)) {
            oos.writeObject(member);
            // serializedMember -> 직렬화된 member 객체 
            serializedMember = baos.toByteArray();
        }
    }
    // 바이트 배열로 생성된 직렬화 데이터를 base64로 변환
    System.out.println(Base64.getEncoder().encodeToString(serializedMember));
}
// 객체를 직렬 화하여 바이트 배열(byte []) 형태로 변환
````
### 역직렬화 조건
 - 직력화 대상이 된 객체의 클래스가 클래스 패스에 존재해야하며 import 되어 있어야한다.
 - 중요한 점은 직렬화와 역직렬화를 진행하는 시스템이 서로 다를 수 있다는 것을 반드시 고려해야한다.(같은 시스템이라도 소스 버전다를수있음)
 - 자바 직렬화 대상 객체는 동일한 serialVersionUID 를 가지고있어야함

````java
private static final long serialVersionUID = 1L;
````

### 역직렬화 
````java
// 직렬화 예제에서 생성된 base64 데이터 
    String base64Member = "...생략";
    byte[] serializedMember = Base64.getDecoder().decode(base64Member);
    try (ByteArrayInputStream bais = new ByteArrayInputStream(serializedMember)) {
        try (ObjectInputStream ois = new ObjectInputStream(bais)) {
            // 역직렬화된 Member 객체를 읽어온다.
            Object objectMember = ois.readObject();
            Member member = (Member) objectMember;
            System.out.println(member);
        }
    }
````
  - 사용이유
  - 직접 데이터를 문자열 형태로 확인 가능한 직렬화 방법입니다. 범용적인 API나 데이터를 변환하여 추출할 때 많이 사용됩니다.
    표형태의 다량의 데이터를 직렬화시 CSV가 많이 쓰이고 구조적인 데이터는 이전에는 XML을 많이 사용했으며 최근에는 JSON형태를 많이 사용하고 되고 있습니다.
    여기서는 CSV와 JSON만 살펴보겠습니다.
    
### csv
````java
 // 데이터를 표현하는 가장 많이 사용되는 방법 중 하나로 콤마(,) 기준으로 데이터를 구분하는 방법입니다
 // 김배민,deliverykim@baemin.com
 
 // 자바에서 사용방법
   Member member = new Member("김배민", "deliverykim@baemin.com", 25);
// member객체를 csv로 변환 
   String csv = String.format("%s,%s,%d",member.getName(), member.getEmail(), member.getAge()); 
   System.out.println(csv);
 ````
  - 예제에서는 문자열로 단순히 변경했습니다

### json
  - 최근에 가장 많이 사용하는 포맷으로 자바스크립트에서 쉽게 사용가능하고 다른 데이터포맷방식에 비해 오버헤드가 적어서 인기많음
````java
{ 
  name: "김배민",
  email: "deliverykim@baemin.com",
  age: 25 
}

// 자바에서 사용방법

Member member = new Member("김배민", "deliverykim@baemin.com", 25);
// member객체를 json으로 변환 
String json = String.format(
        "",
        member.getName(), member.getEmail(), member.getAge());
System.out.println(json); 
````
 - JSON도 물론 이렇게 직접 문자열을 만들일은 거의 없습니다.

### 이진직렬화
  - 데이터 변환 및 전송 속도에 최적화하여 별도의 직렬화 방법을 제시하는 구조입니다.
  - 직렬화뿐만 아니라 전송 방법에 대한 부분도 이야기하고 있지만 여기서는 직렬화 부분만 이야기하겠습니다.
  - 종류로는 Protocol Buffer(이하 프로토콜버퍼) Apache Avro 등이 있습니다.
  - 기타
  - 지면 관계상 프로토콜 버퍼만 한번 살펴보겠습니다. (살펴보면 알겠지만 직렬화하기 위한 패턴은 비슷합니다.)
      - Protocol Buffer(이하 프로토콜 버퍼)
        - 프로토콜 버퍼는 구글에서 제안한 플랫폼 독립적인 데이터 직렬화 플랫폼입니다.
        - 자바에서 사용방법
          프로토콜 버퍼는 특정 언어 또는 플랫폼에 종속되지 않는 방법을 구현하기 위해 직렬화 하기 위한 데이터를 표현하기 위한 문서가 따로 있습니다.
          
````java
message Member {
  required string name = 1;
  optional string email = 2;
  required int32 age = 3;
}
````
          - 이렇게 기술된 member.proto 문서를 프로토콜 버퍼 컴파일러를 이용해서 개발하기 원하는 언어(여기서는 자바)로 변환해야 합니다.
            (프로토콜 버퍼 컴파일러는 별도로 설치하거나 Gradle, Maven등 의 빌드 도구를 이용하면 됩니다.)
            자바로 변환하게 되면 프로토콜 버퍼 형태의 Member 클래스가 생성됩니다.
````java
Member member = Member.newBuilder()
    .setAge(25)
    .setName("김배민")
    .setEmail("deliverykim@baemin.com")
    .build();
ByteArrayOutputStream baos = new ByteArrayOutputStream()
member.writeTo(baos);
// 프로토콜 버퍼 직렬화된 데이터
byte[] serializedMember = baos.toByteArray();   
````
  - 그럼 다시 왜 자바 직렬화를 사용하는지 이야기해보겠습니다. CSV, JSON, 프로토콜 버퍼 등은 시스템의 고유 특성과 상관없는 대부분의 시스템에서의 데이터 교환 시 많이 사용됩니다.
    하지만 "자바 직렬화 형태의 데이터 교환은 자바 시스템 간의 데이터 교환을 위해서 존재한다."고 생각하시면 됩니다.
    
### 여기서 중요한 질문 "그럼 자바에서도 CSV, JSON을 사용하면 되지 자바 직렬화를 써야 되는 이유가 있나요?"
 - "목적에 따라 적절하게 써야 한다."
 - 자바 직렬화의 장점
   - 직렬화는 자바 시스템에서 개발에 최적화되어 있습니다.
   - 잡한 데이터 구조의 클래스의 객체라도 직렬화 기본 조건만 지키면 큰 작업 없이 바로 직렬화를 가능합니다. 물론 역직렬화도 마찬가지입니다.
   - 하게 보이는 장점 중에 하나지만 데이터 타입이 자동으로 맞춰지기 때문에 관련 부분을 큰 신경을 쓰지 않아도 됩니다.
     그렇게 역질렬화가 되면 기존 객체처럼 바로 사용할 수 있게 됩니다. 개발자 입장에서 상당히 편한 부분인거죠.
 
### 자바 직렬화는 언제(when) 어디서(where) 사용되나요?
 - JVM의 메모리에서만 상주되어있는 객체 데이터를 그대로 영속화(Persistence)가 필요할 때 사용됩니다.
   시스템이 종료되더라도 없어지지 않는 장점을 가지며 영속화된 데이터이기 때문에 네트워크로 전송도 가능합니다.
   그리고 필요할 때 직렬화된 객체 데이터를 가져와서 역직렬 화하여 객체를 바로 사용할 수 있게 됩니다.
   그런 특성을 살린 자바 직렬화는 많은 곳에서 이용됩니다. 많이 사용하는 부분 몇 개만 이야기해보겠습니다.
    - 서블릿 세션 (Servlet Session)
      - 서블릿 기반의 WAS(톰캣, 웹로직 등)들은 대부분 세션의 자바 직렬화를 지원하고 있습니다.
        물론 단순히 세션을 서블릿 메모리 위에서 운용한다면 직렬화를 필요로 하지 않지만,
        파일로 저장하거나 세션 클러스터링, DB를 저장하는 옵션 등을 선택하게 되면 세션 자체가 직렬화가 되어 저장되어 전달됩니다.
        (그래서 세션에 필요한 객체는 java.io.Serializable 인터페이스를 구현(implements) 해두는 것을 추천합니다.)
        참고로 위 내용은 서블릿 스펙에서는 직접 기술한 내용이 아니기 때문에 구현한 WAS 마다 동작은 달라질 수 있습니다.
      - 캐시 (Cache)
        자바 시스템에서 퍼포먼스를 위해 캐시(Ehcache, Redis, Memcached, …)
        라이브러리를 시스템을 많이 이용하게 됩니다.
        자바 시스템을 개발하다 보면 상당수의 클래스가 만들어지게 됩니다.
        예를 들면 DB를 조회한 후 가져온 데이터 객체 같은 경우 실시간 형태로 요구하는 데이터가 아니라면
        메모리, 외부 저장소, 파일 등을 저장소를 이용해서 데이터 객체를 저장한 후 동일한 요청이 오면 DB를 다시 요청하는 것이 아니라 저장된 객체를 찾아서 응답하게 하는 형태를 보통 캐시         를 사용한다고 합니다.
        캐시를 이용하면 DB에 대한 리소스를 절약할 수 있기 때문에 많은 시스템에서 자주 활용됩니다. (사실 이렇게 간단하진 않습니다만 간단하게 설명했습니다.)
        이렇게 캐시 할 부분을 자바 직렬화된 데이터를 저장해서 사용됩니다. 물론 자바 직렬 화만 이용해서만 캐시를 저장하지 않지만 가장 간편하기 때문에 많이 사용됩니다.
      - 자바 RMI(Remote Method Invocation)
        최근에는 많이 사용되지 않지만 자바 직렬화를 설명할 때는 빠지지 않고 이야기되는 기술이기 때문에 언급만 하고 넘어가려고 합니다.
        자바 RMI를 간단하게 이야기하자면 원격 시스템 간의 메시지 교환을 위해서 사용하는 자바에서 지원하는 기술입니다.
        보통은 원격의 시스템과의 통신을 위해서 IP와 포트를 이용해서 소켓통신을 해야 하지만 RMI는 그 부분을 추상화하여 원격에 있는 시스템의 메서드를 로컬 시스템의 메서드인 것처럼 호출         할 수 있습니다.
        원격의 시스템의 메서드를 호출 시에 전달하는 메시지(보통 객체)를 자동으로 직렬화 시켜 사용됩니다.
        그리고 전달받은 원격 시스템에서는 메시지를 역직렬화를 통해 변환하여 사용됩니다.
        자세한 내용은 작은 책 한 권 정도의 양이 되기 때문에 따로 한번 찾아보시는 것을 추천드립니다. ㄷㄷ..




continue  https://devlog-wjdrbs96.tistory.com/268
참고 https://techblog.woowahan.com/2550/
