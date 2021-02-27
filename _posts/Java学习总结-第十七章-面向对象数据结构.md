title: Java学习总结-第十七章 面向对象数据结构
date: 2016-03-22 23:44:38
categories: java
tags: [java,总结,原创]
---

** 本文总结源自《Java语言程序设计》原书第五版，作者为Y.daniel Liang，习题及编程练习均参照此书。**

---

## 主要内容

数据结构是按某种方式组织的数据集合。用面向对象的观点来看，一个数据结构就是一个存储着其他对象的对象，存储在数据结构内的对象称为数据或元素。因此数据结构又被称为`容器对象（container object）`或`集合对象（collection object）`。
本章介绍线性表、堆栈、队列和二叉树等四种动态数据结构。

### 线性表
- 线性表是存储顺序排列的数据时最常用的数据结构。
- 线性表可以由数组或者链表实现。这两种类具有相同的操作，但是具有不同的数据域，这些相同的操作可以由接口或抽象类生成。在设计时同时提供接口与抽象类是把它们的优点结合到一起的好策略，这样的抽象类称为`便利类`。
- 数组线性表
  + 数组是一种固定大小的数据结构。但仍然可以采用如下方法实现动态改变：当数组不能再存储线性表中的新元素时，创建一个更大的数组来代替当前数组。这样的数组线性表是一个动态的数据结构。
- 链表
  + 由数据创建的线性表，在特定位置进行插入和删除操作效率是很低的，因此可以采用链表结构来提高效率。
  + 链表的遍历：
  ```java
  	Node temp = first;
    for (int i = 1; i < size; i++) {
        temp = temp.next;
    }
  ```

### 栈和队列
- 栈（stack）可以看做是一种特殊的线性表，访问、插入和删除它的元素只能在栈的一端（栈顶）进行。队列可以表示一个排队等待的队伍。它可以看做是一种特殊的线性表，它的元素只能从队列的末端（队列尾）插入，从开始端（队列头）访问和删除。
- 栈：使用数组线性表来实现栈效率较高。
- 队列：用链表实现。

### 二叉树
- 线性表、栈和队列都是由一列元素构成的线型结构。二叉树是一个层次结构，它要么是空集，要么由一个被称为`根（root）`的元素和两棵不同的子树组成，这两个子树分别称为`左子树（left subtree）`和`右子树（right subtree）`。
- 一个结点的左（右）子树的根节点称为该结点的`左（右）孩子（left（right）child）`。没有孩子的结点称为`叶结点（leaf）`。`二叉搜索树（binary search tree）`是一种常用的特殊二叉树。二叉搜索树的特征是：对它的每一个结点来说，左子树中结点的值都小于该结点的值，右子树中的结点的值都大于该节点的值。

#### 二叉树的遍历
- `中序遍历（inorder）:`首先访问当前结点的左子树，然后访问当前结点，最后访问该结点的右子树。
```java
	private void inorder(TreeNode root) {
        if (root == null) {
            return;
        }
        inorder(root.left);
        System.out.print(root.element + " ");
        inorder(root.right);
    }
```
- `后序遍历（postorder）：`首先访问当前结点的左子树，然后访问该结点的右子树，最后访问该结点。
```java
	private void postorder(TreeNode root) {
        if (root==null) return;
        postorder(root.left);
        postorder(root.right);
        System.out.print(root.element + " ");
    }
```
- `前序遍历（preorder）：`首先访问当前结点，然后访问该结点的左子树，最后访问该结点的右子树。
```java
	private void preorder(TreeNode root) {
        if (root == null) return;
        System.out.print(root.element + " ");
        preorder(root.left);
        preorder(root.right);
    }
```
- `深度优先遍历（depth-first）：`与前序遍历相同。
- `广度遍历法（breadth-first）：`一层一层的访问树的结点。
**如果元素的插入顺序不同，树看起来可能不一样，但是只要元素的集合相同，中序序列是一样的。**



---

## 复习小结
- 基本类型的数据不能作为对象存储，但是可以给基本数据类型值创建对应包装类的对象。

---

## 编程练习
- 习题17.1、17.2、17.3、17.6源代码见我的Github： [chapter17](https://github.com/whtis/Java-Exercises/tree/master/chapter17/src)
- 写双向链表时，在执行下列方法：
```java
public Object remove(int index) {
    TreeNode current = first;
    for (int i = 1; i < index; i++) {
        current = current.next;
    }
    TreeNode target = current.next;
    target.pravious.next = target.next;
    target.next.pravious = target.pravious;
    size--;
    return true;
}
```
执行到第7行语句时：list的状态是`Method threw 'java.lang.NullPointerException' exception. Cannot evaluate _17_3.DoubleLinkedList.toString()`,但是整个方法执行完后，不会报错，原因暂时不知道。
- 习题17.6中需要写一个方法，返回二叉树的深度，深度是指二叉树最长路径的结点个数。代码如下：
```java
public static int treeDepth(TreeNode root) {
    if (root == null) {
        return 0;
    }
    int left = treeDepth(root.left);
    int right = treeDepth(root.right);
    return left > right ? (left + 1) : (right + 1);
}
```

---
<div align="center" style="color:red;width=80px;height:90px;" onmouseout="this.style.border='1px solid blue'" onmouseover="this.style.border='none'">
<p style="font-weight:bold;font-style:italic;">本文章首发[www.whtis.com](http://www.whtis.com)，转载请注明出处</p>
</div>