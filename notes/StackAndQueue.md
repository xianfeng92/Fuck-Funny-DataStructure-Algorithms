# 栈和队列

栈 (Stack)是一种后进先出(last in first out，LIFO)的数据结构，而队列(Queue)则是一种先进先出 (fisrt in first out，FIFO)的结构，
如下图所示：

![stackAndQueue]()

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

这些 API 有两种实现方案：数组和链表。

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

Push方法，即向栈顶压入一个元素，首先保存原先的位于栈顶的元素，然后新建一个新的栈顶元素，最后将该元素的下一个指向原先的栈顶元素。整个过程如下所示：

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

基于链表的Stack实现，在最坏的情况下只需要常量的时间来进行Push和Pop操作。

### 栈的数组实现

使用数组来存储栈中的元素 Push 的时候，直接添加一个元素到S[N]，Pop 的时候直接返回S[N-1]。

![StackImpByArray]()


#### push 方法实现

首先，我们定义一个数组，然后在构造函数中给定初始化大小，Push方法实现如下，就是集合里添加一个元素：

```
 private E[] item;
    private int size = 0; // 栈中元素个数

    public StackbyArray(int Capacity){
        item = (E[]) new Object[Capacity];// Capacity 栈的初始化大小
    }

    @Override
    public void push(E e) {
        if(size == item.length){
            Resize(2*item.length); // 对栈进行扩容
        }
        item[size++] = e;
    }
```

#### Pop方法实现

```
    public E pop() {
        E temp = item[--size];
        item[size] = null;
        if(size > 0 && size == item.length / 4){
            Resize(item.length / 2);
        }
        return temp;
    }
```

在 Push 和 Pop 方法中，为了节省内存空间，需要对数组进行整理。Push 的时候，当元素的个数达到数组的 Capacity 的时候，对原数组扩容 2 倍。
Pop 的时候，当元素的个数小于当前容量的 1/4 的时候，我们将原数组的大小容量减少 1/2。采用的是判断 1/4 的情况，这样效率要比 1/2 要高，
因为可以有效避免在 1/2 附件插入，删除，从而频繁的扩大和缩小数组的情况。

#### Resize 方法实现

```
    private void Resize(int Capacity){
        E[] temp = (E[]) new Object[Capacity];
        System.arraycopy(item,0,temp,0,item.length); // 数组复制
        item = temp;
    }
```
Resize 方法中主要涉及的是数组中元素的拷贝操作。

#### 栈的数组实现的特性

1. Pop 和Push 操作在最坏的情况下与元素个数成比例的 N 的时间，时间主要花在数组的扩容或缩小时元素的拷贝上。

2. 元素在内存中分布紧凑，密度高，便于利用内存的时间和空间局部性，便于CPU进行缓存，较链表实现内存占用小，效率高。


## 队列的实现

Queue 是一种先进先出的数据结构，对外主要提供如下几个 API：

```
public interface BaseQueue<E> {
    void Enqueue(E e);
    E Dequeue();
    boolean isEmpty();
    int size();
}
```

### 基于链表的实现

Dequeue 方法就是返回链表中的第一个元素：

```
    public E Dequeue() {
        if (isEmpty()){
            return null;
        }
        E temp = head.item;
        head = head.next;
        size--;
        if(isEmpty()){
            tail = null;
        }
        return temp;
    }
```

Enqueue 是在链表的末尾增加新的元素:

```
    public void Enqueue(E e) {
        Node oldTail = tail;
        tail = new Node();
        tail.item = e;
        if(isEmpty()){
            head = tail;
        }else {
            oldTail.next = tail;
        }
        size++;
    }
```

## 栈和队列的应用

1. Stack 的应用

Stack这种数据结构用途很广泛，比如编译器中的词法分析器、Java虚拟机、软件中的撤销操作、浏览器中的回退操作，编译器中的函数调用实现等等。
比如，可以利用 stack 处理多余无效的请求，比如用户长按键盘，或者在很短的时间内连续按某一个功能键，我们需要过滤到这些无效的请求。一个
通常的做法是将所有的请求都压入到堆中，然后要处理的时候 Pop 出来一个，这个就是最新的一次请求。

2. Queue 的应用

在现实生活中 Queue 的应用也很广泛，最广泛的就是排队了，”先来后到” First come first service ，以及 Queue 这个单词就有排队的意思。
比如我们的播放器上的播放列表，我们的数据流对象，异步的数据传输结构(文件IO，管道通讯，套接字等)。还有一些解决对共享资源的冲突访问，
比如打印机的打印队列等。消息队列等。交通状况模拟，呼叫中心用户等待的时间的模拟等等。

