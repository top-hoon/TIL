## JUnit
  - 단위테스트 도구. 외부 테스트 프로그램을 작성하여 sout으로 번거롭게 디버깅 하지않아도됨
  - 테스트 결과를 단순 텍스트로 남기는 것이 아니라 Test클래스로 남김 그래서 history 를 넘기는것도가능
  - 나중에해보자... 프로젝트할 때
  
  
  ex)
  ```` java
  ////
  public class Test{
    public int sum(int num1, int num2){
      return num1+num2;
    }
  }
  ////
  @Test
  public class sumtest{
    public void TestSum(){
      fail("madamada");
      }
  }
  ////
  @Test
   public void SumSum(){
    Test test = new Test();
    assertEquals(30, test.sum(10, 20))
      fail("madamada");
      }
  }
  ````
    - 테스트 클래스를 실행하는 방법은 테스트 클래스에서 마우스 오른쪽 버튼-> RunAs-> JUnitTest 선택
    - 단정문 좀 많음 나중에 
  
  
