# 链表倒数第k个节点

## 题目

输入一个链表，输出该链表中倒数第k个节点

## 思路

简单思路：循环到链表末尾找到 length 在找到 length-k 节点，但需要循环两次

优化：

设定两个节点，间距相差 k 个节点，当前面的节点到达终点，取后面的节点

前面的节点到达 k 后，后面的节点才出发

代码鲁棒性：需要考虑 head = null、k = 0、k 大于链表长度的情况

## 代码

```javascript
function FindKthToTail(head, k) {
  if (!head || !k) {
    return null;
  }
  let front = head;
  let behind = head;
  let index = 1;
  while (front.next) {
    index++;
    front = front.next;
    if (index > k) {
      behind = behind.next;
    }
  }
  return (k <= index) && behind;
}
```
