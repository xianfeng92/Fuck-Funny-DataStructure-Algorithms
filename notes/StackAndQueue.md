# 栈和队列

栈 (Stack)是一种后进先出(last in first off，LIFO)的数据结构，而队列(Queue)则是一种先进先出 (fisrt in first out，FIFO)的结构，
如下图所示：

![stackAndQueue]()

# 实现

## 栈的实现

栈是一种后进先出的数据结构，对外主要提供如下几个 API：

```
interface BaseStack<E> {

    int getSize();
    boolean isEmpty();
    void push (E e);
    E pop();
    E peek();
}
```

这些 API 的有两种实现方案：数组和链表。

### 栈的链表实现

首先定义一个内部类来保存每个链表的节点，该节点包括当前节点的值以及指向下一个节点的引用值，然后建立一个节点保存位于栈顶的值以及记录栈的元素个数。

```
 class Node{
        public E item;
        public Node next;
    }

    private Node first = null; // 记录栈顶节点的引用
    private int size = 0; // 记录栈中的节点个数
```

Push方法，即向栈顶压入一个元素，首先保存原先的位于栈顶的元素，然后新建一个新的栈顶元素，然后将该元素的下一个指向原先的栈顶元素。整个过程如下所示：

![Stack_push]()

实现代码如所示：

```
    public void push(E e) {
        Node oldFirst = first; // 原先的位于栈顶的节点
        Node node = new Node(); // 新建一个节点
        node.item = e;
        node.next = oldFirst;
        first = node; // 更新栈顶节点的引用
        size++; // 更新栈中节点数
    }
```


Pop方法也很简单，首先保存栈顶元素的值，然后将栈顶元素设置为下一个元素：

![Stack_pop]()

实现代码如所示:

```
   public E pop() {
        if (isEmpty()){// 当前栈为空时，直接返回 null
            return null;
        }
        E item = first.item;// 获取栈顶的节点值
        first = first.next; // 更新栈顶
        size--; // 更新栈中节点个数
        return item;
    }
```






































