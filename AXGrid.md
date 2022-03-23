# AXGrid
  - 사용해본적 없다 
  
스프링 프레임워크 사용한 예제
https://offbyone.tistory.com/112
기본사용법
https://offbyone.tistory.com/55#rp


화면진입로그 등록 : 로그종류, 회원번호, 로그상세
saveWorkingLog("FL", "", "");

````js
var MST_OrganInfo = "${OrganInfo}"  // 로그인 했을 때 그 아이디 권한으로 구분해서 데이터 불러오는 형식!
var myValidator = new AXValidator();  // 나의 문법등 필요한것들 알아서 체크해주는것
var myGrid = new AXGrid();  // 그리드 객체 생성
sort        : true, //정렬을 원하지 않을 경우 (tip
remoteSort  : false,  // remote 가 서버를 뜻함! 그래서 만약에 header클릭했을 때 정렬을 서버에서 한번 더 거쳐서 정렬해서 가져온다는거
colHeadTool : false, // column tool use
fitToWidth  : false, // 너비에 자동 맞춤
colHeadAlign: "center", // 헤드의 기본 정렬 값.colHeadAlign 을 지정하면 colGroup 에서 정의한 정렬이 무시되고 colHeadAlign : false 이거나 없으면 colGroup 에서 정의한 속성이적용됨

                    colGroup: [ // key 값이 데이터베이스의 컬럼값이고, label은 내가 지정하는 ui에 나오는 이름
                    	{key:"ROWNUM",   label:"No.", width:40,    align:"right"},
                   	{key:"SAPP_DATE",   label:"결제일", width:80,    align:"center", formatter:"custom-date"},
                      ],
                      colHead :{
                        onclick:funcion(){
                        myGrid.click(0);  // click 함수를 만든거고, myGrid.click(0) 0번째, 그러니까 첫번째 컬럼이 선택되게끔 만드는것!
                        }
                      }
                       body : {
                        onclick: function(){ 
                        	//selectRow(this.item);
                        },
	                    ondblclick: function () {
	                    	moveFixedMemInfo(this.item.MEM_NO); //회원관리 > 회원등록관리로 이동   // moveFixedMemInfo 라는 함수를 만들어 놓았고, ()안의 mem_index를 매개변수로 메서드 실행시켜서 페이지 이동하는거!
	                    }   
                    },
                .....
                myGrid.setList({  // 이 그리드 안에서 ajax로 list 불러오기
                    ajaxUrl: sAjaxUrl, //"/sports/finish/searchCardPayStat_JN.json",
                    ajaxPars: gridParameter,  // 이건 파라미터고
                    
````

  - 대충 이 정도만 생각하고 나중에 실제로 그리드 사용할 때 api문서 찾아보면서 하면 좋을듯 싶다!

