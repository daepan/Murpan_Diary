서버를 개발 할 때 데이터베이스를 사용하면 웹 서비스에서 사용되는 데이터를 저장하고,
효율적으로 조회하거나 수정할 수 있다. 
그래서 MySQL, OracleDB와 같은 RDBMS(관계형 데이터베이스)를 사용했다
하지만 이 RDBMS의 2개가 있다

1. 데이터 스키마의 고정적인 부분
2. 확장성 문제

1번에 데이터 스키마에서 스키마의 뜻은 데이터베이스에 어떤 형식의 데이터를 넣을지에대한 정보를 
말한다.
예를 들어 회원 정보 스키마라면 계정명, 이메일, 이름이 있다고 해보자.
여기서 회원 정보를 늘리기 위해 전화번호를 추가할 경우 기존 데이터들과 다르기에 기존 데이터를 모두 
수정해야 새 데이터를 등록할 수 있다.
그래서 데이터 양이 많을 때에 데이터베이스의 스키마를 변경하는 작업이 매우 번거로워 질 수 있다.

2번 확장성은 RDBMS는 저장하고 처리해야할 데이터양이 늘어나면 여러 컴퓨터에 분산시키는 것이 아닌,
해당 데이터베이스 서버의 성능을 업그레이드하는 방식으로 확장해야한다.

이러한 한계를 극복한 것이 오늘 공부할 MongoDB "문서 지향적 NoSQL 데이터베이스"이다.
이 데이터베이스에 등록하는 데이터들은 유동적인 스키마를 지닐 수 있다.
종류가 같은 데이터라고 하더라도 새로 등록해야할 데이터 형식이 바뀐다고 하더라도 기존 데이터까지 수정할 필요는 없다. 
서버의 데이터양이 늘어나도 한 컴퓨터에서 처리한는 것이 아닌, 여러 컴퓨터로 분산처리가 가능하다.

그렇지만 무조건 MongoDB가 좋다고 하지는 않는다.
모든 기술은 상황에 맞게 사용해야한다.
예를 들어 데이터의 구조가 자주 바뀌게 된다면 MongoDB가 적합하지만,
까다로운 조건으로 데이터를 필터링해야하거나, ACID 특성을 지켜야 한다면 RDBMS가 더 유리할 것이다.
```
ACID 설명
-A(Atomicity:원자성)C(Consistenct:일관성)I(Isolation:고립성)D(Durability:지속성)의 앞글자를 
만든 용어로, 데이터베이스
트랜잭션이 안전하게 처리하는 것을 보장하기 위한 성질을 의미한다.
```

--- 

여기서 문서지향적 NoSQL DB라고 하는데 문서는 무엇인가?
문서의 데이터 구조는 한 개 이상의 키-값쌍으로 되어 있다.
```
예시)
{
    "_id":ObjectId("509983df3f4948db2f98391"),
    "username":"MurPan",
    "name":{ first: "M.P", last:"kim"}
}
```
문서는 BSON(바이너리 형태의 JSON)형태로 저장된다. 그렇기에 나중에 JSON형태의 객체를 데이터베이스에 저장할 때 편하게 데이터를 데이터베이스에 저장할 수있다.
새로운 문서를 생성하면 _id라는 고유값을 자동으로 생성하며, 이 값은 시간, 머신아이디
프로세스 아이디, 순차 번호로 되어 있어 값의 고유함을 보장한다.
그리고 이러한 문서들이 여러개 들어 있는 곳을 컬렉션이라고 한다.
기존 RDBMS에서는 테이블 개념을 사용하므로 각 테이블마다 같은 스키마를 가지고 있어야 한다.
반면 MongoDB는 다른 스키마를 가지고 있는 문서들이 한 컬렉션에서 공존할 수 있다.
```
{
    "_id":ObjectId("509983df3f4948db2f98391"),
    "username":"MurPan",

}{
    "_id":ObjectId("509872ef3f4948db2f97245"),
    "username":"MurPan",
    "phone":010-1234-5678
}
```
MongoDB에서는 컬렉션 안의 데이터가 같은 스키마를 가질 필요가 없기에 그냥 넣어주면 됩니다.
========================================================================================
MongoDB 구조
서버 하나에 데이터베이스를 여러개를 가지고 있을 수 있다.
각 데이터베이스에는 여러개의 컬렉션이 있으며 커렉션 내부에는 문서들이 들어있다.

스키마 디자인
MongoDB에서 스키마를 디자인하는 방식은 기존 RDBMS에서 스키마를 디자인하는 방식과는 완전히 다르다
예를 들어 RDBMS에서 블로그용 데이터 스키마를 설계한다면 각 포스트, 댓글마다 테이블을 만들어
필요에 따라 JOIN해서 사용하는 것이 일반적이다

하지만 NoSQL에서는 그냥 모든 것을 문서 하나에 넣는 것으로 해결이 가능하다.
```
{
    _id:ObjectId,
    title:String,
    body:String,
    username:String,
    createdDate:Date,
    comments:{
        {
            _id: ObjectId,
            text:String,
            createdDate:Date
        }
    }
}
```
이런 상황에서 보통 MongoDB는 댓글을 포스트 문서 내부에 넣는다 문서 내부에 또 다른 문서가 위치할 수 있는데 , 이를 subDocument라고 한다. 일반 문서처럼 쿼리가 가능하다
문서 하나에는 16MB만큼 데이터를 넣을 수 있고, 이를 초과할 가능성이 있다면 컬렉션을 분리시키는 것이
좋다.
TIL, 11/10