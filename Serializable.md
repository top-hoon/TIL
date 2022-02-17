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
continue  https://devlog-wjdrbs96.tistory.com/268
참고 https://techblog.woowahan.com/2550/
