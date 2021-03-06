## eGovFrame
  - 정확한 명칭은 전자정부 표준 프레임워크(eGovFrame)
  - 정보시스템 개발을 위해 필요한 기능 및 아키텍쳐를 미리 만들어 제공함으로써, 효율적인 어플리케이션 구축을 지원한다. 
  - 공공사업에 적용되는 개발 프레임워크의 표준 정립으로 응용 SW 표준화, 품질 및 재사용성 향상을 목표로 한다.
  - java기반의 정보시스템 구축에 활용할 수 있는 개발 운영 표준 환경을 제공하기 위해 개발된 프레임워크이다.
  
## 특징
  - 오픈소스기반의 범용화되고 공개된 기술의 활용으로 특정 사업자에 대한 종속성 x
  - 상용 솔루션 연계가 가능한 표준을 제시해 상호 운용성을 보장 - 어제 부장님이 말씀하신거
  - 표준화 지향 
  - 변화 유연성 : 각 서비스의 모듈화로 교체가 용이해 인터페이스 기반 연동으로 모듈간 변경영향을 최소화 한다.
  - 이클립스 기반의 모델링(UML, ERD), 에디팅 , 컴파일링, 디버깅 환경제공
  
## 전자정부 프레임워크 기본구조
  - java : java source
  - resource : 프레임워크 설정 파일 및 sql 파일
    - egovProps: 시스템 설정 파일
    - message : 공통 메세지파일/ 자주사용되는 출력 메세지를 공통으로 관리하여 일관성있게 처리, 고객의 요청에 따라 변경시 일괄 처리되므로 매우유용함 알림 메시지 또한 웹 취약점에 노추됨 시스템쪽에서 처리하여 메시지 코드만 리턴하므로 취약점에도 유리함.
    - spring : ibatis의 sql 파일 및 설정 파일이 있다.
    - log4j.xml : 콘솔에 출력되는 로그의 출력범위를 설정할 수 이싿.
  - webapp 
    - config : servlet 설정 파일
    - jsp : controller 에서 forward 라 하는 jsp 파일 위치, Servlet 설정 파일에서 설정한다.
    - lib : 라이브러리 파일
    - web.xml : 웹 어플리케이션 환경파일  웹 어플리케이션 시작할 때 메모리에 로딩됨. - filter, servlet, 시작페이지 등을 설정



## 현재 우리 프로젝트 구조
  - egovframe
    - mapper -> interface mapper (필요한 메서드 이름 적고 그거 xml 파일에 쿼리문 따로 박아서 관리하기)
      - xml 파일에는 태그를 열고<insert id = mapper에 적은 필요한 메서드>를 써주고 원하는 쿼리를 적어주면 된다.
    - service -> interface service 필요한 메서드 선언
      - 1. 서비스 사용하기 impl 파일 상속받은서비스 + impl -> service를 상속받고 필요한 메서드 사용하기.  @Service(해당 인터페이스 이름 써주면됨)어노테이션사용
      - 2. mapper 사용하기 @Resource(name= mapper 이름)어노테이션 쓰고 객체 선언해줘서 사용하면된다.
    - controller @Resource(name=서비스 이름) 해서 객체 생성
      - @RequestMapping(value='보여주고싶은 view단 경로') 하고 시작
    - jsp 파일 -> html 만들고 연결하는 ajax 라든지.. 사용해서 view단 꾸미면 끝!
	
````java
	//filter
	<filter-mapping>
		<filter-name>encodingFilter</filter-name>
		<url-pattern>*.do</url-pattern>
		<url-pattern>*.c</url-pattern>
		<url-pattern>*.p</url-pattern>
		<url-pattern>*.json</url-pattern>
		<url-pattern>*.jsp</url-pattern>
		<url-pattern>*.jiro</url-pattern>
		<url-pattern>*.cm</url-pattern>
		<url-pattern>*.desk</url-pattern>
	</filter-mapping>
````
## HashMap
````java
  HashMap<String,String> map1 = new HashMap<String,String>();//HashMap생성
  HashMap<String,String> map2 = new HashMap<>();//new에서 타입 파라미터 생략가능
  HashMap<String,String> map3 = new HashMap<>(map1);//map1의 모든 값을 가진 HashMap생성
  HashMap<String,String> map4 = new HashMap<>(10);//초기 용량(capacity)지정
  HashMap<String,String> map5 = new HashMap<>(10, 0.7f);//초기 capacity,load factor지정
  HashMap<String,String> map6 = new HashMap<String,String>(){{//초기값 지정
    put("a","b");
}};
  // containskey 사용하기!
  public static void main(String[] ar){
	Map<String,Integer> map=new HashMap();
	map.put("zl1", 100);
	map.put("zl2", 200);
  
	if(!map.containsKey("zl1"))	//키가 map 안에 들어있는지 확인하고 있으면 안넣고 없으면 넣는다.
		map.put("zl1", 300); 
  
	System.out.println(map);
	System.out.println(map.get("zl1"));   //100
	System.out.println(map.get("zl2"));   //200
}
 // containskey 와 비슷한걸로 containsValue 가 있움 당연히 value값 확인하는고 
  
 // putIfAbsent :  둘다 확인하는것이 있다
	if(!map.containsKey("zl1"))	//키가 map 안에 들어있는지 확인하고 있으면 안넣고 없으면 넣는다.
		map.put("zl1", 300); 
    // 지금까지는 이렇게 했었지만 
  
    map.putIfAbsent("key2",300);  // - Key 값이 존재하는 경우 Map의 Value의 값을 반환하고, Key값이 존재하지 않는 경우 Key와 Value를 Map에 저장하고 Null을 반환해줌
  
 // putAll(Map에다가 다른 Map을 포함시키는것)
  
  public static void main(String[] ar) {
	Map<String,Integer> map1=new HashMap();
	Map<String,Integer> map2=new HashMap();
	//map1 put
	map1.put("map1-key1", 100);
	map1.put("map1-key2", 200);
		
	//map2 put
	map2.put("map2-key3", 300);
	map2.put("map2-key4", 400);
		
	System.out.println("map1:"+map1);
	System.out.println("map2:"+map2);
		
	//map2에 map1을 합침
	map2.putAll(map1);
	System.out.println("map2 includes map1:"+map2);
		
	//map1의 키, 값 변경
	map1.put("map1-key1", 1000);
	//map2에는 영향 없음.
	System.out.println("map2 includes map1:"+map2);
}
  
  // 결과값;
  map1:{map1-key1=100, map1-key2=200}
  map2:{map2-key4=400, map2-key3=300}
  map2 includes map1:{map2-key4=400, map1-key1=100, map1-key2=200, map2-key3=300}
  map2 includes map1:{map2-key4=400, map1-key1=100, map1-key2=200, map2-key3=300}
  // map을 통째로 인자로 넘겨줄때에는 putAll 메소드를 사용하면됨 키와 값의 자료형이 같은 Map이어야한다! 다른 자료형의 키, 값은 못받음 에러남
  // putAll 말고 생성자이용해서 생성돠 동시에 map의 데이터를 바로 넘길 수도 있음
  Map<String, String> mapp2 = new HashMap(mapp1)
  
  // https://reakwon.tistory.com/151 참조
	
	
````
  

  
  
  
