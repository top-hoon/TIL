## 예를 들어  http://localhost:8081/sports/mem/memFixedMemberList.do 다음과 같은 주소를 가진 웹 페이지를 개발 중이라고 할 때, 코드에서 주소 정보를 가져오고 싶을때 
   HttpServletRequest를 사용하여 추출할 수 있음
  - request.getScheme() : http또는 https를 반환
  - request.getServerName() : localhost를 반환
  - request.getServerPort() : 8081를 반환
  - request.getRequestURL() : 전체주소정보 http://localhost:8081/sports/mem/memFixedMemberList.do 를 반환

## 예
     final String strUrl = request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort();
     if (strUrl.contains("localhost")) {
      SimpleDateFormat yyyyMMddHHmmss = new SimpleDateFormat("yyyyMMddHHmmss");
      String ymd = yyyyMMddHHmmss.format(new Date());
      ITEM_CARD_NUMBER 	="941009******2173";
      ITEM_APPROVAL_ID 	=ymd.substring(4);
      ITEM_SALES_DATE 	= ymd;
      ITEM_TRADE_UNIQUE_ID ="500000967246";
      ITEM_ISSUE 			="1300삼성카드";
      ITEM_PURCHASE 		="0006삼성카드사";
      ITEM_SALES_TIME		= "1944";	
  
    - 이런식으로 사용할 수 있음
