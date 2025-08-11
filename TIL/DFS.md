# Java에서 DFS(깊이 우선 탐색) 정리 및 Stack 자료구조 선택 이유

알고리즘 문제를 Java로 풀 때, DFS(Depth-First Search, 깊이 우선 탐색)는 그래프나 트리 탐색에서 매우 자주 사용되는 기법입니다.  
이 문서에서는 Java에서 DFS를 구현하는 방법과, DFS에 적합한 자료구조(특히 Stack으로 ArrayDeque/LinkedList를 사용하는 이유), 그리고 주의할 점을 종합적으로 정리합니다.

---

## 1. DFS란?

- 그래프/트리에서 한 방향으로 계속 깊게 들어가며 탐색하는 방식
- 모든 노드를 방문해야 하는 경우, 경로/집합 탐색, 연결 요소 찾기 등에 자주 활용됨
- **LIFO(Last-In-First-Out)** 원칙에 따라, 가장 최근에 방문한 노드부터 처리 (스택 구조)

---

## 2. DFS 구현 방법

### 2-1. 재귀(Recursive) 방식

- 함수가 자기 자신을 반복적으로 호출하여 깊게 탐색
- Java의 함수 호출 스택을 활용

```java
void dfs(int v) {
    visited[v] = true;
    for (int u : adj[v]) {
        if (!visited[u]) {
            dfs(u);
        }
    }
}
```
- **장점**: 코드가 간결, 구현이 쉬움
- **단점**: 탐색 깊이가 깊으면 StackOverflow 발생 가능

---

### 2-2. 명시적 스택(Stack) 방식

- 직접 Stack 자료구조를 선언하고 반복문을 통해 탐색
- 깊이 제한이 없고, StackOverflow 걱정 없음

```java
Deque<Integer> stack = new ArrayDeque<>();
stack.push(start);
while (!stack.isEmpty()) {
    int v = stack.pop();
    if (!visited[v]) {
        visited[v] = true;
        for (int u : adj[v]) {
            if (!visited[u]) {
                stack.push(u);
            }
        }
    }
}
```

---

## 3. DFS에 적합한 Stack 자료구조와 그 이유

DFS는 **후입선출(LIFO)** 구조이므로 Stack이 핵심 자료구조입니다.  
Java에서는 다음과 같이 구현할 수 있습니다:

### ArrayList vs. ArrayDeque/LinkedList vs. Stack 클래스

#### ArrayList
- **index로 원소 관리**
- 맨 뒤 추가/삭제는 빠름, 맨 앞은 느림
- Stack 연산(push/pop)을 맨 뒤에서만 사용하면 가능하지만,  
  일반적으로 Stack의 표준 연산은 맨 앞 기준이므로 적합하지 않음

#### ArrayDeque
- **배열 기반의 덱(Deque) 자료구조**
- 양쪽 끝에서 원소 추가/제거 모두 O(1)
- `push`, `pop` 연산이 매우 빠름
- Java 공식 문서에서 추천
- index 기반 접근 불가

```java
Deque<Integer> stack = new ArrayDeque<>();
```

#### LinkedList
- **양방향 연결 리스트 (Deque 인터페이스 구현)**
- 양쪽 끝에서 원소 추가/제거 O(1)
- Stack처럼 사용 가능 (다만 ArrayDeque가 더 빠름)

```java
Deque<Integer> stack = new LinkedList<>();
```

#### Stack 클래스 (`java.util.Stack`)
- 내부적으로 Vector로 구현된 레거시 클래스
- 동기화 지원(멀티스레드 안전)이지만 단일 스레드에서는 성능 저하
- 새로운 코드에서는 ArrayDeque/LinkedList 사용 권장

---

## 4. 구조적 차이와 Stack 선택 이유

| 자료구조     | 특징                                               |
|--------------|----------------------------------------------------|
| ArrayList    | index로 관리, 맨 뒤 추가/삭제는 빠름, 앞쪽은 느림  |
| ArrayDeque   | 맨 앞/뒤 추가/삭제 모두 빠름, index 접근 불가      |
| LinkedList   | 맨 앞/뒤 추가/삭제 모두 빠름, index 접근 느림      |
| Stack        | 레거시, 동기화로 느림                              |

- DFS에서는 **맨 앞/뒤 원소 추가/삭제 연산이 빈번**하므로  
  ArrayDeque 또는 LinkedList가 훨씬 효율적입니다.

---

## 5. DFS 사용 시 주의점

- **StackOverflow**: 재귀 방식일 때 탐색 깊이가 깊으면 발생 → 명시적 스택 방식으로 대체
- **방문 체크**: 무한 루프 방지 또는 중복 방문 방지 위해 반드시 visited 배열/집합 사용

---

## 6. 결론

- DFS는 깊이 우선 탐색이며, LIFO 구조의 Stack을 사용
- Java에서는 구현 방식에 따라 재귀 또는 명시적 스택을 사용할 수 있음
- Stack 구현은 ArrayDeque 또는 LinkedList를 사용하는 것이 가장 효율적
- 방문 체크와 스택 오버플로우에 주의하며 구현할 것

---
