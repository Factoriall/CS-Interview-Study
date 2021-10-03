# Database

- [배열 vs 연결리스트](#배열-vs-연결리스트)
- [스택 vs 큐](#스택-vs-큐)
- [맵 vs 해쉬맵](#맵-vs-해쉬맵)

## 배열 vs 연결리스트
### 배열

![array](../image/datastructure_array.png)

- 같은 자료형을 가진 변수를 하나로 나타낸 것
- 연속된 메모리 공간으로 이뤄져 있는 자료구조
- Index를 이용해 표현, 지역성을 가짐
- 정적 표현
- 장점
  + 인덱스를 통한 검색 용이
  + 메모리 관리가 편함
- 단점
  + 한 데이터를 삭제해도 배열은 연속해야 하므로 공간이 남음 -> 메모리 낭비
  + 배열 크기를 컴파일 이전이 정해줘야 함
  + 컴파일 이후 배열 크기 조절 불가

### 연결리스트

![linkedlist](../image/datastructure_linkedlist.png)

- 순서가 있는 데이터 집합
- 불연속적으로 메모리 공간 차지
- Index를 가지지 않으며 순차성을 가지지 않음
- 동적 표현, 빈 Element는 허용하지 않음
- 장점
  + 삽입 및 삭제가 용이
  + 크기가 정해져있지 않아 메모리 재사용에 편리
- 단점
  + 검색 성능이 안 좋음
  + 포인터 통해 다음 데이터를 가리키므로 추가적인 메모리 공간 필요
- 코드 구현

~~~java
import java.io.*

public class LinkedList{  
  static class Node{
    int data;
    Node next;

    Node(int d){
      data = d;
      next = null;
    }
  }

  Node head;
  Node tail;

  public void insert(int data){
    Node newNode = new Node(data);
    if(head == null){
      head = newNode;
      tail = newNode;
    }
    else{
      tail.next = newNode;
      tail = newNode;
    }
  }

  public void remove(int idx){
    Node prev = null;
    Node del = head;
    while(idx > 0){
      prev = del;
      del = del.next;
      idx--;
    }
    if(now == head){
      head = head.next;
    }
    else if(now == tail){
      tail = prev;
    }
    if(prev != null) prev.next = del.next;
    del = null;
  }
}
~~~

## 스택 vs 큐
### 스택
- 정해진 방향으로만 쌓을 수 있는 LIFO 구조
- push / pop / top
- 웹 브라우저 뒤로가기, 역순 문자열, 실행 취소 등에 사용
- 코드
~~~java
~~~

### 큐
- 삽입은 앞쪽, 삭제는 뒤쪽에서 이뤄지는 FIFO 구조
- offer / poll / element
- 시간 순서대로 처리해야 할때 사용
- 우선순위, 은행 업무 등등...
~~~java
~~~

## 맵 vs 해쉬맵
