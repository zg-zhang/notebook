# 二叉树

树是用来模拟具有树状结构性质的数据集合。根据它的特性可以分为非常多的种类，对于我们来讲，掌握二叉树这种结构就足够了，他也是树最简单、应用最广泛的种类。

> 二叉树是一种典型的树状结构，如它的名字所描述的那样，二叉树是每个节点最多有两个子树的树结构，通常子树被称作 左子树 和 右子树

* 树结点：没有父结点的结点
* 叶结点：没有子结点的结点
* 内部结点：一个结点既不是根节点也不是叶节点

* 满二叉树：二叉树中每个内部结点都有存在左子树和右子树，或者说满二叉树所有的叶节点都有同样的深度。 
    * 满二叉树一定是完全二叉树，但是完全二叉树不一定是满二叉树
    * 满二叉树的严格定义是：一颗深度为 h 且有 2^h - 1 个结点的二叉树
* 完全二叉树：就是从上往下填结点，从左往右填，填满了一层才能填下一层，除最后一层外，每一层的节点数均达到最大值，在最后一层上只缺少右边的若干结点
    ![完全二叉树](https://notebook-images.oss-cn-chengdu.aliyuncs.com/data-structure/%E5%AE%8C%E5%85%A8%E4%BA%8C%E5%8F%89%E6%A0%91.jpg)
    
其他名词解释：

* 结点的度：结点拥有的子树的数目
* 叶子结点：度为0的结点，在任意一个二叉树种，度为0的叶子节点总是比度为2的结点多一个
* 分支结点：度不为0的结点
* 树的度：树中结点的最大的度
* 层次：根节点的层次为1，其余结点的层次等于该节点的双亲结点的层次加1
* 树的高度：树种结点的最大层次

## 二叉树的基本操作

* [二叉树的基本操作](https://github.com/zg-zhang/notebook/blob/master/Interview/data-structure/%E4%BA%8C%E5%8F%89%E6%A0%91/%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E5%9F%BA%E6%9C%AC%E6%93%8D%E4%BD%9C.md)

### 二叉树遍历

重点中的重点，最好同时掌握递归和非递归版本，递归版本很容易书写，但是真正考察基本功的是非递归版本。

* [二叉树的前序遍历](https://github.com/zg-zhang/notebook/blob/master/Interview/data-structure/%E4%BA%8C%E5%8F%89%E6%A0%91/%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E5%89%8D%E5%BA%8F%E9%81%8D%E5%8E%86.md) :首先访问根结点，然后遍历左子树，最后遍历右子树
* [二叉树的中序遍历](https://github.com/zg-zhang/notebook/blob/master/Interview/data-structure/%E4%BA%8C%E5%8F%89%E6%A0%91/%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E4%B8%AD%E5%BA%8F%E9%81%8D%E5%8E%86.md) :首先遍历左子树，然后访问根结点，最后遍历右子树
* [二叉树的后序遍历](https://github.com/zg-zhang/notebook/blob/master/Interview/data-structure/%E4%BA%8C%E5%8F%89%E6%A0%91/%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E5%90%8E%E5%BA%8F%E9%81%8D%E5%8E%86.md) :首先遍历左子树，然后遍历右子树，最后访问根结点

根据前序遍历和中序遍历的特点重建二叉树，逆向思维，很有意思的题目。

* [重建二叉树](https://github.com/zg-zhang/notebook/blob/master/Interview/data-structure/%E4%BA%8C%E5%8F%89%E6%A0%91/%E9%87%8D%E5%BB%BA%E4%BA%8C%E5%8F%89%E6%A0%91.md)
* [求二叉树的遍历](https://github.com/zg-zhang/notebook/blob/master/Interview/data-structure/%E4%BA%8C%E5%8F%89%E6%A0%91/%E6%B1%82%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E9%81%8D%E5%8E%86.md)

### 二叉树的对称性

* [对称的二叉树](https://github.com/zg-zhang/notebook/blob/master/Interview/data-structure/%E4%BA%8C%E5%8F%89%E6%A0%91/%E5%AF%B9%E7%A7%B0%E7%9A%84%E4%BA%8C%E5%8F%89%E6%A0%91.md)
* [二叉树的镜像](https://github.com/zg-zhang/notebook/blob/master/Interview/data-structure/%E4%BA%8C%E5%8F%89%E6%A0%91/%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E9%95%9C%E5%83%8F.md)
