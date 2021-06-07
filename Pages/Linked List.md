# Linked List

- [Reverse a Linked List](#Reverse-a-Linked-List)
- [Remove Nth Node From End of List](#Reverse-a-Linked-List)
- [Middle of Linked List](#Remove-Nth-Node-From-End-of-List)
- [Merge two sorted Linked List](#Merge-two-sorted-Linked-List)
- [Add two numbers given as Linked List](#Add-two-numbers-given-as-Linked-List)
- [Intersection Point of two Linked List](#Intersection-Point-of-two-Linked-List)
- [Detect a Cycle in Linked List](#Detect-a-Cycle-in-Linked-List)
- [Check if Linked list is palindrome or not](#Check-if-Linked-list-is-palindrome-or-not)
- [Reverse Nodes in k-Group](#Reverse-Nodes-in-k-Group)
- [Starting Point of the Cycle](#Starting-Point-of-the-Cycle)
- [Flattening a Linked List](#Flattening-a-Linked-List)
- [Rotate a Linked List](#Rotate-a-Linked-List)
- [Copy Linked List with Random Pointer](#Copy-Linked-List-with-Random-Pointer)

## [Reverse a Linked List](https://www.geeksforgeeks.org/reverse-a-linked-list/)


```python
def reverse_linked_list(head):
    prev_node = None
    while head != None:
        next_node = head.next
        head.next = prev_node
        prev_node = head
        head = next_node

    return prev_node
```

## [Remove Nth Node From End of List](https://www.geeksforgeeks.org/delete-nth-node-from-the-end-of-the-given-linked-list/)


```python
def remove_Nth_from_end(head, n):
    temp_node = head
    prev_node = None
    fast_node = head

    for _ in range(n):
        print(fast_node.val)
        fast_node = fast_node.next

    while fast_node != None:
        prev_node = temp_node
        temp_node = temp_node.next
        fast_node = fast_node.next

    if prev_node == None:
        head = head.next
        del temp_node
    else:
        prev_node.next = temp_node.next
        del temp_node

    return head
```

## [Middle of Linked List](https://www.geeksforgeeks.org/write-a-c-function-to-print-the-middle-of-the-linked-list/)


```python
def middle_of_linked_list(head):
    temp_node = head
    fast_node = head

    while fast_node != None and fast_node.next != None:
        temp_node = temp_node.next
        fast_node = fast_node.next.next

    return temp_node
```

## [Merge two sorted Linked List](https://www.geeksforgeeks.org/merge-two-sorted-linked-lists/)


```python
# Merge two sorted Linked List
def merge_two_sorted_lists(l1, l2):
    if l1 == None:
        return l2
    if l2 == None:
        return l1

    if l2.val < l1.val:
        l1, l2 = l2, l1

    res_node = l1

    while l1 != None and l2 != None:
        while l1 != None and l1.val <= l2.val:
            temp = l1
            l1 = l1.next
        temp.next = l2

        l1, l2 = l2, l1

    return res_node
```

## [Add two numbers given as Linked List](https://www.geeksforgeeks.org/add-two-numbers-represented-by-linked-lists/)


```python
def add_two_num(l1, l2):
    head = None
    node = None
    carry = 0

    while l1 != None or l2 != None or carry:
        if head == None:
            head = ListNode()
            node = head
        else:
            node.next = ListNode()
            node = node.next

        node_sum = 0
        if l1 != None:
            node_sum += l1.val
            l1 = l1.next

        if l2 != None:
            node_sum += l2.val
            l2 = l2.next

        node_sum += carry

        node.val = node_sum % 10
        carry = node_sum // 10

    return head
```

## [Intersection Point of two Linked List](https://www.geeksforgeeks.org/write-a-function-to-get-the-intersection-point-of-two-linked-lists/)


```python
def get_intersection_node(headA, headB):
    if headA == None or headB == None:
        return None

    head_a = headA
    head_b = headB

    while (head_a != head_b):
        head_a = head_a.next if head_a != None else headB
        head_b = head_b.next if head_b != None else headA

    return head_a
```

## [Detect a Cycle in Linked List](https://www.geeksforgeeks.org/detect-loop-in-a-linked-list/)


```python
def has_cycle(self, head):
    if head == None:
        return False

    slow_node = head
    fast_node = head.next

    while slow_node != fast_node:
        if fast_node == None or fast_node.next == None:
            return False

        slow_node = slow_node.next
        fast_node = fast_node.next.next

    return True
```

## [Check if Linked list is palindrome or not](https://www.geeksforgeeks.org/function-to-check-if-a-singly-linked-list-is-palindrome/)


```python
def reverse(node):
    prev_node = None
    curr_node = node

    while curr_node != None:
        next_node = curr_node.next
        curr_node.next = prev_node
        prev_node = curr_node
        curr_node = next_node

    return prev_node

def middle_node(node):
    slow_node = node
    fast_node = node.next

    while fast_node != None and fast_node.next != None:
        slow_node = slow_node.next
        fast_node = fast_node.next.next

    return slow_node

def isPalindrome(head):
    if head == None:
        return True

    middle_node = middle_node(head)
    reverse_node = reverse(middle_node)

    while head != None and reverse_node != None:
        if head.val != reverse_node.val:
            return False

        head = head.next
        reverse_node = reverse_node.next

    return True
```

## [Reverse Nodes in k-Group](https://www.geeksforgeeks.org/reverse-a-list-in-groups-of-given-size/)


```python
def reverseKGroup(head, k):
    if k == 1 or head == None:
        return head

    # Calculate length
    n = 0
    temp_head = head
    while temp_head != None:
        n += 1
        temp_head = temp_head.next

    n_swaps = n // k    
    
    dummy = ListNode(val = 0, next = head)
    prev = dummy
    cur = head
    pre = dummy
    
    for i in range(n_swaps):
        first_in_interval = cur 

        for j in range(k):
            nxt = cur.next
            cur.next = prev
            prev = cur
            cur = nxt

        first_in_interval.next = cur
        one_before_interval.next = prev

        one_before_interval = first_in_interval
    
    # if n % k != 0 than we need to also reverse the last group
    if n != 0:
        cur = pre.next
        nex = cur.next
        for i in range(1, n):
            cur.next = nex.next
            nex.next = pre.next
            pre.next = nex
            nex = cur.next
        pre = cur
    
    return dummy.next
```

## [Starting Point of the Cycle](https://www.geeksforgeeks.org/find-first-node-of-loop-in-a-linked-list/)


```python
def detectCycle(head):
    if head == None or head.next == None:
        return None

    slow = head
    fast = head

    entry = head

    while fast != None and fast.next != None:
        slow = slow.next
        fast = fast.next.next

        if slow == fast:
            while slow != entry:
                slow = slow.next
                entry = entry.next

            return entry

    return None
```

## [Flattening a Linked List](https://www.geeksforgeeks.org/flattening-a-linked-list/)


```python
def merge(l1, l2):
    l1_temp = l1
    l2_temp = l2
    
    head_node = Node(0)
    temp_node = head_node
    
    while l1_temp != None and l2_temp != None:
        if l1_temp.data < l2_temp.data:
            temp_node.bottom = l1_temp
            temp_node = temp_node.bottom
            l1_temp = l1_temp.bottom
        else:
            temp_node.bottom = l2_temp
            temp_node = temp_node.bottom
            l2_temp = l2_temp.bottom
    
    if l1_temp != None:
        temp_node.bottom = l1_temp
    else:
        temp_node.bottom = l2_temp
        
    return head_node.bottom

def flatten(root):
    if root == None or root.next == None:
        return root
    
    root.next = flatten(root.next)
    root = merge(root, root.next)
    
    return root
```

## [Rotate a Linked List](https://www.geeksforgeeks.org/clockwise-rotation-of-linked-list/)


```python
def rotateRight(head, k):
    if head == None or head.next == None or k == 0:
        return head

    temp_head = head
    n = 0
    while temp_head.next != None:
        n += 1
        temp_head = temp_head.next
    n += 1    

    last_node = temp_head

    total_rotation = k % n
    if total_rotation == 0:
        return head

    count = n - total_rotation - 1

    temp_node = head
    for _ in range(count):
        temp_node = temp_node.next
        
    '''
    For Anti-clockwise rotation in the for loop
    use 'total_rotation-1' instead of count.
    ''' 
        
    new_head = temp_node.next
    temp_node.next = None

    last_node.next = head

    return new_head
```

## [Copy Linked List with Random Pointer](https://www.geeksforgeeks.org/a-linked-list-with-next-and-arbit-pointer/)


```python
def copyRandomList(head):
    if head == None:
        return None

    temp_head = head

    while temp_head != None:
        new_node = Node(temp_head.val, temp_head.next)
        temp_head.next = new_node
        temp_head = new_node.next

    temp_head = head
    while temp_head != None:
        copy_node = temp_head.next
        copy_node.random = temp_head.random.next if temp_head.random != None else None
        temp_head = copy_node.next

    new_head = head.next
    head.next = new_head.next
    temp_head = new_head
    temp_node = new_head.next
    while temp_node != None:
        temp_head.next = temp_node.next
        temp_head = temp_head.next
        temp_node.next = temp_head.next
        temp_node = temp_node.next

    return new_head
```
