---
{
  "title": "链表倒数第k个节点",
}
---
## 题目

输入一个链表，输出该链表中倒数第k个结点。
链表的倒数第 k 个节点即为正数第n−k+1 个节点
## 思路1

简单思路： 循环到链表末尾找到 length  在找到length-k节点   需要循环两次。

## 代码
```js
//时间复杂度：O(n) 空间复杂度：O(1)
var getKthFromEnd = function(head, k) {
  var len=0;
  var root=head;
  while(root){
    len++;
    root=root.next;
  }
  var index=0;
  root=head;
  //for循环也可以
  while(root){
    index++
    if(index===len-k+1){
        return root;
    }
    root=root.next;
  }
};
//结合哈希，不用遍历两次     
//时间复杂度：O(n) 空间复杂度：O(n)
var getKthFromEnd = function(head, k) {
    var map=new Map();
    var index=0;
    while(head){
        index++;
        map.set(index,head);
        head=head.next;
    }
    return map.get(index-k+1);
};
```
## 思路2
优化（双指针法）：

只要让快与慢指针保持k的长度同时前进即可（快指针先走k），当快指针到末尾时，慢指针则在末尾前k个的位置。

代码鲁棒性： 需要考虑head为null，k为0，k大于链表长度的情况。

鲁棒性是指程序能够判断输入是否合乎规范要求，并对不符合要求的输入予以合理的处理。

## 代码

```js
    //双指针法。时间复杂度：O(n) 空间复杂度：O(1)
var getKthFromEnd = function(head, k) {
  let slow = head, fast = head
  while (k--) {
    fast = fast.next
  }
  while (fast) {
    slow = slow.next
    fast = fast.next
  }
  return slow
};
```