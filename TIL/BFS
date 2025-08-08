# Java에서 BFS(너비 우선 탐색)에 Queue로 LinkedList를 사용하는 이유

알고리즘 문제를 Java로 풀 때, BFS(너비 우선 탐색) 구현에 `Queue`를 사용하는 경우가 많습니다.  
그런데 왜 `Queue`를 선언할 때 `LinkedList`를 주로 사용할까요?  
구조적으로 어떤 차이가 있어 `LinkedList`를 사용하는지 알아보고자 합니다.

## ArrayList와 LinkedList의 구조적 차이

### ArrayList
- **중복 가능**
- **순서 유지**
- **index로 원소 관리**
- **크기 고정되지 않음** (내부적으로 초과시 일정 수만큼 공간 증가)
- **주요 동작**
  - `add(E element)`: 제일 뒤에 원소 추가 (`index`로 관리하므로 빠름)
  - `add(int index, E element)`: 원하는 index에 원소 추가 (index 뒤 원소 모두 이동 필요 → 느림)
  - `remove(int index)`: 원하는 index 제거 (앞으로 원소 이동 필요 → 느림)
  - `get(int index)`: index 위치 원소 조회 (빠름)

### LinkedList
- **양방향 연결 리스트** (순/역방향 접근 가능)
- **크기 고정**
- **중간에 추가/삭제 시 데이터 이동 필요**
- **주요 동작**
  - `add(E element)`: 제일 뒤에 원소 추가 (Head부터 찾아가야 함 → 느림)
  - `add(int index, E element)`: 원하는 index에 추가 (Head부터 index까지 찾아감 → 느림)
  - `remove(int index)`: index의 원소 제거 (앞/뒤 노드 연결만 바꾸면 됨 → 효율적)
  - `get(int index)`: index 위치 원소 조회 (Head부터 index까지 찾아야 함)

## 왜 BFS Queue에는 LinkedList가 적합할까?

Queue의 본질은 **선입선출(FIFO)** 구조입니다.

- **ArrayList로 Queue를 구현하면?**
  - 맨 앞의 원소를 꺼내고 나면, 그 자리를 채우기 위해 모든 원소를 한 칸씩 앞으로 옮겨야 합니다.
  - 원소가 많아질수록 **시간이 많이 걸림** → 비효율적!

- **LinkedList로 Queue를 구현하면?**
  - 삭제하고 싶은 원소를 **null로 변경**하고 앞/뒤 노드 연결만 바꿔주면 됩니다.
  - **빈 공간을 채울 필요가 없음** → 매우 효율적!

### 결론

BFS에서 자주 발생하는 **맨 앞 원소 꺼내기/삭제** 연산에서는  
ArrayList보다 LinkedList가 훨씬 효율적이므로  
Java에서 BFS Queue 구현에는 **LinkedList**를 사용하는 것이 일반적
