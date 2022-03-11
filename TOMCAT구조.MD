## WEB SERVER
    - http 프로토콜 기반으로 web client (browser) 로부터의 요청을 서비스하는 기능을 담당하는 프로그램. 
    - ex) apache ..
    - 역할: html, image, xml 등에 대한 처리 담당
    
## WEB Application
    - 여러 web client의 요구를 web서버가 감당할수없는 기능을 구조적으로 web서버와 분리하기 위해 만들어진것으로 web application server(WAS) 라고 한다. 
    - ex) tomcat, weblogic, jeus, jboss.. 
## 웹 서버와 와스의 차이점
    - WebServer와 WAS는 사용 목적이 다르다. 
    - web서버 : html, 이미지 요청등 정적데이터 요청을 처리하는데 빠르다
    - WAS : servlet, jsp 등 비즈니스 로직을 수행하는데 적합하다.
    - 그렇다고 WAS가 html, 이미지등의 요청을 처리하지 못하는것은 아님 다만 처리속도가 web서버에 비해 느리기 때문
    - 서로 다른 강점을 가지고 있어서 합해서 사용하는 것이 대부분이다. WebServer+WAS
## Tomcat
  - 톰켓은 jsp환경을 포함하고 있는 servlet 컨테이너이다.
  - 서블릿 컨테이너는 사용자 입장에서 서블릿을 유지하고 호출하여 실행하는 쉘이다.
  - 톰켓은 크게 3가지 컨테이너로 구분한다.

1. Stand-alone servlet containers (tomcat의 기본모드)
    - 내장된 웹서버 기능 사용
    - 기능면에서 java web server의 부분인 servlet 컨테이너와 java 근간의 웹 서버 사용.
2. In-process servlet containers
    -  servlet컨테이너는 웹서버 플러그인와 java 컨테이너 구현 
    - 웹서버 플러그인은 웹서버 주소공간 내 JVM을 열고 그 안에서 java컨테이너가 실행되도록 한다.
    - 다중스레드의 단일 프로세스 서버에 적합하고 퍼포먼스도 좋지만 확장성에 한계가 있음.
3. Out-of-process servlet container
    - 웹서버 플러그인과 웹서버의 외부 JVM에서 실행하는 java컨테이너 구현
    - 웹서버 플러그인과 java 컨테이너 JVM은 몇몇 IPC(보통은 tcp/ip소켓)를 사용해서 통신
    - out of process엔진반응 시간은 in process만큼 좋지 않지만 out of process엔진은 확장성과 안정성면은 좋다.

## Tomcat Directory 구조
  - └ bin - tomcat 실행에 필요한 서버동장 제어 스크립트 및 실행파일 (binary) , 
  - └ common -웹어플리케이션에서 공통적으로 사용하는 클래스 파일
  - └ conf - 서버 전체 설정파일 폴더 (server.xml 등 )
  - └ logs - 예외 발생 사항 등의 로그 저장
  - └ server - 서버에서 사용하는 클래스 라이브러리 
  - └ temp - 임시 저장용 폴더 
  - └ webapps - 웹 어플리케이션 루트 폴더 
  - └ work - jsp 파일을 서블릿 형태로 변환한 java 파일과 class파일 저장 




 // 참조 : https://cheershennah.tistory.com/111
 덜 쓴 부분이 있으니 나중에 참고








