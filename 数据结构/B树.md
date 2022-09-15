# 二叉查找树（Binary Search Trees）

1、每个结点都包含一个元素以及n个子树，这里0≤n≤2。
2、左子树和右子树是有顺序的，次序不能任意颠倒。左子树的值要小于父结点，右子树的值要大于父结点。

## 构建二叉树

[35 27 48 12 29 38 55]

![](E:\wok\文档\doc\image\data_structure\tree\b_1_1.jpg)

![b_1_2](E:\wok\文档\doc\image\data_structure\tree\b_1_2.jpg)

![b_1_3](E:\wok\文档\doc\image\data_structure\tree\b_1_3.jpg)

![b_1_4](E:\wok\文档\doc\image\data_structure\tree\b_1_4.jpg)

![b_1_5](E:\wok\文档\doc\image\data_structure\tree\b_1_5.jpg)

![b_1_6](E:\wok\文档\doc\image\data_structure\tree\b_1_6.jpg)



## 极端二叉树

[12 27 29 35 38 48 55]

![](E:\wok\文档\doc\image\data_structure\tree\b_2_1.jpg)





# 平衡二叉树 (AVL Trees)

是一个特殊的二叉树同时

它的左右两个子树的高度差的绝对值不超过1，并且左右两个子树都是一棵平衡二叉树。

## 构建平衡二叉树



![](E:\wok\文档\doc\image\data_structure\tree\avl_b_1_1.jpg)

![avl_b_1_2](E:\wok\文档\doc\image\data_structure\tree\avl_b_1_2.jpg)

![avl_b_1_3](E:\wok\文档\doc\image\data_structure\tree\avl_b_1_3.jpg)

![avl_b_1_4](E:\wok\文档\doc\image\data_structure\tree\avl_b_1_4.jpg)

![avl_b_1_5](E:\wok\文档\doc\image\data_structure\tree\avl_b_1_5.jpg)

![avl_b_1_6](E:\wok\文档\doc\image\data_structure\tree\avl_b_1_6.jpg)

![avl_b_1_7](E:\wok\文档\doc\image\data_structure\tree\avl_b_1_7.jpg)



## 二叉树节点旋转

旋转是二叉树的基本操作，我们可以对任意一个存在父亲节点的子节点进行旋转，包括如下几种形式（设被旋转节点为x，其父亲节点为p）

旋转的方式：也就是把父节点旋转到旋转点的子节点，然后把旋转点多余的子节点给父节点

### 左旋

旋转前，x是p的右儿子。
x的左儿子（若存在）变为p的右儿子，p变为x的左儿子

![](E:\wok\文档\doc\image\data_structure\tree\B_LEFT.jpg)

### 右旋

旋转前，x是p的左儿子。
x的右儿子（若存在）变为p的左儿子，p变为x的右儿子

![](E:\wok\文档\doc\image\data_structure\tree\B_RIGHT.jpg)

# B-Tree

1、每个结点最多m个子结点。
2、除了根结点和叶子结点外，每个结点最少有m/2（向上取整）个子结点。
3、如果根结点不是叶子结点，那根结点至少包含两个子结点。
4、所有的叶子结点都位于同一层。
5、每个结点都包含k个元素（关键字），这里m/2≤k<m，这里m/2向下取整。
6、每个节点中的元素（关键字）从小到大排列。
7、每个元素（关键字）字左结点的值，都小于或等于该元素（关键字）。右结点的值都大于或等于该元素（关键字）。

## 构建B-Tree

![](E:\wok\文档\doc\image\data_structure\tree\Btree_1_1.jpg)

![Btree_1_2](E:\wok\文档\doc\image\data_structure\tree\Btree_1_2.jpg)

![Btree_1_3](E:\wok\文档\doc\image\data_structure\tree\Btree_1_3.jpg)

![Btree_1_4](E:\wok\文档\doc\image\data_structure\tree\Btree_1_4.jpg)

![Btree_1_5](E:\wok\文档\doc\image\data_structure\tree\Btree_1_5.jpg)

![Btree_1_6](E:\wok\文档\doc\image\data_structure\tree\Btree_1_6.jpg)

![Btree_1_7](E:\wok\文档\doc\image\data_structure\tree\Btree_1_7.jpg)

## 遍历B-Tree

![](E:\wok\文档\doc\image\data_structure\tree\Btree_2_1.jpg)

![Btree_2_2](E:\wok\文档\doc\image\data_structure\tree\Btree_2_2.jpg)

![Btree_2_3](E:\wok\文档\doc\image\data_structure\tree\Btree_2_3.jpg)

​	从这个流程我们能看出，B-Tree的查询效率好像也并不比平衡二叉树高。但是查询所经过的结点数量要少很多，也就意味着要少很多次的磁盘IO，这对
性能的提升是很大的。

​	前面对B-Tree操作的图我们能看出来，元素就是类似1、2、3这样的数值，但是数据库的数据都是一条条的数据，如果某个数据库以B-Tree的数据结构存储数据，那数据怎么存放的呢？

![](E:\wok\文档\doc\image\data_structure\tree\Btree_data_1.jpg)



# B+Tree

1、所有的非叶子节点只存储关键字信息。
2、所有卫星数据（具体数据）都存在叶子结点中。
3、所有的叶子结点中包含了全部元素的信息。
4、所有叶子节点之间都有一个链指针。



![](E:\wok\文档\doc\image\data_structure\tree\Btee+_1.jpg)