## 개요

- 해시는 **키 key, 그리고 그와 연관된associated 값 value**을 가지고 있다.   
- 이는 **연상 배열Associative Arrays**을 바탕으로 만들어진 자료 구조이다.   
- 해시에서는 키가 주어지면 바로 그와 연관된 값을 찾을 수 있다.   
↔연결 리스트에서는 모든 요소를 살펴보며 특정 노드를 찾는다. O(n)의 시간 복잡도를 갖는다.

### 해시의 메소드들과 시간 복잡도

- add, remove, lookup | find
- all entries, all keys, all values - 최소 Θ(n)의 시간 복잡도를 갖는다.
- size, isEmpty, isFull, loadFactor - O(1), 상수 시간으로 구현이 가능하다.

### **연상 배열Associative Arrays**

<img width=400 src='https://user-images.githubusercontent.com/62924471/214090929-ce66b39d-c361-4a49-923c-ee8dd2c71067.png'>

좌측 이미지에서 id는 key이고, grade는 value이다.

```java
hashCode(String id) {
		remove cssc;
		convert to int;
		int -10;
		return int idex;
} // 슈도 코드
```

위와 같은 hashCode() 메소드를 이용해, id 값을 배열의 index 값으로 변환한다.   
그리고 변환된 index로 배열의 값을 찾거나 삭제할 때 활용할 수 있다. 

## 해시 함수Hash Function

특정 오브젝트를 파라미터로 받아서, 정수int를 반환한다. 이 정수는 객체를 저장할 인덱스 값으로 활용할 수 있다.

- property of the data, 데이터의 속성에 따른다.
    - 예를 들어, CSSC 아이디가 있다면 CSSC 부분을 제거해야 하는 기능을 가진다.
- be fast to compute, 연산이 빨라야 한다.
- if two things are "equal", should return the same value, 두 요소가 "같다면(개발자가 정의)" 같은 값을 반환해야 한다.
- Always return the same value for an object during one run of the code, 같은 실행 환경일 경우 같은 객체라면 같은 값이 나와야 한다.
- possible to return different values for an object in seperate runs, 코드를 새로 실행하면 객체가 같더라도 다른 값이 나올 수 있다.
    - 그러나 구현 시, 위와 같은 특징은 피할 수 있다면 피하는 것이 좋다. 
    - 예를 들어, 디스크에 데이터를 저장한 경우, 이전과 같은 해시 값이 나와야 저장한 위치를 정확히 알 수 있기 때문이다.
    - 위와 같은 이유로 새로운 클래스를 만들 때, Object의 toString, equals, hashCode와 같은 메소드들은 오버라이딩을 권장한다.
- minimize collisions, 해시 충돌을 최소화 해야 한다.

## 해시 충돌Hash Collisions

**서로 다른 값을 가진 키가 일치**하는 경우를 **해시 충돌**이라고 한다.

<img width=400 src='https://user-images.githubusercontent.com/62924471/214091037-ef7a219c-c81e-437f-9382-da6f324007c8.png'>

좌측 이미지에서는 전화번호를 3분할(folding)하여 더한 값을 키로 지정하였다.  
그런데 결과로 산출된 키가 2386으로 같아 **해시 충돌이 발생**한다.

### 문자열과 해시 함수

문자열을 정수 키로 변환하는 경우, 해시 함수를 활용한다.

- 문자열을 유니코드 값으로 변환한 뒤, 각 문자의 자릿수에 따라 특정 상수의 제곱값을 곱해 만들 수 있다.
- 계산 복잡도는 문자열의 길이에 따라 달라지므로, 아주 긴 문자열의 경우에는 subString()을 활용하여 작은 문자열로 계산할 수 있다.

```java
public int hashCode(String s) {
    int g = 31; //임의의 값
    int hash = 0;

    for (int i = 0, len = s.length(); i < len; i++) {
        hash = g * hash + s.charAt(i);
    }
    return hash;
}
```

참고) 유니코드 혹은 ASCII 코드

- 근래에는 유니코드로 변환해서 사용한다.
- 유니코드에서
    - 0-9 → 48-57
    - A-Z → 65-90
    - a-z → 97-122

### 해시 크기 최적화

**해시 충돌**을 **방지**하기 위해 **해시 테이블의 크기를 최적화**합니다.

1. 해시 테이블의 크기를 **홀수**로 설정하여, ****나머지 연산(%)의 결과로 다양한 숫자가 나오게 할 수 있다.
2. 해시 테이블의 크기를 **소수**로 설정하여, 나머지 연산의 결과로 다양한 숫자가 나오게 할 수 있다.

### 2의 보수

해시 함수의 결과로 음수가 나올 수도 있다. 이런 경우에는 2의 보수를 이용해 음수를 양수로 변환한다.

```java
// data의 index 결정
int hashval = data.hashCode(s);
hashval = hashval & ox7FFFFFFF; // 음수라면 양수로 변환된다.
// 예를 들면, -1 은 2147483647로
// -10은 2147483638 로 변환될 것이다.

hashval = hashval % tablesize // 해시 테이블 크기에 적합하도록 모듈 연산을 수행한다.
```

### 테이블 적재율 확인

**적재율**은 **λ**로 표기한다. 이는 테이블(배열) 내 요소 수를 테이블 전체 크기로 나눈 값이다.  
해시 충돌이 일어나지 않도록, **λ의 크기에 따라 테이블의 크기를 조절**한다.

현재 해시 테이블에 데이터가 얼마만큼 있는지 알려주는 **LoadFactor 메소드**를 구현해 활용한다.

# 해시 충돌 해결 - 개방 주소법

### 1. 선형 조사법(linear probing)

- 인덱스가 가리키는 공간이 이미 차 있다면, 비어있는 칸을 찾을 때까지 **다음 칸**을 확인하고 요소를 삽입한다.
- 특정 값을 조회할 때에도, 찾는 값이 나올 때까지(혹은 빈 칸에 도달할 때까지) 주어진 해시 주소 값의 다음 주소를 순차적으로 확인한다.
- 요소를 삭제하는 경우에는 선형 조사가 지속적으로 가능하게 하기 위해서, 공간의 값을 단순히 삭제하는 것이 아니라, flag를 이용해 요소가 있었으나, 삭제되었음을 알린다.
- 데이터가 뭉치게 되고, 순차적으로 저장이 된다는 단점이 발생할 수 있다.

### 2. 2차식 조사법(quadratic probing)

- 데이터가 지역적으로 뭉치는 것을 방지하기 위해, 인덱스에 1을 더하는 대신, **1 부터 순서대로 제곱한 값**(1^2,  2^2, ...)**을 순차적으로 더한 인덱스**를 활용해 요소를 삽입, 확인하는 방법이다.
- 테이블의 크기를 넘어간다면, % 연산을 해서 다시 테이블의 범위 안에 들어오게 조정한다. (이는 선형 조사법도 마찬가지다.)

### 3. 이중 해싱(double hashing)

- **두 가지 해시 함수를 사용**해 요소를 더 골고루 저장할 수 있다. **두번째 해시 함수는 반드시 첫번째와는 달라야 하고, 0를 반환해서는 안된다.**
- 채우려는 공간이 이미 차 있다면, **두 해시 함수의 결과를 더한 값**을 테이블 공간의 위치 인덱스로 이용한다. (이와 같이 합을 이용하기에 두번째 해시 함수 값은 0이 될 수 없는 것이다.)
- 해시 함수가 2개 필요한 것이 단점이다. 왜냐하면, 자바에서는 모든 자료 형태에 두번째 해시 함수가 있다는 것을 보장하지 못 한다. ( 기본 해시 함수는 모든 자료 타입에서 존재한다. 다시 말하면, 모든 객체가 hashCode()메소드를 갖는다.)

선형 조사법과 2차식 조사법을 이용하는 경우에 적재율이 0.6 이상이 된다면, 테이블의 크기를 키워야 한다.

## Chaining

체이닝은 요소마다 연결 리스트를 만들어, 수많은 데이터를 수용할 수 있게 하는 방법이다. 해시 충돌을 피하기 위해 체이닝을 사용한다. 

**체인 해시**는 ****해시와 연결 리스트를 합친 자료 구조를 말하며, **가장 안정적이고 보편적으로 사용되는 자료 구조** 중 하나이다. (교수님 피셜, 최고의 자료구조)

- 파이썬의 딕셔너리와 펄의 해시가 체인 해시 구조를 갖는다.
<img width=400 src='https://user-images.githubusercontent.com/62924471/214091114-4855481c-924e-494d-a196-02ed02bfdf0a.png'>

- 요소를 추가하고, 찾고, 삭제할 때, 상수 시간이 소요된다.
- 체이닝을 하면 수용 가능한 요소 개수에 제한이 없어지고 크기 조정도 자주 할 필요가 없다.
- 체인 해시에서 **적재율 λ는 요소의 개수를 가능한 체인의 수(연결 리스트의 갯수 혹은 배열의 크기)로 나눈 값**이다. 
  - 체인 1개에 여러 항목을 넣을 수 있어 λ는 1보다 큰 수가 될 수 있다.
- 만약 해시 함수가 같은 숫자만 반환하여 하나의 체인이 너무 길어지면 결국 자료구조가 연결 리스트와 같아지는 문제가 발생한다. 
  - 이와 같은 최악의 경우, 요소를 찾고 제거할 때의 시간 복잡도가 상수 시간이 아닌 O(n)이 된다.
- 체인 해시 자료구조에서 키와 값을 알려주는 메소드는 항상 O(n)의 시간 복잡도를 갖는다.

## 재해싱

체인 해시에서 해시가 너무 많이 차면 **크기 조정**이 필요하다.

- 기존의 2배 크기로 테이블을 생성하고, 변경된 테이블 사이즈를 적용해 **재해싱**하여 다시 인덱스를 결정해서 요소들을 삽입한다.
- 만약, 연결 리스트를 기존의 인덱스를 유지해 새로운 테이블로 옮기면, 해당 요소를 찾거나 제거하려 할 때 문제가 발생할 수 있다. 인덱스를 지정할 때, 테이블 크기로 모듈 연산을 수행하기 때문이다. 테이블 크기가 변경되면, 경우에 따라 인덱스 값이 달라진다.

### 최대 적재율

- 일반적으로 해시 테이블이 제네릭 타입(혹은 어디에 사용될지 모르는 상황)인 경우, 0.75가 적절한 최대 적재율이라고 한다.
    - 최대 적재율을 줄이면, 해시 테이블에서 연결 리스트가 더 골고루 분포할 수 있다. 하지만, 그만큼 크기 조정을 더 자주 하게 되어서 시간이 많이 든다.
    - 반대로 최대 적재율을 늘리면, 크기 조정 작업은 비교적 덜 하게 되지만, 그만큼 연결 리스트의 분포가 고르지 못하게 된다는 단점이 있다.

## 해시 클래스

- 제네릭 키와 값을 갖는  내부 클래스인 HashElement를 생성한다.