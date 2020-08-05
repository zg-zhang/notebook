# 数据结构

> 数据结构即数据元素相互之间存在的一种和多种特定的关系的集合

一般你可以从两个维度来理解它，逻辑结构 和 存储结构

## 逻辑结构

简单来说逻辑结构就是数据之间的关系，逻辑结构大概统一的可以分成两种：线性结构、非线性结构。

### 线性结构

是一个有序数据元素的集合，其中数据元素之间的关系是一对一的关系，即除了第一个和最后一个数据元素之外，其他数据元素都是收尾相连的。

常用的线性结构有：栈、队列、链表、线性表

### 非线性结构

各个数据元素不再保持在一个线性序列中，每个数据元素可能与零个或者多个其他数据元素发生联系

常见的非线性结构有：二维数组、树等

## 存储结构

逻辑结构指的是数据间的关系，而存储结构是逻辑结构用计算机语言的实现。常见的存储结构有顺序结构、链式结构、索引存储以及散式存储。

例如：数组在内存中的位置是连续的，它就属于顺序存储；链表是主动建立数据间的关联关系的，在内存中却不一定是连续的，它属于链式存储；还有顺序和逻辑上都不存在顺序关系，但你可以通过一定的方式去问它的哈希表，数据散列存储

## 数据结构 - 二叉树

[二叉树 - 概述](https://github.com/zg-zhang/notebook/blob/master/Interview/data-structure/%E4%BA%8C%E5%8F%89%E6%A0%91/README.md)

### 目录

* [二叉树的基本操作](https://github.com/zg-zhang/notebook/blob/master/Interview/data-structure/%E4%BA%8C%E5%8F%89%E6%A0%91/%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E5%9F%BA%E6%9C%AC%E6%93%8D%E4%BD%9C.md)
* [二叉树的前序遍历](https://github.com/zg-zhang/notebook/blob/master/Interview/data-structure/%E4%BA%8C%E5%8F%89%E6%A0%91/%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E5%89%8D%E5%BA%8F%E9%81%8D%E5%8E%86.md)
* [二叉树的中序遍历](https://github.com/zg-zhang/notebook/blob/master/Interview/data-structure/%E4%BA%8C%E5%8F%89%E6%A0%91/%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E4%B8%AD%E5%BA%8F%E9%81%8D%E5%8E%86.md)
* [二叉树的后序遍历](https://github.com/zg-zhang/notebook/blob/master/Interview/data-structure/%E4%BA%8C%E5%8F%89%E6%A0%91/%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E5%90%8E%E5%BA%8F%E9%81%8D%E5%8E%86.md)
* [重建二叉树](https://github.com/zg-zhang/notebook/blob/master/Interview/data-structure/%E4%BA%8C%E5%8F%89%E6%A0%91/%E9%87%8D%E5%BB%BA%E4%BA%8C%E5%8F%89%E6%A0%91.md)
* [求二叉树的遍历](https://github.com/zg-zhang/notebook/blob/master/Interview/data-structure/%E4%BA%8C%E5%8F%89%E6%A0%91/%E6%B1%82%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E9%81%8D%E5%8E%86.md)
* [对称的二叉树](https://github.com/zg-zhang/notebook/blob/master/Interview/data-structure/%E4%BA%8C%E5%8F%89%E6%A0%91/%E5%AF%B9%E7%A7%B0%E7%9A%84%E4%BA%8C%E5%8F%89%E6%A0%91.md)
* [二叉树的镜像](https://github.com/zg-zhang/notebook/blob/master/Interview/data-structure/%E4%BA%8C%E5%8F%89%E6%A0%91/%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E9%95%9C%E5%83%8F.md)
* [二叉搜索树的第k个节点](https://github.com/zg-zhang/notebook/blob/master/Interview/data-structure/%E4%BA%8C%E5%8F%89%E6%A0%91/%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91%E7%9A%84%E7%AC%ACk%E4%B8%AA%E8%8A%82%E7%82%B9.md)
* [二叉搜索树的后序遍历](https://github.com/zg-zhang/notebook/blob/master/Interview/data-structure/%E4%BA%8C%E5%8F%89%E6%A0%91/%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91%E7%9A%84%E5%90%8E%E5%BA%8F%E9%81%8D%E5%8E%86.md)
* [二叉树的最大深度](https://github.com/zg-zhang/notebook/blob/master/Interview/data-structure/%E4%BA%8C%E5%8F%89%E6%A0%91/%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E6%9C%80%E5%A4%A7%E6%B7%B1%E5%BA%A6.md)
* [二叉树的最小深度](https://github.com/zg-zhang/notebook/blob/master/Interview/data-structure/%E4%BA%8C%E5%8F%89%E6%A0%91/%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E6%9C%80%E5%B0%8F%E6%B7%B1%E5%BA%A6.md)
* [平衡二叉树](https://github.com/zg-zhang/notebook/blob/master/Interview/data-structure/%E4%BA%8C%E5%8F%89%E6%A0%91/%E5%B9%B3%E8%A1%A1%E4%BA%8C%E5%8F%89%E6%A0%91.md)
* [不分行从上到下打印二叉树](https://github.com/zg-zhang/notebook/blob/master/Interview/data-structure/%E4%BA%8C%E5%8F%89%E6%A0%91/%E4%B8%8D%E5%88%86%E8%A1%8C%E4%BB%8E%E4%B8%8A%E5%88%B0%E4%B8%8B%E6%89%93%E5%8D%B0%E4%BA%8C%E5%8F%89%E6%A0%91.md)
* [把二叉树打印成多行](https://github.com/zg-zhang/notebook/blob/master/Interview/data-structure/%E4%BA%8C%E5%8F%89%E6%A0%91/%E6%8A%8A%E4%BA%8C%E5%8F%89%E6%A0%91%E6%89%93%E5%8D%B0%E6%88%90%E5%A4%9A%E8%A1%8C.md)
* [二叉树中和为某一值的路径](https://github.com/zg-zhang/notebook/blob/master/Interview/data-structure/%E4%BA%8C%E5%8F%89%E6%A0%91/%E4%BA%8C%E5%8F%89%E6%A0%91%E4%B8%AD%E5%92%8C%E4%B8%BA%E6%9F%90%E4%B8%80%E5%80%BC%E7%9A%84%E8%B7%AF%E5%BE%84.md)
* [二叉搜索树与双向链表](https://github.com/zg-zhang/notebook/blob/master/Interview/data-structure/%E4%BA%8C%E5%8F%89%E6%A0%91/%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91%E4%B8%8E%E5%8F%8C%E5%90%91%E9%93%BE%E8%A1%A8.md)
* [按之字顺序打印二叉树](https://github.com/zg-zhang/notebook/blob/master/Interview/data-structure/%E4%BA%8C%E5%8F%89%E6%A0%91/%E6%8C%89%E4%B9%8B%E5%AD%97%E5%BD%A2%E9%A1%BA%E5%BA%8F%E6%89%93%E5%8D%B0%E4%BA%8C%E5%8F%89%E6%A0%91.md)
* [序列化二叉树](https://github.com/zg-zhang/notebook/blob/master/Interview/data-structure/%E4%BA%8C%E5%8F%89%E6%A0%91/%E5%BA%8F%E5%88%97%E5%8C%96%E4%BA%8C%E5%8F%89%E6%A0%91.md)
* [二叉树的下一个节点](https://github.com/zg-zhang/notebook/blob/master/Interview/data-structure/%E4%BA%8C%E5%8F%89%E6%A0%91/%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E4%B8%8B%E4%B8%80%E4%B8%AA%E8%8A%82%E7%82%B9.md)
* [树的子结构](https://github.com/zg-zhang/notebook/blob/master/Interview/data-structure/%E4%BA%8C%E5%8F%89%E6%A0%91/%E6%A0%91%E7%9A%84%E5%AD%90%E7%BB%93%E6%9E%84.md)
