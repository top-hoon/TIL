## promise
 - 프로미스는 자바스크립트 비동기 처리에 사용되는 객체 여기서 자바스크립트의 비동기 처리란 특정코드의 실행이 완료될 때까지 기다리지 않고 다음 코드를 먼저 수행하는
  자바스크립트의 특성을 의미 
### 왜 필요한가
 - 프로미스는 주로 서버에서 받아온 데이터를 화면에 표시할 때 사용
 - 데이터를 요청-> 다 받아오기전에 화면에 데이터를 표시하려고 하면 오류가 발생하거나 빈화면 -> 이러한 문제점을 해결하기 위한 방법들중 하나가 프로미스
### 프로미스의 3가지 상태(states)
 - 프로미스를 사용할 때 알아야하는 가장 기본적인 개념이 바로 프로미스의 상태
 - 여기서 말하는 상태란 프로미스의 처리과정을의미 new promise()로 프로미스를 생성하고 종료될 때까지 3가지 상태를 가짐
 
    ### 1. Pending(대기)
````js
new Promise(); 
````
     - promise 메서드를 호출하면 pending상태가됨
````js
new Promise(function(resolve,reject){

)}
````
     - 호출할 때 콜백함수를 선언할 수 있고, 콜백함수의 인자는 resolve, reject이다
 
   ### 2. Fulfiled(이행)
````js
new Promise(function(resolve,reject){
 //resolve();
 resolve(data);
)}
````
    - 콜백함수의 인자 resolve를 이렇게 실행하면 이행 상태가 됨

   ### 3. Rejected(실패)
````js
new Promise(function(resolve,reject){
 reject(new Error('Request is failed));
)}
````
 - 실패상태가 되면 실패한 이유(실패 처리의 결과 값)을 catch로 받을 수 







 - https://joshua1988.github.io/web-development/javascript/promise-for-beginners/
