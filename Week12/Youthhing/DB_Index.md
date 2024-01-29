# DB_인덱스

이번 교육 주제는 DB인덱스로 정했다. 교육팀 하면서 꼭 한 번은 다루고 싶던 주제이기도 했고 8기의 후반부라 부원들도 살짝 난이도 있는 주제를 정해도 이해가 쉽지 않을까 싶어서 해당 주제를 선택했다.

### 사례

우선, 단적인 상황에 대한 예시를 먼저 보자. 8기 코테이토 카페, CS교육 자료들이 업로드 되어있다.

![https://blog.kakaocdn.net/dn/cdmQwV/btsEb85sP0y/LzPiAuCHxqxE7jSUcNMkD1/img.png](https://blog.kakaocdn.net/dn/cdmQwV/btsEb85sP0y/LzPiAuCHxqxE7jSUcNMkD1/img.png)

이 많은 자료 중에 특정 단어, **SOP**에 대한 개념을 배웠던 것 같은데 기억이 나지 않아 해당 자료가 어디 있었지? 하며 찾아보는 중이다.

SOP라는 단어가 어디에 위치하는지 아무 정보도 없다면 어떻게 탐색해야할까?

시작부터 끝까지 순차적으로 탐색하며 존재하는지, 아닌지를 찾아야한다.

하지만, 아래와 같이 용어와 용어의 위치에 대한 정보를 나타내는 표가 있다면 어떨까?

![https://blog.kakaocdn.net/dn/l6ffc/btsEbDECLcu/pJaIVVHoSr6fuC5IXkNHz1/img.png](https://blog.kakaocdn.net/dn/l6ffc/btsEbDECLcu/pJaIVVHoSr6fuC5IXkNHz1/img.png)

용어가 사전순으로 정렬되어있으니 해당표의 **S**를 찾고, SOP라는 단어의 위치를 찾아 조회를 하면 된다.

이러한 표를 색인표, 인덱스라고 하는데 인덱스를 사용하면 **탐색 속도를 향상** 시킬 수 있다.

---

## **인덱스란?**

인덱스란, 데이터베이스의 테이블에서 특정 컬럼을 기준으로 빠른 조회를 위해 사용되는 DB의 자료구조를 인덱스라고 한다.

Member Table의 컬럼이, id, email, nickname, password가 있다고 가정할 때 email을 기준으로 인덱스를 만들면 아래와 같다.

![https://blog.kakaocdn.net/dn/IsCgA/btsD4TaUJ4k/tQqFyAiTBBmFwkXsrmlFIK/img.png](https://blog.kakaocdn.net/dn/IsCgA/btsD4TaUJ4k/tQqFyAiTBBmFwkXsrmlFIK/img.png)

왼쪽에는 email(String)값을 기준으로 사전 순으로 정렬되어있고, 오른쪽엔 해당 이메일을 가진 실제 데이터가 위치하는 데이터의 주소가 존재한다.

인덱스를 사용하고 사용하지 않을 때, 아래와 같은 쿼리문을 실행하면 어떻게 작동될까?

```sql
select * from member where email = 'aaaa@naver.com'
```

앞서 들었던 예시처럼, 인덱스를 사용하지 않으면 모든 데이터를 다 조회해야하기에 시간이 오래걸리지만, 인덱스를 사용하면 바로 페이지3의 3번째 레코드를 조회하면 된다.

**인덱스를 사용하면, 인덱스를 사용한 컬럼을 기준으로 데이터의 주소가 정렬되어 있고 바로 조회가 가능해 조회 속도가 빨라진다.**

# **Full Scan 방식**

인덱스를 사용하지 않는 경우엔, 데이터의 위치를 알 수 없기에 실제 데이터의 모든 테이블을 다 순차적으로 조회해야한다. 이 경우 N개의 데이터가 존재한다면, 평균적으로 모든 데이터를 탐색해야하기에 **O(N)의 시간복잡도**를 갖는다.

별도의 인덱스를 생성하지 않는 경우나, 데이터가 적어 인덱스를 사용하는 것으로 탐색 속도의 향상을 크게 시키지 못하는 경우 Full Scan 방식을 사용한다.

## **B - Tree 기반 인덱스**

우선, B-Tree가 무엇인지부터 이해하고 넘어가자.

### **B - Tree란?**

B-Tree는 Balanced Tree의 약자로, 균형 잡힌 트리를 이야기한다. 일반적으로 탐색을 도와주는 트리는 이분 탐색 트리, Binary Search Tree가 존재한다.

일반적인 BST의 경우엔, 아래와 같이 균형이 잡혀 평균 탐색 시간 O(logN)을 갖는다.

![https://blog.kakaocdn.net/dn/bZEGBt/btsD7lkHmcT/DLhhYNSKgBmvRbB31xBKA1/img.png](https://blog.kakaocdn.net/dn/bZEGBt/btsD7lkHmcT/DLhhYNSKgBmvRbB31xBKA1/img.png)

하지만, 아래와 같이 한쪽에 노드가 쏠려, 불균형한 트리가 형성된다면? **최악의 경우 O(N)의 시간 복잡도**로 탐색에 오랜 시간이 소요된다.

![https://blog.kakaocdn.net/dn/msTIa/btsD3ov0UkW/izh8s31i5SkrMCitqU65Ok/img.png](https://blog.kakaocdn.net/dn/msTIa/btsD3ov0UkW/izh8s31i5SkrMCitqU65Ok/img.png)

이러한 BST의 탐색 시간 불균형 문제를 해결하기 위해 등장한 자료 구조가 **B-Tree (Balanced Tree)**이다.

B-Tree는 BST와 달리 다음과 같은 특징을 갖는다.

### **B-Tree의 특징**

1. 모든 노드의 레벨이 동일하다.

2. 하나의 노드, 하나의 정보만 저장하는 것이 아닌 여러 정보를 저장할 수 있다.

3. 2개 이상의 자식을 가질 수 있다.

4. 균형이 잡혔기에 탐색 시 항상 O(logN)의 시간 복잡도를 갖는다.

따라서, 대부분의 RDBMS는 인덱스 구조에서 B-Tree 기반한 자료구조를 사용한다. (B+tree등)

### **데이터 탐색 (SELECT)**

B-Tree의 최상위 노드를 루트 노드, 중간 노드를 브랜치 노드, 제일 아래 노드를 리프노드라고 하며 루트 노드 부터

리프노드로 탐색을 하며 데이터의 주소를 찾는다.

id를 index로 하는 아래와 같은 B-Tree가 있다고 가정해보자.

![https://blog.kakaocdn.net/dn/xzL0B/btsEbyJ5sWa/z6NPiFR3GxuNjSaDRiLlhK/img.png](https://blog.kakaocdn.net/dn/xzL0B/btsEbyJ5sWa/z6NPiFR3GxuNjSaDRiLlhK/img.png)

id가 19인 회원의 데이터를 조회하고 싶다면, 19는 루트 노드 15 ~ 26사이의 값이므로, 1002 페이지를 찾아가서 조회를 한다. 불필요한 완전 탐색이 아닌, 필요한 방향을 향해 탐색해가기에 조회(Select)의 속도가 향상된다.

B-Tree 인덱스를 사용하면 조회 속도를 향상 된다.

### **데이터 삽입, 삭제, 갱신(INSERT, DELETE, UPDATE)**

B-Tree는 조회에서 성능향상을 띈다. 하지만 Write연산이 포함된 삽입, 삭제, 갱신에는 어떨까?

이 3가지 경우엔 비용이 커서 전반적으로 성능이 저하된다. 그 이유는 다음과 같다.

**삽입(insert)**

새로운 데이터를 삽입(Insert)할 때, 들어갈 페이지가 가득찼다면, B-Tree는 균형을 맞추기 위해 페이지를 **분할**한다. 이 과정에서 오버헤드가 발생해, 일반적인 Full Scan 방식보다 오버헤드가 크다.

**삭제(delete)**

삭제와 같은 경우 인덱스를 실제로 삭제하는 것이 아닌, 인덱스의 리프노드에 삭제 마킹을 한다. 이 경우 불필요한 데이터가 공간을 차지 하고 있기에 공간 활용도, 효율이 낮아진다.

**갱신(update)**

데이터를 수정하는 update의 경우 대부분의 DB에선, 값을 수정하는 것이 아닌, 레코드를 삭제 후 삽입 과정을 거친다. 따라서, 위의 두 연산이 모두 동반되기에 비용이 크다.

이 외에도 Hash Table을 사용하는 방식도 존재한다.

## **인덱스의 종류?**

그렇다면, 인덱스의 종류는 어떤 것들이 있을까?

index를 사용하는 컬럼을 기준으로 인덱스 루트노드, 브랜치 노드는 정렬되어 있다.

실제 데이터가 정렬되어 있는가를 기준으로 Clustered index와 Non-Clustered Index로 구분된다.

### **Clustered Index**

Cluster란 군집된, 모여있는 이란 뜻을 가진 단어로 index에 해당된 데이터들이 실제로도 모여있는 index를 말한다.

가령, 아래와 같은 부원 정보가 있다고 가정하자.

![https://blog.kakaocdn.net/dn/6jBwb/btsEaMB7pm5/1PuQW0fWh84X0shfrUQW1k/img.png](https://blog.kakaocdn.net/dn/6jBwb/btsEaMB7pm5/1PuQW0fWh84X0shfrUQW1k/img.png)

id를 기준으로 clustered index를 구성하면, index는 id값을 기준으로 오름차순으로 정렬될 것이다.

이 때, 실제 데이터도 id의 순서와 동일하게 정렬된 index를 clustered index라고 한다.

B-Tree기반으로 clustered index를 구성하면 다음과 같은데,

![https://blog.kakaocdn.net/dn/bjYReq/btsEcavshLB/ogCK5hCzXwE2qkfvY3soI0/img.png](https://blog.kakaocdn.net/dn/bjYReq/btsEcavshLB/ogCK5hCzXwE2qkfvY3soI0/img.png)

index를 기준으로 루트노드에 id 값이 1, 12, 20이 존재하면 1~11에 해당하는 데이터는 1000페이지에 존재한다.

즉, 리프노드에 데이터의 주소가 아니라 실제 데이터가 index를 기준으로 정렬되어 존재한다.

따라서, clustered index 방식은 실제 데이터가 인덱스의 순서에 맞게 리프노드에 정렬되어 있기에, **테이블 당 1개의 인덱스**만 존재할 수 있다.

또한, MySQL의 경우 별도로 인덱스를 생성하지 않고 `CREATE TABLE`명령어로 테이블을 생성할 경우 **PK를 기준으로 자동으로 Clustered Index를 생성**한다.

따라서, Clustered Index의 특징을 정리하면 다음과 같다.

**Clustered Index의 특징**

1. 실제 데이터가 항상 정렬되어 있어 조회 속도가 매우 빠르다.

2. 실제 데이터가 항상 정렬되어 있어, 업데이트가 자주 되는 테이블엔 부적합하다.

3. B-Tree를 사용 시, 리프노드엔 실제 데이터가 존재한다.

4. 실제 데이터가 정렬되어 있기에 테이블 당 1개만 존재할 수 있다.

5. PK를 기준으로 자동으로 생성된다.

### **Non - Clustered Index**

clustered index 방식이 index를 기준으로 실제 데이터가 정렬되어 있는 방식이었다면, non - clustered index 방식은 index와 별개로 실제 데이터는 정렬되어 있지 않는 방식을 이야기한다.

부원 테이블에서 name 컬럼을 기준으로 B-Tree를 사용해 Non-Clustered Index를 구성하면 다음과 같다.

![https://blog.kakaocdn.net/dn/bByg5H/btsD7mX9KZw/6hkDBf4x7Y47OaWV8Icfvk/img.png](https://blog.kakaocdn.net/dn/bByg5H/btsD7mX9KZw/6hkDBf4x7Y47OaWV8Icfvk/img.png)

데이터가 실제로 정렬되어 있지 않기 때문에, 리프노드에는 실제 데이터가 아닌, 실제 데이터의 **주소**가 들어있는 **별도의 인덱스 페이지**가 존재한다.

예를 들어 '성연'님의 데이터를 조회하고 싶다면, 인덱스 페이지 101페이지를 찾고 찾으려는 데이터의 실제주소를 확인하고 실제 주소를 참조해 데이터를 조회하는 것이다.

Non-Clustered Index의 경우 `CREATE INDEX`라는 명령어를 통해 생성할 수 있다. 또한, 실제 데이터가 정렬되어 있지 않기에 Clustered Index에 비해 조회속도는 느리지만, WRITE연산 시 데이터를 재정렬하지 않아 WRITE는 상대적으로 유리하다.

**Non - Clustered Index의 특징**

1. 실제 데이터가 그대로 존재한다. 인덱스를 기준으로 정렬되어 있지 않다.

2. 별도의 인덱스 페이지를 생성해야하기에 추가적인 공간이 필요하다.

3. 실제 데이터를 정렬하지 않기에, 테이블당 여러개의 인덱스를 생성할 수 있다.

4. 리프 노드엔 실제 데이터의 주소 정보가 들어간다.

## **어떤 컬럼을 기준으로 인덱스를 정할까?**

그렇다면, 인덱스를 생성할 때 어떤 컬럼을 기준으로 생성하면 좋을까? 비즈니스 로직에 맞게 조회 등이 잦은 컬럼을 사용하는 것도 좋지만 한 가지 기준으론 카디널리티라는 개념이 존재한다.

### **카디널리티(Cardinality)란?**

특정 컬럼의 데이터 중 unique한 값의 개수를 의미한다. 예를 들어 아래와 같은 테이블에 7개의 레코드가 있다고 할 때 카디널리티를 계산해보자.

![https://blog.kakaocdn.net/dn/dgGob4/btsD3pn69ei/0vgsPYrH7tOHTrHazFbZB1/img.png](https://blog.kakaocdn.net/dn/dgGob4/btsD3pn69ei/0vgsPYrH7tOHTrHazFbZB1/img.png)

ID 컬럼의 경우 중복된 값이 존재하지 않기에 7

Name의 경우 '민재'라는 이름이 중복되기에 6

Position의 경우 BE, FE, PM, DESIGN으로 구성되어 4

전화번호는 모두 다르기에 7이 된다.

인덱스를 정할 때는 **카디널리티가 큰 컬럼일수록** 인덱스에 적합한 컬럼이다. 즉, 중복도가 낮은 컬럼을 index로  사용하면 적합하다.

## **정리**

지금까지 DB인덱스에 대한 내용을 다뤘다.

인덱스의 장점으로는 **조회 속도를 향상**시킨다는 장점이 있다.

하지만, 인덱스를 사용하면

1. 인덱스 페이지 자체가 공간을 차지해 공간 효율성이 떨어진다는 점.

2. WRITE 연산 시 실제 데이터 외에도 인덱스 페이지 또한 수정해야한다는점에서 비용이 비싸다는점

3. 데이터가 적을 경우엔 인덱스보다 Full Scan이 유리하기 때문에 데이터의 개수를 고려해야한다.

따라서 조회 연산이 많이 사용되는 경우, Data가 많은 경우, 조건절 (Where, Join, Order By절이 많이 사용되는 경우)에는 인덱스 사용을 고려해보자.

