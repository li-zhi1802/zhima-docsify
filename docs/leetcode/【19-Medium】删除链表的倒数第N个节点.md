## 删除链表的倒数第N个节点

给你一个链表，删除链表的倒数第 `n` 个结点，并且返回链表的头结点。

**示例 1**：

![example](【19-Medium】删除链表的倒数第N个节点/example.jpg)

```
输入：head = [1,2,3,4,5], n = 2
输出：[1,2,3,5]
```

**示例 2：**

```
输入：head = [1], n = 1
输出：[]
```

**示例 3：**

```
输入：head = [1,2], n = 1
输出：[1]
```

**提示：**

- 链表中结点的数目为 `sz`
- `1 <= sz <= 30`
- `0 <= Node.val <= 100`
- `1 <= n <= sz`

**进阶：**你能尝试使用一趟扫描实现吗？

* Related Topics
* 链表
* 双指针

### 双指针

这道题典型的用快慢指针法来解决

将`slow`和`fast`指向头部，将`fast`指针先往后走n步

然后两者齐头并进，当`fast.next`为`null`的时候，`slow`已经在倒数第n-1个节点的位置了，此时进行删除操作

该方法的原理是利用**步长**，fast先走的那几步，在`fast`到了链表末尾的`null`的时候，`slow`则表示的是倒数第n个节点，

因为这里需要执行删除操作，所以需要将`fast`走到链表末尾即可

> 示例代码

```java
public ListNode removeNthFromEnd(ListNode head, int n) {
    ListNode slow = head;
    ListNode fast = head;
    // fast先走 n 步
    for (int i = 0; i < n && fast != null; i++) {
        fast = fast.next;
    }
    // 说明总长度比 n 大，删除头节点即可
    if (fast == null) {
        return head.next;
    }
    // 快慢指针同时向后走
    while (fast.next != null) {
        slow = slow.next;
        fast = fast.next;
    }
    // slow的next就是倒数第 n 个节点
    // 删除即可
    slow.next = slow.next.next;
    return head;
}
```
