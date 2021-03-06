## index
 - 인덱스란 추가적인 쓰기 작업과 저장 공간을 활용하여 데이터베이스 테이블의 검색 속도를 향상시키기 위한 자료구조이다.
 - 만약 우리가 책에서 원하는 내용을 찾는다고 하면, 책의 모든 페이지를 찾아 보는것은 오랜 시간이 걸린다. 
 - 그렇기 때문에 책의 저자들은 책의 맨 앞 또는 맨 뒤에 색인을 추가하는데, 데이터베이스의 index는 책의 색인과 같다.
 - 데이터베이스에서도 테이블의 모든 데이터를 검색하면 시간이 오래 걸리기 때문에 데이터와 데이터의 위치를 포함한 자료구조를 생성하여 빠르게 조회할 수 있도록 돕고 있다.
 - 조회를 하는 작업은 다 성능이 좋아진다. select외에도 update delete의 성능도 이유는 당연히 delete, update도 조회를 하고 하기 때문
 - index를 사용하지 않은 컬럼을 조회해야하는 상황이라면 fullscan을 수행해야하는데 그러면 처리속도가 떨어진다.

### 관리
 - dbms는 index를 항상 최신의 정렬된 상태로 유지해야 원하는 값을 빠르게 탐색할 수 있다.
 -  그래서 인덱스가 적용된 컬럼에 insert, delete, update 를 수행하면 밑의 연산을 추가적으로 해주어야 하며 그에 따른 오버헤드발생
 - insert : 추가하는 데이터에 대한 인덱스를 추가해야함
 - delete : 삭제하는 데이터의 인덱스를 사용하지 않는다는 작업을 진행
 - update : 기존의 인덱스를 사용하지않는다고 처리하고 갱신된 데이터에 인덱스를 추가

### 장점
 - 테이블조회속도 그에따른 성능 향상
 - 시스템 부하를 줄임
> **<h3>사용하면 좋은 경우</h3>**
>  - 규모가 작지않은테이블
>  - insert, update, delete 가 자주발생하지않는 컬럼
>  - join이나 where절에 order by에 자주사용되는 컬럼
>  - 데이터의 중복도가 낮은 컬럼
### 단점
 - 인덱스를 관리하기 위해 10%에 해당하는 저장공간이 필요
 - 인덱스를 관리하기 위해 추가작업이 필요
 - 인덱스를 잘못 사용할 경우 오히려 성능이 저하되는 역효과 발생
> **만약 CRU 가 빈번한 속성에 인덱스를 걸면 인덱스의 크기가 비대해져서 성능이 오히려 저하되는 효과가 발생할 수 있음**


## Clustered Index VS Non Clustered Index
 - Clustered Index는 개발자가 설정하는 Index가 아닌 MySQL이 자동으로 설정하는 Index이다.
 - 테이블에 Auto increment 값으로 pk가 있다면 해당컬럼이 Clustered Index가 된다.
 - 만약 해당 PK가 없다면 컬럼중에 Unique컬럼을 Clustered Index로 선정한다.
 - unique컬럼도 하나도 없다면? MySQL에서 내부적으로 Hidden Clustered Index Key (row ID)를 만들어 Clustered Index로 사용한다.
 - MySQL에서 자동으로 설정되는 Clustered Index는 최대 효율을 위해 중복이 최대한 발생하지 않는 컬럼을 사용하게 된다.
 - Non Clustered Index 는 개발자가 설정
 - 멀티컬럼 index 의 경우에는 최대 16개의 컬럼을 사용할 수 있고, 테이블당 인덱스의 개수는 최대 64개까지 가능! (+Clustered Index)= 65개까지 가능

## 속도 비교
![index_speed](https://user-images.githubusercontent.com/79463595/160515123-8c6add03-3fc6-413d-b884-0d744b56bbc3.png)
 - 보는것과 같이 데이터양이 많아지면 많아질수록 fullscan보다 index scan이 더 빠르다
 - 하지만 특정 데이터양 전까지는 fullscan 이 더 빠른것을 확인할 수 있다.
  ### why?
    - Index를 통한 검색은 B+Tree에서 leaf node까지 찾아 내려간 후 해당 데이터를 찾기위해 disk로 접근한다.
    - Full scan은 이렇게 B+Tree를 찾아가는 과정이 없다. 디스크로 가서 바로 모든 데이터를 읽어온다.
    - 데이터 양이 많지 않거나, Index가 효율적으로 설정되어있지 않은 경우 오히려 Table Full scan이 더 빠르다.
    - 그리고 Index Full Scan이 실행되는 경우 Index Full Scan의 데이터가 테이블의 모든 데이터의 양과 비슷한 경우에도 역시 Table Full Scan이 더 빠를 수 있다.

## index의 자료구조
 - 인덱스를 구현하기 위해서는 다양한 자료구조를 사용할 수 있는데, 가장 대표적인게 해시테이블, b+tree이다.

### 해시테이블
 - 해시테이블은 key,value로 데이터를 저장하는 자료구조 중 하나로 빠른 데이터 검색이 필요할 때 유용하다. 해시테이블은 key값을 이용해 고유한 index를 생성하여 그 index에 저장된 값을 꺼내오는구조
 - 해시 테이블 기반의 DB 인덱스는 (데이터=컬럼의 값, 데이터의 위치)를 (Key, Value)로 사용하여 컬럼의 값으로 생성된 해시를 통해 인덱스를 구현하였다. 
 - 해시 테이블의 시간복잡도는 O(1)이며 매우 빠른 검색을 지원한다. 하지만 DB 인덱스에서 해시 테이블이 사용되는 경우는 제한적인데, 그러한 이유는 해시가 등호(=) 연산에만 특화되었기 때문이다. 
 - 해시 함수는 값이 1이라도 달라지면 완전히 다른 해시 값을 생성하는데, 이러한 특성에 의해 부등호 연산(>, <)이 자주 사용되는 데이터베이스 검색을 위해서는 해시 테이블이 적합하지 않다.
 - 이러한 이유로 데이터베이스의 인덱스에서는 B+Tree가 일반적으로 사용된다.

### b-tree
 - b+tree를 보기전에 b-tree를 한번 보자!


![1](https://user-images.githubusercontent.com/79463595/160510537-b016c215-79f9-4dad-9b23-d693ab43a331.png)
 - b-tree의 핵심은 데이터가 정렬된 상태로 유지되어 있다는것
 - 사각형들을 노드라고 부름
 - 가장상단의 노드를 루트노드, 중간노드를 브랜치노드, 아래의 노드들을 리프노드라 부름

![다운로드](https://user-images.githubusercontent.com/79463595/160510790-5102b1b0-82c4-4430-85bb-3eed681bc0af.jpg)
 - 한 노드당 자식 노드가 2개이상 가능하다
 - key값을 이용해 찾고자 하는 데이터를 트리구조를 이용해서 찾는것이다.
 - B-tree의 장점 한가지는 어떤 값에 대해서도 같은시간에 결과를 얻을 수 있다. 인데 이것을 균일성이라 부름
 - 예시에서 리프노드에있는 77이나 97이나 찾는 시간은동일할것 약간의 차이는 있지만 O(logN)이라는 시간복잡도를 구할 수 있다.
 - 하지만 선형탐색일때는 리프노드에 있는 값만 따져도 하나하나 체크 할수밖에 없고 시간은 더욱 소요됨
 - b-tree는 균형트리이다 균형트리는 루트로-리프의 거리가 일정한 트리구조를 뜻하고 트리중에서 성능이 안정되어있지만 테이블의 갱신의 반복으로 균형이깨지고 성능도 악화됨
 - 균형을 회복하는 기능이있다고는 하지만 갱신빈도가 높은 테이블에 작성되는 인덱스는 인덱스 재구성을 해서 균형을 되찾는 작업이 필요하다

### b+tree
 - b+tree는 b-tree의 확장개념이다. b+tree 의경우 브랜치에 key만 담아두고 data는 담지않는다. 리프노드에만 <key,data>를 저장하고 리프노드끼리 linked list로 연결되어있음

### 장점
 - 리프노드를 제외하고 데이터를 담아두지않기 때문에 메모리를 더 확보함으로써 더 많은 key 들을 활용가능 하나의 노드에 더 많은 key들을 담아서 트리의 높이는 더 낮아진다!(cache hit를 높일수 있음)
 - 풀 스캔 시, B+tree는 리프 노드에 데이터가 모두 있기 때문에 한 번의 선형탐색만 하면 되기 때문에 B-tree에 비해 빠르다. B-tree의 경우에는 모든 노드를 확인해야 한다


![다운로드](https://user-images.githubusercontent.com/79463595/160512878-df2bcbe8-ca94-42c2-b4bf-0c1fb80a6f63.png)
 - 더 복잡해보인다
 - 같은레벨의 노드들끼리 Linked List 가 아닌 Double Linked List 사용 자식이랑은 SingleLinkedList 사용
 - key의 범위마다 찾아가야할  페이지 넘버가 있는데 해당 페이지 넘버를 통해 곧바로 다음 노드로 넘어간다.
 - 리프노드에 갔을 때 디스크에 존재하는 데이터의 주소값을 구할 수 있고 Linked List를 통해 탐색도 가능하다















#### 참고
 - https://blog.jcole.us/2013/01/10/btree-index-structures-in-innodb/
 - http://www.btechsmartclass.com/data_structures/b-trees.html
 - https://blog.jcole.us/2013/01/10/btree-index-structures-in-innodb/
 - https://kyungyeon.dev/posts/66 - 데이터 삽입에대한 설명도 있음
 - https://zorba91.tistory.com/293









