##  数据结构

### 定义

数据结构是指相互之间存在着一种或多种关系的数据元素的集合和该集合中数据元素之间的关系组成

### 分类

数据结构按照逻辑结构可分为：

1. 线性结构

   数据结构中的元素存在一对一的相互关系

2. 树结构

   数据结构中的元素存在一对多的相互关系

3. 图结构

   数据结构中的元素存在多对多的相互关系

### 组成结构

#### 线性表

1. 顺序表
2. 链表

### 顺序表

查询的时间复杂度是O(n)

#### Python中列表和顺序表的不同？

1. 列表可以无限的延长，线性表不行，线性表在开辟的时候必须指定长度
2. 列表里面可以存贮不同的元素，线性表只能存贮相同类型的元素

#### 延伸内存知识

内存是一个一个的小格子，每个格子的大小是1B(一个字节)。格子的编号从0开始到最后，现在不使用32位的原因就是32位电脑的寻址能力差，32位计算机在CPU里存一个数存32位

~~~python
# 32bit的机器CPU最大的寻址能力是 0 - 4294967295
print(2 ** 32 - 1)

# 所以支持的最大内存是4294967296 - > 4 G
print(4294967296 / 1024 / 1024 / 1024)
# 所以32bit最大的是4G内存 增加内存条也不管用
~~~

#### Python中的列表使用的是动态表

动态列表先开，向内存索要4个字节的内存，先要空间，里面什么也没有。然后`append`向列表中增加元素。但是当列表满的时候，解决的办法是重新的向内存索要一块内存，是原来内存的一倍(8个字节)前面的数据复制下来，后面还可以支持4次`append`。使用摊换分析后的时间复杂度是O(1)，Python对列表实现了动态扩张。Python的列表是通过存贮地址来解决，存取整数的时候开辟的地址是`100-103`，列表开辟的内存是`1000-1003`来存取`100-103`的==内存地址==，存取的数据是小数开辟的地址是`200-207`，列表开辟的内存是`1004-1007`来存取`200-207`的==内存地址==

### 栈(Stack)LIFO后进先出

#### 对应的是深度优先遍历(DFS)

是一个数据的集合，可以理解为只能在一端进行插入和删除操作的列表

~~~bash
# n个数入栈一共有多少出栈的方式？
# 1 1 2 5 14 42卡特兰数
~~~

#### 栈的应用——括号匹配的问题

问题描述，给一个字符串，其中包括小括号，中括号，大括号，求该字符串的括号是否匹配

`()()[]{}` 匹配

`([{()}])` 匹配

`[](` 不匹配

`[(])` 不匹配 

~~~Python
def brace_match(string):
    stack = []
    dic = {'[': ']', '{': '}', '(': ')'}
    for ch in string:
        if ch in {'[', '{', '('}:
            stack.append(ch)
        elif ch in {']', '}', ')'}:
            if len(stack) == 0:
                return False
            elif dic[stack[-1]] == ch:
                stack.pop()  # 匹配出栈
            else:  # 不匹配 直接返回False
                return False
        else:
            pass
    if len(stack) > 0:
        return False
    else:
        return True

print(brace_match('{}['))
~~~

### 队列(Queue)FIFO先进先出

#### 对应的是广度优先遍历(BFS)

~~~python
# 使用Python实现linux的tail命令

# 获取文件的后五行内容

from collections import deque

ret = list(deque(open('./1.txt', mode='r', encoding='utf-8'), 5))
print(ret)

result = deque([1, 2, 3, 4], maxlen=2)  # 参数一 iterable可迭代对象 参数二maxlen 最大的行数  超过行数的丢弃
print(list(result))
~~~

#### 如何实现两个栈完成队列

1. `stack1` 进行栈的进去 1 2 3，然后出栈，`stack2`进行入栈

2. `stack2` 进行栈的进入 3 2 1，然后在进行的出栈 1 2 3，栈2空了就从栈1获取元素

   ~~~python
   
   
   class Queue(object):
   
       def __init__(self):
           self.stack1 = []
           self.stack2 = []
   
       def push(self, val):
           self.stack1.append(val)
   
       def pop(self):
           if len(self.stack2) > 0:
               return self.stack2.pop()
           elif len(self.stack1) > 0:
               while len(self.stack1) > 0:
                   self.stack2.append(self.stack1.pop())
               return self.stack2.pop()
           else:
               raise Exception('Empty queue')
   
   q = Queue()
   q.push(1)
   q.push(2)
   q.push(3)
   print(q.pop())
   q.push(4)
   print(q.pop())
   print(q.pop())
   print(q.pop())
   ~~~

### 链表

#### 定义

链表中的每一个元素都是一个对象，每个对象成为一个节点，包含有数据域`key`和指向下一个节点指针`next`。通过各个节点之间的相互连接，最终串联成一个链表。

#### 手动的创建链表

~~~python
class Node(object):

    def __init__(self, data):
        self.data = data
        self.next = None

a = Node(1)
b = Node(2)
c = Node(3)
d = Node(4)
a.next = b
b.next = c
c.next = d
~~~

#### 头插法创建链表

~~~python
# 头插法
def create_linklist(li):  # 创建链表
    head = Node()
    for val in li:
        p = Node(val)
        p.next = head.next
        head.next = p
    return head
~~~

#### 尾插法创建链表

~~~python
# 尾插法
def create_linklist_tail(li):
    head = Node()
    tail = head
    for val in li:
        p = Node(val)
        tail.next = p
        tail = p
    return head
~~~

#### 遍历链表

~~~python
def traverse_linklist(head):  # 遍历链表
    p = head.next
    while p:
        yield p.data
        p = p.next
~~~

#### 链表节点的插入

~~~python
# 链表节点的插入
p.next = curNode.next
curNode.next = p
~~~

#### 链表节点的删除

~~~python
# 链表节点的删除
# 删除的是curNode后面的节点
p = curNode.next
curNode.next = p.next  # 等同于 curNode.next = curNode.next.next
del p
~~~

#### 创建双向链表

~~~python
class Dnode(object):

    def __init__(self, data=None):
        self.data = data
        self.next = None
        self.prior = None
~~~

#### 尾插法

~~~python
def create_linklist_tail(li):
    head = Dnode()
    tail = head
    for val in li:
        p = Dnode(val)
        tail.next = p
        p.prior = tail
        tail = p
    return head, tail
~~~

#### 倒着遍历

~~~python
def traverse_linklist_back(tail):  # 倒着遍历链表
    p = tail
    while p.prior:
        yield p.data
        p = p.prior
~~~

#### 双向链表的插入

~~~python
p.next = curNode.next
curNode.next.prior = p
p.prior = curNode
curNode.next = p
~~~

#### 双向链表的删除

~~~python
p = curNode.next
curNode.next = p.next
p.next.prior = curNode
del p
~~~

### 链表和顺序表的区别

| 时间复杂度         | 顺序表 | 链表 |
| ------------------ | ------ | ---- |
| 按元素查找         | O(n)   | O(n) |
| 按下标查找         | O(1)   | O(n) |
| 在某元素的后面插入 | O(n)   | O(1) |
| 删除某元素         | O(n)   | O(1) |

1. 链表在插入和删除的操作上明显库快于顺序表
2. 链表的内存可以更加灵活的分配

文件系统的使用的是链表系统

磁盘的存贮的单位都是块，一个块是4K 

### 哈希表

解决哈希冲突的问题

1. 开放寻址法
   1. 自己设置一个装载因子，当哈希表达到装载因子的时候，动态扩张 
   2. 线性探查，如果i的位置被占用，探查i+1，i+2， 但是会导致大量的数和大量的空格堆积在一起
   3. 二次探查，如果i的位置被占用，探查i+1^2，i-1^2
   4. 二度哈希
2. 拉链法(哈希分筒)

哈希表在Python中的应用

字典和集合都是通过哈希表实现的

### 二叉树

~~~~python
class BiTreeNode(object):

    def __init__(self, data=None):
        self.data = data
        self.lchild = None
        self.rchild = None


a = BiTreeNode('A')
b = BiTreeNode('B')
c = BiTreeNode('C')
d = BiTreeNode('D')
e = BiTreeNode('E')
f = BiTreeNode('F')
g = BiTreeNode('G')

e.lchild = a
e.rchild = g
a.rchild = c
c.lchild = b
c.rchild = d
g.rchild = f

root = e


# 二叉树的遍历
#  深度优先遍历--前序遍历

def pre_order(root):
    if root:
        print(root.data, end='')
        pre_order(root.lchild)
        pre_order(root.rchild)


pre_order(root)
print()


#  深度优先遍历--中序遍历
def in_order(root):
    if root:
        in_order(root.lchild)
        print(root.data, end='')
        in_order(root.rchild)


in_order(root)


# 深度优先遍历--后序遍历
def post_order(root):
    if root:
        post_order(root.lchild)
        post_order(root.rchild)
        print(root.data, end='')
print()
post_order(root)
print()
# 广度优先遍历--层次遍历
from collections import deque
def level_order(root):
    q = deque()
    q.append(root)
    while len(q) > 0:
        n = q.popleft()
        print(n.data, end='')
        if n.lchild:
            q.append(n.lchild)
        if n.rchild:
            q.append(n.rchild)

level_order(root)

# 前序定根 中序分左右
~~~~



