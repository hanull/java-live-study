### 과제 2. LinkedList를 구현하세요.

> LinkedList에 대해 공부하세요.
>
> 정수를 저장하는 ListNode 클래스를 구현하세요.
>
> ListNode add(ListNode head, ListNode nodeToAdd, int position)를 구현하세요.
>
> ListNode remove(ListNode head, int positionToRemove)를 구현하세요.
>
> boolean contains(ListNode head, ListNode nodeTocheck)를 구현하세요.



## 연결 리스트(Linked List)

`연결 리스트란` 각 노드가 데이터와 다음 노드에 대한 포인터를 가지고 한 줄로 연결되어 있는 자료구조이다.

## 왜 배열이 아닌 연결 리스트를 사용할까??

배열은 비슷한 유형의 선형 데이터를 저장하는데 사용할 수 있지만 제한 사항이 있다.

1. 배열의 크기가 고정되어 있어 미리 요소의 크기에 대해 할당 받아야한다.
2. 새로운 요소를 삽입하는 것은 비용이 많이 든다. (공간을 만들고, 기존 요소들을 재배치 해야한다.)

## 연결 리스트의 장점

- 메모리를 미리 할당하지 않아도 된다. 즉, 크기를 늘리고 줄이는 부가적인 처리를 하지 않아도 된다.
- 데이터가 메모리상의 연속된 위치에 저장되지 않아도 된다.
- 노드의 삽입, 삭제가 용이하다. `O(1)`

## 연결 리스트의 단점

-  임의 접근(Random Access)가 불가능하여 데이터를 검색하는데 `O(n)`이란 시간이 걸리게 된다.
- 중간 노드 연결이 끊어지면 그 다음 노드를 찾기 힘들다.


## LinkedList 구현하기

### 1. LinkedList

```java
public interface LinkedList {
    ListNode add(ListNode head, ListNode nodeToAdd, int position);
    ListNode remove(ListNode head, int positionToRemove);
    boolean contains(ListNode head, ListNode nodeTocheck);
}
```

### 2. ListNode



### 3. 메소드 구현

```java
package linkedlist;

public class LinkedListImplement implements LinkedList {

    @Override
    public ListNode add(ListNode head, ListNode nodeToAdd, int position) {
        if (!isValidateRange(head, position)) return null;
        if (position == 0) {
            nodeToAdd.next = head;
            head.pre = nodeToAdd;
        } else {
            ListNode curNode = head;
            int idx = 0;
            while (idx < position - 1) {
                curNode = curNode.next;
                idx++;
            }
            nodeToAdd.next = curNode.next;
            curNode.next = nodeToAdd;
        }
        return nodeToAdd;
    }

    @Override
    public ListNode remove(ListNode head, int positionToRemove) {
        if (!isValidateRange(head, positionToRemove)) return null;
        ListNode headNode = head;
        if (positionToRemove == 0) {
            head.next.pre = null;
            headNode = head.next;
        } else {
            ListNode curNode = head;
            int idx = 0;
            while (idx < positionToRemove) {
                curNode = curNode.next;
                idx++;
            }
            curNode.pre.next = curNode.next;
            curNode.next.pre = curNode.pre;
        }
        return headNode;
    }

    @Override
    public boolean contains(ListNode head, ListNode nodeTocheck) {
        if (head == null) return false;
        while (head != null) {
            if (head == nodeTocheck) return true;
            head = head.next;
        }
        return false;
    }

    public boolean isValidateRange(ListNode head, int position) {
        int size = getSize(head);
        return position < 0 || position > size ? false : true;
    }

    public int getSize(ListNode head) {
        if (head == null) return 0;
        int res = 0;
        while (head != null) {
            head = head.next;
            res++;
        }
        return res;
    }

}

```

### 4. 테스트 코드

```java
package linkedlist;

import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Nested;
import org.junit.jupiter.api.Test;

import static org.junit.jupiter.api.Assertions.*;

@DisplayName("## LinkedList 테스트 ##")
class LinkedListImplementTest {

    @Nested
    @DisplayName("add 메소드는")
    class AddTest {

        @Nested
        @DisplayName("올바른 position이 주어졌을 때, ")
        class ValidatePosition {

            @Test
            @DisplayName("추가된 노드를 반환한다.")
            void returnAddNode() {
                LinkedListImplement list = new LinkedListImplement();
                ListNode head = new ListNode(1);
                ListNode node2 = new ListNode(2);
                ListNode node3 = new ListNode(3);
                ListNode node4 = new ListNode(4);

                list.add(head, node2, 1);
                list.add(head, node3, 2);
                list.add(head, node4, 3);
                assertEquals(1, head.getData());
                assertEquals(2, head.getNext().getData());
                assertEquals(3, head.getNext().getNext().getData());
                assertEquals(4, head.getNext().getNext().getNext().getData());

                ListNode newNode = new ListNode(100);
                head = list.add(head, newNode, 0);
                assertEquals(100, head.getData());
            }
        }

        @Nested
        @DisplayName("잘못된 position이 주어졌을 때,")
        class InValidatePosition {

            @Test
            @DisplayName("null을 반환한다.")
            void returnNull() {
                LinkedListImplement list = new LinkedListImplement();
                ListNode head = new ListNode(1);
                ListNode node2 = new ListNode(2);

                assertNull(list.add(head, node2, 2));
            }
        }
    }


    @Nested
    @DisplayName("remove 메소드는")
    class RemoveTest {

        @Nested
        @DisplayName("올바른 position이 주어졌을 때,")
        class ValidatePosition {
            @DisplayName("해당 위치의 노드를 삭제 후, head를 반환한다.")
            @Test
            public void returnRemoveNode() {
                LinkedListImplement list = new LinkedListImplement();

                ListNode head = new ListNode(1);
                ListNode node2 = new ListNode(2);
                ListNode node3 = new ListNode(3);

                list.add(head, node2, 1);
                list.add(head, node3, 2);

                head = list.remove(head, 0);
                assertEquals(head, node2);
                head = list.remove(head, 0);
                assertEquals(head, node3);
            }
        }

        @Nested
        @DisplayName("잘못된 position이 주어졌을 때,")
        class InValidatePostion {
            @DisplayName("null을 반환한다.")
            @Test
            public void returnNull() {
                LinkedListImplement list = new LinkedListImplement();

                ListNode head = new ListNode(1);
                ListNode node2 = new ListNode(2);

                head = list.remove(head, 2);
                assertNull(head);
            }
        }
    }

    @Nested
    @DisplayName("contains 메소드는")
    class ContainTest {

        @Nested
        @DisplayName("check 노드가 존재할 때,")
        class hasCheckNode {
            @DisplayName("true를 반환한다.")
            @Test
            public void returnTrue() {
                LinkedListImplement list = new LinkedListImplement();

                ListNode head = new ListNode(1);
                ListNode node2 = new ListNode(2);

                list.add(head, node2, 1);

                assertTrue(list.contains(head, head));
                assertTrue(list.contains(head, node2));
            }
        }

        @Nested
        @DisplayName("check 노드가 존재하지 않을 때,")
        class NoHasCheckNode {
            @DisplayName("false를 반환한다.")
            @Test
            public void returnFalse() {
                LinkedListImplement list = new LinkedListImplement();

                ListNode head = new ListNode(1);
                ListNode node2 = new ListNode(2);

                assertFalse(list.contains(head, node2));
            }
        }
    }
}
```