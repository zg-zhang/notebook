# 链表 - 概述

用一组任意存储的单元来存储线性表的数据元素，一个对象存储着本身的值和下一个元素的地址

* 需要遍历才能查询到元素，查询慢
* 插入元素只需要断开连接重新赋值，插入快

![链表](https://notebook-images.oss-cn-chengdu.aliyuncs.com/data-structure/%E9%93%BE%E8%A1%A8.png)

链表在开发中也是经常用到的数据结构，React 16 的 Fiber Node 连接起来形成的 Fiber Tree，就是个单链表结构

## 基本应用

主要是对链表基本概念和特性的应用，如果基础概念掌握牢靠，此类问题即可迎刃而解

* [从尾到头打印链表](https://github.com/zg-zhang/notebook/blob/master/Interview/data-structure/%E9%93%BE%E8%A1%A8/%E4%BB%8E%E5%B0%BE%E5%88%B0%E5%A4%B4%E6%89%93%E5%8D%B0%E9%93%BE%E8%A1%A8.md)
* [删除链表中的节点](https://github.com/zg-zhang/notebook/blob/master/Interview/data-structure/%E9%93%BE%E8%A1%A8/%E5%88%A0%E9%99%A4%E9%93%BE%E8%A1%A8%E4%B8%AD%E7%9A%84%E8%8A%82%E7%82%B9.md)
* [删除链表中重复的节点](https://github.com/zg-zhang/notebook/blob/master/Interview/data-structure/%E9%93%BE%E8%A1%A8/%E5%88%A0%E9%99%A4%E9%93%BE%E8%A1%A8%E4%B8%AD%E9%87%8D%E5%A4%8D%E7%9A%84%E8%8A%82%E7%82%B9.md)
* [反转链表](https://github.com/zg-zhang/notebook/blob/master/Interview/data-structure/%E9%93%BE%E8%A1%A8/%E5%8F%8D%E8%BD%AC%E9%93%BE%E8%A1%A8.md)
* [复杂链表的复制](https://github.com/zg-zhang/notebook/blob/master/Interview/data-structure/%E9%93%BE%E8%A1%A8/%E5%A4%8D%E6%9D%82%E9%93%BE%E8%A1%A8%E7%9A%84%E5%A4%8D%E5%88%B6.md)

## 环类链表

环类题目即从判断一个单链表是否存在循环而扩展衍生的问题

* [环状链表](https://github.com/zg-zhang/notebook/blob/master/Interview/data-structure/%E9%93%BE%E8%A1%A8/%E7%8E%AF%E7%8A%B6%E9%93%BE%E8%A1%A8.md)
* [链表环的入口节点](https://github.com/zg-zhang/notebook/blob/master/Interview/data-structure/%E9%93%BE%E8%A1%A8/%E9%93%BE%E8%A1%A8%E4%B8%AD%E7%8E%AF%E7%9A%84%E5%85%A5%E5%8F%A3%E8%8A%82%E7%82%B9.md)
* [约瑟夫环（圈圈中最后剩下的数字）](https://github.com/zg-zhang/notebook/blob/master/Interview/data-structure/%E9%93%BE%E8%A1%A8/%E9%93%BE%E8%A1%A8%E5%80%92%E6%95%B0%E7%AC%ACk%E4%B8%AA%E8%8A%82%E7%82%B9.md)

## 双指针

双指针的思想在链表和数组中的题目都会经常用到，主要是利用两个或多个不同位置的指针，通过速度和防线的变换来解决问题

* 两个指针从不同位置出发：一个从始端开始，另一个从末端开始
* 两个指针以不同速度移动：一个指针快一些，另一个指针慢一些

对于单链表，因为我们只能在一个方向上遍历链表，所以第一种情景可能无法工作，然而，第二种情景也被称为慢指针和快指针技巧，是非常有用的

* [两个链表的第一个公共节点](https://github.com/zg-zhang/notebook/blob/master/Interview/data-structure/%E9%93%BE%E8%A1%A8/%E4%B8%A4%E4%B8%AA%E9%93%BE%E8%A1%A8%E7%9A%84%E7%AC%AC%E4%B8%80%E4%B8%AA%E5%85%AC%E5%85%B1%E8%8A%82%E7%82%B9.md)
* [链表倒数第k个节点](https://github.com/zg-zhang/notebook/blob/master/Interview/data-structure/%E9%93%BE%E8%A1%A8/%E9%93%BE%E8%A1%A8%E5%80%92%E6%95%B0%E7%AC%ACk%E4%B8%AA%E8%8A%82%E7%82%B9.md)

## 双向链表

双链还有一个引用字段，称为 prev 字段，有了这个额外的字段，您就能够直到当前节点的前一个节点

* [扁平化多级双向链表]()
