# 의존 객체 자동 주입(Automatic Dependency injection)
  - 의존 객체 자동 주입(Automatic Dependency Injection)은 스프링 설정파일에서 혹은 태그로 의존 객체 대상을 명시하지 않아도
    스프링 컨테이너가 자동적으로 의존 대상 객체를 찾아 해당 객체에 필요한 의존성을 주입하는 것을 말한다.
  - @Resource, @Autowired, @Inject 이 태그들의 차이점은 의존 객체를 찾는방식이 다르다.

## @Resource
  - ex) 	
````java 
    @Resource(name = "egovMessageSource")
    EgovMessageSource egovMessageSource;
````
  - java 에서 지원하는 어노테이션 특정 프레임 워크에 종속적이지않다.
  - 찾는순서 : 이름 -> 타입 - > @Qualifier - > 실패
  - name 속성의 이름을 기준으로 찾음 없으면 타입, 없으면 @Qualifier 어노테이션의 유무를 찾아 그 어노테이션이 붙은 속성에 의존성을 주입한다.
  - xml에 <context:annotation-config/> 구문 설정해야함
  - 사용할 수 있는 위치 : 멤버변수, setter 메소드
  
## @Autowired
  - ex) 	
````java
    @Autowired
    MemberService memberService;
````
  -  스프링에서 지원하는 어노테이션! (스프링아니면안댐)
  -  찾는순서 : 타입 -> 이름 -> @Qualifier -> 실패
  -  @Autowired 는 주입하려고 하는 객체의 타입이 일치하는지를 찾고 객체를 자동으로 주입한다.
  -  만약 타입이 존재하지않으면 @Autowired 에 위치한 속성명이 일치하는 bean을 컨테이너에서 찾는다. 그리고 이름이 없을경우, @Qualifier 어노테이션의 유무를 찾아 그 어노테이션이 붙은 속성에 의존성을 주입한다. 
  -  사용할 수 있는 위치 : 멤버변수, setter메소드, **생성자, 일반 메소드에 적용가능

## @Inject
  - java 에서 지원하는 어노테이션 특정 프레임 워크에 종속적이지않다.
  - @Inject를 사용하기 위해서는 maven이나 gradle에 javax 라이브러리 의존성을 추가해야한다.
  -  사용할 수 있는 위치 : 멤버변수, setter 메소드, 생성자, 일반 메소드에 적용 가능
  
## @Qualifier
  -  처음보는것같다.
  -  타입이 동일한 bean 객체가 여러개 있으면 spring이 exception을 생김. @Autowired 를 동일한 타입에 쓴 부분이 있다면! 어떤 bean 을 주입해야하는지 모르기 때문에
  -  사용방법 
```` java

<context:annotation-config>
    <-- Member member = new Member() -->
    <bean id="member1" class="example.Member">
        <qualifier value="m1"/>
    </bean>
<context:annotation-config/>

pubic class MemberDao{  
    @Autowired  @Qualifier("m1")
    private Member member;       

    public void setMember(Member member){      
        this.member = member;  
    }
}
  //https://velog.io/@sungmo738/Resource-Autowired-Inject-%EC%B0%A8%EC%9D%B4
  //감사합니다.
  
  
````















