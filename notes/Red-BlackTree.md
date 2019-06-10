# Red-Black Tree

## 概述

红黑树是一种自平衡的二叉查找树, 它的基本思想是用标准的二叉查找树 (完全由2-节点构成)和一些额外的信息(替换3-节点 )来表示2-3树。
可以说红黑树是2-3树的一种等同。

红黑树中的链接可以分为两种类型：

* 红链接: 它将两个2-节点连接起来构成一个3-节点 (也可以说是将3-节点表示为由一条红色左链接 (两个2-节点其中之一是另一个的左子节点)相连的两个2-节点)

* 黑链接: 表示2-3树中的普通链接

![redblack-encoding](https://github.com/xianfeng92/Fuck-Funny-DataStructure-Algorithms/blob/master/images/redblack-encoding.png)

这种表示方式带来的优点如下:

1. 无需修改就可以直接使用标准的 二叉查找树 中的查找方法

2. 对于任意的 2-3树,只要对节点进行转换,我们都可以立即派生出一棵对应的二叉查找树

## 红黑树的性质

红黑树是含有红黑链接并满足下列条件的二叉查找树(满足这些条件的 红黑树 才是与相应的 2-3树 一一对应的)

* 红链接均为左链接(这条仅限于偏向左红链接实现的 红黑树 )

* 每个节点不是红色就是黑色的

* 没有任何一个节点同时和两条红链接相连

* 该树是完美黑色平衡的,即任意空链接到根节点的路径上的黑链接数量相同

* 根节点 是黑色的

* 所有 叶子节点 (即null节点)的颜色是黑色的

## 与2-3树的对应关系

假如我们将一棵红黑树中的红链接画平,此时所有的空链接到根节点的距离都将是相同。如果再把由红链接相连的节点合并,得到的就是一棵 2-3树。
相对的,如果将一棵2-3树中的3-节点画作由红色左链接相连的两个2-节点 ,那么不会存在能够和两条红链接相连的节点,且树必然是完美黑色平衡的,
因为黑链接就是2-3树中的普通链接,根据定义这些链接必然是完美平衡的。

通过这些结论,__可以发现红黑树即是二叉查找树,也是2-3树__。

![redblack-1-1](https://github.com/xianfeng92/Fuck-Funny-DataStructure-Algorithms/blob/master/images/redblack-1-1.png)



## 节点的实现

我们使用 boolean 类型的变量 color 来表示链接的颜色。如果指向它的链接为红色,则 color 变量为 true ,黑色则为 false (空链接也为黑色)。
并且定义了一个 isRed() 函数用于判断链接的颜色。这里节点的__颜色指的是指向该节点的链接的颜色__。

![redblack-color](https://github.com/xianfeng92/Fuck-Funny-DataStructure-Algorithms/blob/master/images/redblack-color.png)

```
    private static final boolean RED = true;
    private static final boolean BLACK = false;

    private Node root; // root node

    private class Node {
        private K key;
        private V value;
        private Node left, right; // links to left and right subtress
        private boolean color; // color of parent link
        private int size; // subtree count

        public Node(K key, V value, boolean color, int size) {
            this.key = key;
            this.value = value;
            this.color = color;
            this.size = size;
        }
    }
	
	 // node x is red? if x is null return false.
    private boolean isRed(Node x) {
        if (x == null) return false;
        return x.color == RED;
    }
```

## 旋转

当我们在实现某些操作时,可能会产生一些红色右链接或者两条连续的红色左链接，此时就需要进行旋转操作来修复红黑树的平衡性。

旋转操作保证了红黑树的两个重要性质：__有序性__和__完美平衡性__。

### 左旋转

假设当前有一条红色右链接需要被修正旋转为左链接,这个操作叫做左旋转。


![redblack-left-rotate](https://github.com/xianfeng92/Fuck-Funny-DataStructure-Algorithms/blob/master/images/redblack-left-rotate.png)


### 右旋转

![redblack-right-rotate](https://github.com/xianfeng92/Fuck-Funny-DataStructure-Algorithms/blob/master/images/redblack-right-rotate.png)


## 颜色转换

颜色转换操作也是用于保证红黑树的性质的,它将父节点的颜色由黑变红,将子节点的颜色由红变黑。这项操作与旋转操作一样是局部变换, 
不会影响整棵树的黑色平衡性.

![color-flip](https://github.com/xianfeng92/Fuck-Funny-DataStructure-Algorithms/blob/master/images/color-flip.png)

__根节点总是为黑__

颜色转换可能会使根节点变为红色,但红色的根节点说明根节点是一个3-节点 的一部分,实际情况并不是这样的。所以我们需要将根节点设为黑色。

__每当根节点由红变黑时,树的黑链接高度就会加1__。


## 插入

在红黑树中实现插入操作是比较复杂的,因为需要保持红黑树的平衡性。但只要利用好左旋转 、右旋转 、颜色转换这三个辅助操作,就能够保证插入操作后树的平衡性。


## 删除

删除操作也需要定义一系列局部变换来在删除一个节点的同时保持树的完美平衡性。然而,这个过程要比插入操作还要复杂,它不仅要在(为了删除一个节点而)构造临时 4-节点
时沿着查找路径向下进行变换,还要在分解遗留的4-节点时沿着查找路径向上进行变换(同插入操作)。


# 总结

无论键的插入顺序如何,红黑树都几乎是完美平衡的,基于它实现的有序符号表操作的运行时间均为对数级别(除了范围查询)。
在红黑树的实现中复杂的代码仅限于 put() 和 delete() 方法,像 get() 这些不会涉及检查颜色的方法与二叉查找树中的实现一致(因为这些操作与平衡性无关)。

