# Maven 개요
  - 불필요한 설정을 최소화 한다는 개녕 아래 Ant와 같은 빌드 기능을 제공하고 구조화 된 빌드 기능을 통해 learning curve 및 재사용성 향상
### 장점
  - 의존성관리Good , 의존성 자동 업데이트, 저장소를 통한 라이브러리 일괄 관리
  - 모든 프로젝트에 걸쳐 쉽게 적용 가능한 일괄적인 사용법
  - 라이브러리 및 메타데이터 저장을 위한 지속적으로 확장되고 있는 저장소
  - 쉽게 작성 가능한 플러그 인을 통한 확장성
  - 동시에 다수의 프로젝트 핸들링 할 수 있는 쉬운 설정 기반의 메커니즘
  - 간단한 설정을 통한 배포 관리

### 단점
  - repository 관리의 불편함
  - Maven 프로젝트의 급속한 발전으로 central repository가 제공하는 라이브러리들이 급속히 증가하고 있으나 아직3rd 파티 라이브러리 등 미제공 라이브러리들이 있음
  - pom.xml 파일 관리 - 메이븐 프로젝트 관리에 대한 모든 내용이 pom.xml 파일에 담기게 되므로 길고 장황하게 될 수 있음

### Maven 디렉터리 구조
  - /pom.xml - 프로젝트 객체 모델. 해당 프로젝트에 대한 전반적인 정보를 갖는다.
  - /src/main/java Java 소스 파일 위치
  - /src/main/resources 배포할 리소스, XML, properties, …
  - /src/main/webapp 웹 어플리케이션 관련 파일 위치(WEB-INF,css 등)
  - /src/test/java 테스트 케이스 java 소스
  - /src/test/resources 테스트 케이스 리소스
  - /target 빌드 된 output이 위치하는 디렉터리
