---
{
  "title": "删除链表中的节点or重复的节点",
}
---

## 删除链表中的节点

给定单链表中的一个的指针节点，在O(1)时间内删除该节点。注意，删除节点并不是指从内存中删除它，node是链表中的节点，且不是末尾节点，节点的值都是唯一的。这里的意思是：
- 给定节点的值不应该存在于链表中。
- 链表中的节点数应该减少 1。
- node 前面的所有值顺序相同。
- node 后面的所有值顺序相同。

此题实际并没有真实的删除该节点，而是把当前节点易容成了next节点，然后删除真实的next节点。
（既然不能先删除自己，那就把自己整容成儿子，再假装自己就是儿子来养活孙子）
```js
    var deleteNode = function (node) {
        node.val = node.next.val;
        node.next = node.next.next;
   };
```
## 删除链表中的节点-变种
给定单向链表的头指针和一个要删除的节点的值，定义一个函数删除该节点。
```js 
//双指针
var deleteNode = function (head, val) {
    if(head.val==val){
        head=head.next;
    }else{
        var pre=head;
        var cur=head.next;
        while(cur&&cur.val!==val){
            pre=cur;
            cur=cur.next;
        }
        pre.next=pre.next.next;
    }
    return head;
};

//单指针
var deleteNode = function (head, val) {
    if(head.val==val){
        head=head.next;
    }else{
        var pre=head;
        while(pre.next&&pre.next.val!==val){
            pre=pre.next;
        }
        pre.next=pre.next.next;
    }
    return head;
};
```

## 删除链表中重复的节点

### 方法1.存储链表中元素出现的次数

- 1.用一个map存储每个节点出现的次数
- 2.删除出现次数大于1的节点

此方法删除节点时可以使用上面总结的办法。

时间复杂度：O(n) 

空间复杂度：O(n)

```js
    //单指针
    function deleteDuplication(pHead) {
      const map = new Map();
      var cur=pHead;
      var pre=null;
      while (cur) {
         if(map.has(cur)){
             pre.next=cur.next;
         }
         map.set(cur);
         pre=cur;
         cur = cur.next;
      }
      return pHead;
    }
```


### 方法2.重新比较连接数组


链表是排好顺序的，所以重复元素都会相邻出现
       递归链表：
- 1.当前节点或当前节点的next为空，返回该节点
- 2.当前节点是重复节点：找到后面第一个不重复的节点
- 3.当前节点不重复：将当前的节点的next赋值为下一个不重复的节点

```js
    function deleteDuplication(pHead) {
      if (!pHead || !pHead.next) {
        return pHead;
      } else if (pHead.val === pHead.next.val) {
        let tempNode = pHead.next;
        while (tempNode && pHead.val === tempNode.val) {
          tempNode = tempNode.next;
        }
        return deleteDuplication(tempNode);
      } else {
        pHead.next = deleteDuplication(pHead.next);
        return pHead;
      }
    }
```


时间复杂度：O(n) 

空间复杂度：O(1)


## 考察点

- 链表
- 考虑问题的全面性