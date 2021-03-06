# Binary Search Tree

树是由多个节点（Node）的集合组成，每个节点又有多个与其关联的子节点（Child Node）。子节点就是直接处于节点之下的节点，
而父节点（Parent Node）则位于节点直接关联的上方。树的根（Root）指的是一个没有父节点的单独的节点。

所有的树都呈现一些共有的性质:

1. 只有一个根节点

2. 除了根节点，所有节点都有且只有一个父节点

3. 无环。将任意一个节点作为起始节点, 都不存任何回到该节点的路径

## 树中的相关术语

* __根（Root）__:树中最顶端的节点，根没有父节点。

* __子节点（Child）__:节点所拥有子树的根节点称为该节点的字节点。

* __父节点(Parent)__:如果节点拥有子节点，则该节点为子节点的父节点。

* __兄弟节点(Sibling)__:拥有相同父节点的节点。

* __子孙节点（Descendant）__:节点向下路径上可达的节点。

* __叶节点（Leaf）__:没有子节点的节点。

* __内节点（Internal Node）__:至少有一个子节点的节点。

* __度（Degree）__:节点拥有子树的数量。

* __边（Edge）__:两个节点中间的连接。

* __路径（Path）__:从节点到子孙节点过程中的边和节点所组成的序列。

* __层级（Level）__:根的 Level 为 0 层，根的子节点为 Level 1 层，以此类推。

* __高度（Height）/深度（Depth）__:树中层的数量。比如只有 Level 0，Level 1，Level 2 则高度为3。

## 二叉树

二叉树（Binary Tree）是一种特殊的树类型, 其每个节点最多只能有两个子节点。这两个子节点分别称为当前节点的左孩子（left child）
和右孩子（right child）。

### 完全二叉树和满二叉树

满二叉树（Full Binary Tree）: 一棵深度为 h, 且有 2^h - 1 个节点称之为满二叉树。即除了叶子节点, 每个节点都有两个孩子节点。

完全二叉树（Complete Binary Tree）：深度为 h, 有 n 个节点的二叉树, 当且仅当其每一个节点都与深度为 h 的满二叉树中, 序号为 1 
至 n 的节点对应时，称之为完全二叉树。

![completeBinaryTreeAndFullBinaryTree](https://github.com/xianfeng92/Fuck-Funny-DataStructure-Algorithms/blob/master/images/completeBinaryTreeAndFullBinaryTree.jpg)

我们知道数组是将元素连续地排列在内存当中，而二叉树却不是采用连续的内存存放。实际上，通常 BinaryTree 类的实例仅包含根节点（Root Node）
实例的引用, 而根节点实例又分别指向它的左右孩子节点实例,以此类推。所以关键的不同之处在于,组成二叉树的节点对象实例可以分散到 CLR 托管堆中
的任何位置，它们没有必要像数组元素那样连续的存放。

![BinaryTreeMemory](https://github.com/xianfeng92/Fuck-Funny-DataStructure-Algorithms/blob/master/images/BinaryTreeMemory.gif)

如果要访问二叉树中的某一个节点, 通常需要逐个遍历二叉树中的节点, 来定位那个节点。它不像数组那样能对指定的节点进行直接的访问。所以查找二叉树的渐进时间复杂度是线性的 O(n), 在最坏的情况下需要查找树中所有的节点。也就是说, 随着二叉树节点数量增加, 查找任一节点的步骤数量也将相应地增加。

那么，如果一个二叉树的查找时间是线性的, 定位时间也是线性的, 那相比数组来说到底哪里有优势呢？

毕竟数组的查找时间虽然是线性 O(n)，但定位时间却是常量 O(1) 啊？

的确是这样，通常来说普通的二叉树确实不能提供比数组更好的性能。然而，如果我们按照一定的规则来组织排列二叉树中的元素时，就可以很大
程度地改善查询时间和定位时间。

如下图,普通的二叉树:

![Normal_BinaryTree](https://github.com/xianfeng92/Fuck-Funny-DataStructure-Algorithms/blob/master/images/Normal_BinaryTree.png)

在此基础上, 加上节点之间的大小关系, 就是二叉查找树:

![BST](https://github.com/xianfeng92/Fuck-Funny-DataStructure-Algorithms/blob/master/images/BST.png)

二叉查找树（Binary Search Tree）是指一棵空树或者具有下列性质的二叉树:

1. 若任意节点的左子树不空, 则左子树上所有结点的值均小于它的根结点的值

2. 若任意节点的右子树不空, 则右子树上所有结点的值均大于它的根结点的值

3. 任意节点的左、右子树也分别为二叉查找树

4. 没有键值相等的节点（no duplicate nodes）

从二叉查找树的性质可知, 其各节点存储的数据必须能够与其他的节点进行比较。给定任意两个节点, BST 必须能够判断这两个节点的值是小于、大于还是等于。

## 二叉查找树的实现

我们需要定义一个内部类 Node, 它包含两个分别指向左右节点的 Node, 一个用于排序的 e 。

```
public class BST<E extends Comparable<E>> {

    private class Node{
        public E e;
        public Node left;
        public Node right;

        public Node(E e){
            this.e = e;
            left = null;
            right = null;
        }
    }
```

## 查找

假设我们要查找节点 n，从 BST 的根节点开始。算法不断地比较节点值的大小直到找到该节点，或者判定不存在。每一步我们都要处理两个节点：
树中的一个节点，称为节点 c，和要查找的节点 n，然后并比较 c 和 n 的值。开始时，节点 c 为 BST 的根节点。然后执行以下步骤：

1. 如果 c 值为空，则 n 不在 BST 中

2. 比较 c 和 n 的值

3. 如果值相同，则找到了指定节点 n

4. 如果 n 的值小于 c，那么如果 n 存在，必然在 c 的左子树中。回到第 1 步，将 c 的左孩子作为 c

5. 如果 n 的值大于 c，那么如果 n 存在，必然在 c 的右子树中。回到第 1 步，将 c 的右孩子作为 c

通过 BST 查找节点，理想情况下我们需要检查的节点数可以减半。如下图中的 BST 树，包含了 15 个节点。从根节点开始执行查找算法，第一次比较决
定我们是移向左子树还是右子树。对于任意一种情况，一旦执行这一步，我们需要访问的节点数就减少了一半，从 15 降到了 7。同样，下一步访问的节
点也减少了一半，从 7 降到了 3，以此类推。

![demo_BST_Search](https://github.com/xianfeng92/Fuck-Funny-DataStructure-Algorithms/blob/master/images/demo_BST_Search.gif)

根据这一特点，查找算法的时间复杂度应该是 O(lg n)。而实际上，对于 BST 查找算法来说，其十分依赖于树中节点的拓扑结构，也就是节点间的布局关系。
下图描绘了一个节点插入顺序为 20, 50, 90, 150, 175, 200 的 BST 树。这些节点是按照递升顺序被插入的，结果就是这棵树没有广度（Breadth）可言。
也就是说，它的拓扑结构其实就是将节点排布在一条线上，而不是以扇形结构散开，所以查找时间也为 O(n)。

![demo_BST_Search2](https://github.com/xianfeng92/Fuck-Funny-DataStructure-Algorithms/blob/master/images/demo_BST_Search2.gif)

当 BST 树中的节点以扇形结构散开时，对它的插入、删除和查找操作最优的情况下可以达到亚线性的运行时间 O(log2n)。因为当在 BST 中查找一个节点时，
每一步比较操作后都会将节点的数量减少一半。尽管如此，如果拓扑结构像上图中的样子时，运行时间就会退减到线性时间 O(n)。因为每一步比较操作后还是
需要逐个比较其余的节点。也就是说，在这种情况下，在 BST 中查找节点与在数组（Array）中查找就基本类似了。

__因此，BST 算法查找时间依赖于树的拓扑结构。最佳情况是 O(log­2n)，而最坏情况是 O(n)。__

查找的具体代码实现如下:

```
    // 以 node 为根的二分搜索树中是否包含元素e, 递归算法
    private boolean contains(Node node, E e){
        if (node == null){
            return false;
        }
        if (e.compareTo(node.e) < 0){
            return contains(node.left,e);
        }else if(e.compareTo(node.e) > 0){
            return contains(node.right,e);
        }else{
            return true;
        }
    }

    // 看二分搜索树中是否包含元素 e
    public boolean contains(E e){
        return contains(root, e);
    }
```

## 插入

当向树中插入一个新的节点时，该节点将总是作为叶子节点。所以，最困难的地方就是如何找到该节点的父节点。我们将需要插入的节点称为节点 n，
而遍历的当前节点称为节点 c。开始时，节点 c 为 BST 的根节点。则定位节点 n 父节点的步骤如下：

1. 如果节点 c 为空，则节点 c 的父节点将作为节点 n 的父节点。如果节点 n 的值小于该父节点的值，则节点 n 将作为该父节点的左孩子;
   否则节点 n 将作为该父节点的右孩子。

2. 比较节点 c 与节点 n 的值。

3. 如果节点 c 的值与节点 n 的值相等，则说明用户在试图插入一个重复的节点。解决办法可以是直接丢弃节点 n，或者可以抛出异常。

4. 如果节点 n 的值小于节点 c 的值，则说明节点 n 一定是在节点 c 的左子树中。则将父节点设置为节点 c 的左孩子，然后返回至第 1 步。

5. 如果节点 n 的值大于节点 c 的值，则说明节点 n 一定是在节点 c 的右子树中。则将父节点设置为节点 c 的右孩子，然后返回至第 1 步。

当合适的节点找到时，该算法结束。从而使新节点被放入 BST 中成为某一父节点合适的孩子节点。

具体代码实现如下：

```
    // 向以 node 为根的二分搜索树中插入元素 e，递归算法
    // 返回插入新节点后二分搜索树的根
    private Node add(Node node, E e){
        if(node == null){
            size++;
            return new Node(e);
        }
        if(e.compareTo(node.e) < 0){
            node.left = add(node.left,e);
        }else if(e.compareTo(node.e) > 0){
            node.right = add(node.right,e);
        }
        return node;
    }

   // 向二分搜索树中添加新的元素e
    public void add(E e){
        root = add(root, e);
    }
```

随机插入形成树的动画如下，可以看到，插入的时候树还是能够保持近似平衡状态：

![inset_BST_demo](https://github.com/xianfeng92/Fuck-Funny-DataStructure-Algorithms/blob/master/images/inset_BST_demo.gif)


__BST 的插入算法的复杂度与查找算法的复杂度是一样的：最佳情况是 O(log­2n)，而最坏情况是 O(n)。因为它们对节点的查找定位策略是相同的。__

## 删除

从 BST 中删除节点比插入节点难度更大。因为删除一个非叶子节点，就必须选择其他节点来填补因删除节点所造成的树的断裂。如果不选择节点来填补这个断裂，
那么就违背了 BST 的性质要求。

删除节点算法的第一步是定位要被删除的节点，可以使用 BST 的查找算法，因此运行时间为 O(lg­n),接着应该选择合适的节点来代替被删除节点的位置，
它共有三种情况需要考虑:

1. 如果删除的节点没有右孩子，那么就选择它的左孩子来代替原来的节点。

2. 如果被删除节点的右孩子没有左孩子，那么这个右孩子被用来替换被删除节点。

3. 如果被删除节点的右孩子有左孩子，就需要用被删除节点右孩子的左子树中的最下面的节点来替换它，就是说，我们用被删除节点的右子树中最小值的节点来替换。

![BST_Delete](https://github.com/xianfeng92/Fuck-Funny-DataStructure-Algorithms/blob/master/images/BST_Delete.gif)

以上二叉查找树的删除节点的算法不是完美的，因为随着删除的进行，二叉树会变得不太平衡，下面是动画演示。

![BST_Delete_Demo](https://github.com/xianfeng92/Fuck-Funny-DataStructure-Algorithms/blob/master/images/BST_Delete_Demo.gif)

和查找、插入算法类似，__删除算法的运行时间也与 BST 的拓扑结构有关，最佳情况是 O(lg­n)，而最坏情况是 O(n)。__

## 遍历节点

对于线性的连续的数组来说，遍历数组采用的是单向的迭代法。从第一个元素开始，依次向后迭代每个元素。而 BST 则有三种常用的遍历方式：

* 前序遍历（Perorder traversal）

* 中序遍历（Inorder traversal）

* 后序遍历（Postorder traversal）

这三种遍历方式都是从根节点开始，然后访问其子节点。区别在于遍历时，访问节点本身和其子节点的顺序不同。

![traversal_BST](https://github.com/xianfeng92/Fuck-Funny-DataStructure-Algorithms/blob/master/images/traversal_BST.gif)

### 前序遍历（Perorder traversal）

前序遍历从当前节点（节点 c）开始访问，然后访问其左孩子，再访问右孩子。开始时，节点 c 为 BST 的根节点。算法如下：

1. 访问节点 c

2. 对节点 c 的左孩子重复第 1 步

3. 对节点 c 的右孩子重复第 1 步

则上图中树的遍历结果为：90, 50, 20, 5, 25, 75, 66, 80, 150, 95, 92, 111, 175, 166, 200。

### 中序遍历（Inorder traversal）

中序遍历是从当前节点（节点 c）的左孩子开始访问，再访问当前节点，最后是其右节点。开始时，节点 c 为 BST 的根节点。算法如下：

* 访问节点 c 的左孩子

* 对节点 c 重复第 1 步

* 对节点 c 的右孩子重复第 1 步

则上图中树的遍历结果为：5, 20, 25, 50, 66, 75, 80, 90, 92, 95, 111, 150, 166, 175, 200。

### 后序遍历（Postorder traversal）

后序遍历首先从当前节点（节点 c）的左孩子开始访问，然后是右孩子，最后才是当前节点本身。开始时，节点 c 为 BST 的根节点。算法如下：

* 访问节点 c 的左孩子

* 对节点 c 的右孩子重复第1 步

* 对节点 c 重复第 1 步

则上图中树的遍历结果为：5, 25, 20, 66, 80, 75, 50, 92, 111, 95, 166, 200, 175, 150, 90。

# 小结

二叉查找树的运行时间和树的形状有关，树的形状又和插入元素的顺序有关。在最好的情况下，节点完全平衡，从根节点到最底层叶子节点只有lgn个节点。
在最差的情况下，根节点到最底层叶子节点会有 n 个节点。在一般情况下，树的形状和最好的情况接近。它和二分查找一样，插入和查找的时间复杂度均为lgn，但是在最坏的情况下仍然会有 O(n) 的时间复杂度。原因在于插入和删除元素的时候，树没有保持平衡。我们追求的是在最坏的情况下仍然有较好的时间复杂度，这就需要使用平衡查找树，如:2-3查找树。
