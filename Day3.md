# 203. Remove Linked List Elements
```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode removeElements(ListNode head, int val) {
        // Step 1: Create a sentinel (dummy) node.
        // The sentinel points to the head of the original list.
        // Why do we need this? 
        // -> It simplifies edge cases, especially when the first node(s) contain the target value,
        //    since we always have a node before `head` to manipulate.
        ListNode sentinel = new ListNode(0);
        sentinel.next = head;

        // Step 2: Use two pointers:
        // `prev` will trail behind `current`.
        // `current` traverses the list to check each node.
        ListNode current = head;
        ListNode prev = sentinel;

        // Step 3: Traverse the linked list.
        // The goal is to skip nodes that match the value `val`.
        while (current != null) {
            if (current.val == val) {
                // If current node should be removed:
                // Link the previous node directly to the node after `current`.
                // Effectively "skips" current.
                prev.next = current.next;
            } else {
                // If current node is kept:
                // Move `prev` forward to current (since it's safe).
                prev = prev.next;
            }
            // Always move `current` forward to check the next node.
            current = current.next;
        }

        // Step 4: Return the new head of the list.
        // sentinel.next may have changed if the original head was removed.
        return sentinel.next;
    }
}
```
# 707. Design Linked List
``` java
class MyLinkedList {
    Node node;
    int size;

    private class Node {
        int val;
        Node next;

        private Node() {
        }

        private Node(int val) {
            this.val = val;
        }
    }

    public MyLinkedList() {
        this.size = 0;
    }

    public int get(int index) {
        // If index is invalid
        if (index < 0 || index >= size)
            return -1;

        Node curr = this.node;
        for (int i = 0; i < index; i++) {
            curr = curr.next;
        }
        return curr.val;
    }

    public void addAtHead(int val) {
        Node prev = this.node;
        this.node = new Node(val);
        this.node.next = prev;
        this.size++;
    }

    public void addAtTail(int val) {
        if (size == 0) {
            this.node = new Node(val);
        } else {
            Node curr = this.node;
            while (curr.next != null) {
                curr = curr.next;
            }
            curr.next = new Node(val);
        }
        size++;
    }

    public void addAtIndex(int index, int val) {
        if (index < 0 || index > size)
            return;
        if (index == 0) {
            addAtHead(val);
            return;
        }
        Node dummy = new Node(0);
        dummy.next = this.node;
        Node curr = dummy;
        for (int i = 0; i < index; i++) {
            curr = curr.next;
        }
        Node newNode = new Node(val);
        newNode.next = curr.next;
        curr.next = newNode;
        this.node = dummy.next;
        size++;
    }

    public void deleteAtIndex(int index) {
        if (index >= size) {
            return;
        }
        Node sentinel = new Node(0);

        Node prev = sentinel;
        Node curr = this.node;

        prev.next = curr;
        for (int i = 0; i < index; i++) {
            curr = curr.next;
            prev = prev.next;
        }
        prev.next = curr.next;

        this.node = sentinel.next;
        this.size--;
    }
}
```

# 206. Reverse Linked List
``` java
/**
 * Your MyLinkedList object will be instantiated and called as such:
 * MyLinkedList obj = new MyLinkedList();
 * int param_1 = obj.get(index);
 * obj.addAtHead(val);
 * obj.addAtTail(val);
 * obj.addAtIndex(index,val);
 * obj.deleteAtIndex(index);
 */

/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { 
 *         this.val = val; 
 *         this.next = next; 
 *     }
 * }
 */

class Solution {
    public ListNode reverseList(ListNode head) {
        // ✅ Base case: if the list is empty, there’s nothing to reverse
        if (head == null) {
            return null;
        }

        // Create a dummy node that will act as the head of our new reversed list
        // We'll keep inserting nodes in front of dummy.next to reverse order
        ListNode dummy = new ListNode(0);

        // Initialize the reversed list with the first node's value
        // (We create a new node because we’re not modifying the original list)
        dummy.next = new ListNode(head.val);

        // Traverse the remaining nodes in the original list
        while (head.next != null) {
            // Move head to the next node
            head = head.next;

            // Keep a reference to the current head of the reversed list
            ListNode prevNext = dummy.next;

            // Create a new node with the current value
            // Insert it at the beginning of the reversed list
            dummy.next = new ListNode(head.val);

            // Connect the new node to the previously reversed portion
            dummy.next.next = prevNext;
        }

        // Return the head of the reversed list (which is dummy.next)
        return dummy.next;
    }
}
```
