## Logger - java.util.logging.Logger
  - WAS를 구축하고 운영하면서 특정 기능의 소요시간이나 운영상에서의 예외상황, 에러를 확인하고 싶은 상황이 자주있는데, 콘솔에 sout 로 확인할수있지만, 관리하고 운영하기에 콘솔은 불편하다
  - 그래서 Log형태로 저장해서 관리하는 것이 일반적이다.
  
    - 시스템운영기록, 디버깅, 시스템 에러 추적, 성능, 문제점 향상 등의 목적으로 사용
    - 어느정도까지 로그를 남기는가
      - 너무적은로그는 정확한 시스템의 상황을 파악하기 힘듬.
      - 너무많은로그는 빈번한 file I/O 의 오버헤드와 로그 파일의 백업 문제등 파생문제 발생 가능성이 있다.

## Java.util.logging

  - Java 에서 기본적으로 제공
  - 파일이나 콘솔에 출력가능
  - jre/lib/logging.properties 파일을 편집해서 로그의 출력 방식 로그 레벨을 변경할 수 있음(IDE console)
  - 오픈소스로 log4j(현재 이슈 발견됬다던데 지금은 어찌된지 모르것다)
  - 밑의링크에 사진 올려놓은것있음 그거보면 Handler를 참조하는데 Handler 에서 console로 할건지 File로 할건지 선택할 수 있다.
````java
  public class codeclass{
    private static final Logger LOGGER = LoggerFactory.getLogger(codeclass.class);  // Logger.gerLogger
    
    public static void main(String[] args){
      LOGGER.debug("?");    
    }
  }
````
  -  이렇게 찍으면 출력결과가 2번 찍힌다고 함. why?!!?!!?!!?!!?!
  -  아직 정확한 이유는 찾다가 말았음
````java
    public static void main(String[] args){
      LOGGER.debug("?");   ;
      System.out.println("?");  
    }
````
  - 이것도 2번찍힌다구함
 
## 해결방법
  -  그냥 String 클래스를 사용하는것이라고 함(아직 다른 방법은 찾아보지못함...

````java

    public static void main(String[] args){
      private static final Logger LOGGER = LoggerFactory.getLogger(codeclass.class.getSimpleName()); 
      LOGGER.debug("?");   ;
      System.out.println("?");  
    }
````  
  - 이렇게 해야 한번  찍힘.
  
  
  아직 프로젝트에서 사용해보지는 않아서 정확히는 모르겠지만 조금 더 알아볼 필요성이있고, 사용해보면 로그를 sout 보다 편하게 사용하는 날이 올것같다
  ㅎ
    
  // https://sdesigner.tistory.com/100 
  감사합니다..
