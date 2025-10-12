# 24. Swap Nodes in Pairs
``` java
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
    public ListNode swapPairs(ListNode head) {
        // ✅ Create a sentinel (dummy) node that points to the head.
        // This helps simplify edge cases (like swapping the first pair)
        // because we always have a 'prev' node to attach the swapped pair to.
        ListNode sentinel = new ListNode(0);
        sentinel.next = head;

        // 'prev' always points to the node **before** the pair being swapped.
        ListNode prev = sentinel;

        // Continue while there are at least two nodes left to swap
        while (head != null && head.next != null) {
            // Identify the two nodes to be swapped
            ListNode firstNode = head;          // first in the pair
            ListNode secondNode = head.next;    // second in the pair

            // ⚙️ Swapping step 1: 
            // Connect the first node to the node after the second
            // (i.e., skip over secondNode temporarily)
            firstNode.next = secondNode.next;

            // ⚙️ Swapping step 2:
            // Make the second node point to the first one
            // (this reverses the pair)
            secondNode.next = firstNode;

            // ⚙️ Swapping step 3:
            // Connect the previous node to the second node
            // (this "plugs in" the swapped pair into the main list)
            prev.next = secondNode;

            // 🧭 Move pointers forward:
            // Now 'prev' moves to the end of the swapped pair (firstNode)
            prev = firstNode;

            // Move 'head' to the next pair’s first node
            head = firstNode.next;
        }

        // ✅ Return the new head, which is sentinel.next
        return sentinel.next;
    }
}

```

# 19. Remove Nth Node From End of List
``` java
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
    public ListNode removeNthFromEnd(ListNode head, int n) {
        // --- Step 1: Create a dummy node before the head ---
        // I create a dummy node to handle edge cases gracefully, such as when the node
        // to be removed is the head itself. By doing this, I can avoid having to write 
        // special-case logic for when 'head' needs to be deleted.
        ListNode dummy = new ListNode(0);
        dummy.next = head;

        // --- Step 2: Initialize two pointers: fast and slow ---
        // Both pointers start at the dummy node. The fast pointer will be advanced first
        // to create a gap of 'n' nodes between fast and slow.
        ListNode fast = dummy;
        ListNode slow = dummy;

        // --- Step 3: Move fast pointer ahead by n+1 steps ---
        // The "+1" ensures that when fast reaches the end, slow will be right before
        // the node that needs to be deleted.
        for (int i = 0; i < n + 1; i++) {
            fast = fast.next;
        }

        // --- Step 4: Move both pointers until fast reaches the end ---
        // Now, move both fast and slow together, one step at a time.
        // When fast reaches the end (null), slow will be positioned right before
        // the node we want to remove.
        while (fast != null) {
            fast = fast.next;
            slow = slow.next;
        }

        // --- Step 5: Remove the target node ---
        // The node to remove is 'slow.next', so we simply skip it by linking
        // 'slow.next' to 'slow.next.next'.
        slow.next = slow.next.next;

        // --- Step 6: Return the new head ---
        // Since the head might have been removed, we return 'dummy.next',
        // which correctly represents the possibly updated head of the list.
        return dummy.next;
    }
}
```

# 160. Intersection of Two Linked Lists
``` java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */

public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        // The goal is to find the node where two singly linked lists intersect.
        // If they do not intersect, return null.
        //
        // We'll use a HashSet to store the nodes visited in list A.
        // Then, we’ll traverse list B and check whether any node in B
        // has already been seen in A — that node would be the intersection point.
        
        // Pointer to traverse list A
        ListNode pointerA = headA;
        
        // A HashSet is used to store references (not just values) of nodes from list A
        // Because intersection depends on reference equality, not value equality.
        HashSet<ListNode> seen = new HashSet<>();

        // Step 1: Traverse through list A and store each node reference in the set
        while (pointerA != null) {
            seen.add(pointerA);      // Store the current node’s memory reference
            pointerA = pointerA.next; // Move to the next node
        }

        // Step 2: Traverse through list B and check if any node has been seen before
        ListNode pointerB = headB;
        while (pointerB != null) {
            // If the current node in list B was already seen in list A,
            // that means it’s the intersection node.
            if (seen.contains(pointerB)) {
                return pointerB; // Return the intersection node immediately
            }
            pointerB = pointerB.next; // Otherwise, continue traversing
        }

        // Step 3: If no common node is found, return null
        return null;
    }
}
```

# 142. Linked List Cycle II
``` java
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */

public class Solution {
    /**
     * This method detects if a linked list has a cycle, and if it does,
     * returns the node where the cycle begins.
     * If there is no cycle, it returns null.
     *
     * Approach:
     * - We use a HashSet to record all nodes we have visited so far.
     * - If we ever visit a node that is already in the set,
     *   that means we’ve entered a cycle, and this node is the starting point of that cycle.
     * - Otherwise, if we reach the end (head == null), it means there’s no cycle.
     *
     * Time complexity: O(n), because we visit each node at most once.
     * Space complexity: O(n), since we may store all nodes in the HashSet in the worst case.
     */
    public ListNode detectCycle(ListNode head) {
        // Use a HashSet to keep track of visited nodes.
        HashSet<ListNode> seen = new HashSet<>();
        
        // Traverse the linked list one node at a time
        while (head != null) {
            // If we've already seen this node, then a cycle exists.
            // The current node is the start of the cycle.
            if (seen.contains(head)) {
                return head;
            }

            // Otherwise, record this node as visited
            seen.add(head);

            // Move to the next node
            head = head.next;
        }

        // If we reach the end (head == null), then there is no cycle
        return null;
    }
}
```
