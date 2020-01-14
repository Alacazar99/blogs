### 一、链表

```
class Node(object):
    def __init__(self, value):
        # 元素域
        self.value = value
        # 链接域
        self.next = None


class LinkedListOneway(object):
    def __init__(self, node=None):
        self.__head = node

    def __len__(self):
        # 游标，用来遍历链表
        cur = self.__head
        # 记录遍历次数
        count = 0
        # 当前节点为None则说明已经遍历完毕
        while cur:
            count += 1
            cur = cur.next
        return count

    def is_empty(self):
        # 头节点不为None则不为空
        return self.__head == None

    def add(self, value):
        """
        头插法
        先让新节点的next指向头节点
        再将头节点替换为新节点
        顺序不可错，要先保证原链表的链不断，否则头节点后面的链会丢失
        """
        node = Node(value)
        node.next = self.__head
        self.__head = node

    def append(self, value):
        """尾插法"""
        node = Node(value)
        cur = self.__head
        if self.is_empty():
            self.__head = node
        else:
            while cur.next:
                cur = cur.next
            cur.next = node

    def insert(self, pos, value):
        # 应对特殊情况
        if pos <= 0:
            self.add(value)
        elif pos > len(self) - 1:
            self.append(value)
        else:
            node = Node(value)
            prior = self.__head
            count = 0
            # 在插入位置的前一个节点停下
            while count < (pos - 1):
                prior = prior.next
                count += 1
            # 先将插入节点与节点后的节点连接，防止链表断掉，先链接后面的，再链接前面的
            node.next = prior.next
            prior.next = node

    def remove(self, value):
        cur = self.__head
        prior = None
        while cur:
            if value == cur.value:
                # 判断此节点是否是头节点
                if cur == self.__head:
                    self.__head = cur.next
                else:
                    prior.next = cur.next
                break
            # 还没找到节点，有继续遍历
            else:
                prior = cur
                cur = cur.next

    def search(self, value):
        cur = self.__head
        while cur:
            if value == cur.value:
                return True
            cur = cur.next
        return False

    def traverse(self):
        cur = self.__head
        while cur:
            print(cur.value)
            cur = cur.next
```

---

### 二、队列

```
class  Queue:
    #初始化队列
    def __init__(self):
        self.items=[]

    #加入队列
    def inqueue(self,item):
        self.items.append(item)
    #出队列
    def dequeue(self):
        return self.items.pop(0)
        
    #判断是否为空
    def empty(self):
        return self.size()==0
    #返回长度
    def size(self):
        return len(self.items)
```
##### 附个实例：约瑟夫斯问题
```
#约瑟夫斯问题
def josephus(namelist, num):
    simqueue = Queue()
    for name in namelist:
        simqueue.inqueue(name)

    while simqueue.size() > 1:
        for i in range(num):
            simqueue.inqueue(simqueue.dequeue())
        simqueue.dequeue()
    return simqueue.dequeue()

def josephus(namelist,num):
    squeue = Queue()
    for name in namelist:
        squeue.inqueue(name)

    while squeue.size()>1:
        for i in range(num):
            squeue.inqueue(squeue.dequeue())
        squeue.dequeue()
    return squeue.dequeue()

if __name__ == '__main__':
    print(josephus(["Java", "Python", "C++", "C", "PHP", "Html5"], 5))

```

---

### 三、堆栈

```
# 后进先出
class Stack():
    def __init__(self,size):
        self.size=size
        self.stack=[]
        self.top = -1
    # 压入数据
    def push(self,x):
        if self.isfull():
            raise eecception("栈满...")
        else:
            self.stack.append(x)
            self.top = self.top + 1

    # 弹出数据
    def pop(self):
        if self.isempty():
            raise exception("栈空...")
        else:
            self.top = self.top -1
            self.stack.pop()

    def isfull(self):
        return self.top+1 == self.size

    def isempty(self):
        return self.top == '-1'
        
    def showStack(self):
        print(self.stack)
 
if __name__ == "__main__":
    s=Stack(10)
    for i in range(10):
        s.push(i)
    s.showStack()
    for i in range(3):
        s.pop()
    s.showStack()
 
"""
类中有top属性，用来指示栈的存储情况，初始值为1，一旦插入一个元素，其值加1，利用top的值乐意判定栈是空还是满。
执行时先将0,1,2,3,4依次入栈，然后删除栈顶的前三个元素
"""
```

---

### 四、AVL树（平衡二叉树）
详细关于AVL树请阅读：[浅谈【树】：红黑树、AVL树、B树、B+树](https://www.jianshu.com/p/5f7f89451b58)
```
class Node(object):
    def __init__(self, key, parent=None, left=None, right=None):
        self.key = key          # 存储的数据
        self.parent = parent    # 父节点
        self.left = left        # 左孩子节点
        self.right = right      # 右孩子节点
        self.bf = 0             # 平衡因子 等于左子树高度减去右子树高度

    def l_rotate(self, par, cur):
        """
        左单旋转
        :param par:  平衡因子为2|-2的节点
        :param cur:  作为轴心旋转的节点(平衡因子不为2)
        :return: None
        """
        p = cur.left                    # 辅助引用p指向cur的左节点
        cur.left = par                  # cur的左节点指向par
        par.right = p                   # par的右节点指向cur的左节点
        if p:
            p.parent = par              # cur的左节点不为空时更新该节点的父节点
        if par == self.root:
            self.root = cur             # 判断平衡因子为2的par是否为根
        else:
            if par == par.parent.left:  # 更新par父节点的子节点信息
                par.parent.left = cur
            else:
                par.parent.right = cur
        cur.parent = par.parent         # 当前cur的父节点指向原来par的父节点
        par.parent = cur                # par变为cur的左子节点
        cur.bf = par.bf = 0             # 插入操作中, 操作的两个节点旋转后平衡因子恢复为0

    def r_rotate(self, par, cur):
        """右单旋转"""
        p = cur.right
        cur.right = par
        par.left = p
        if p:
            p.parent = par
        if par == self.root:
            self.root = cur
        else:
            if par == par.parent.left:
                par.parent.left = cur
            else:
                par.parent.right = cur

        cur.parent = par.parent         # 更新父节点信息
        par.parent = cur
        cur.bf = par.bf = 0             # 更新平衡因子

    def lr_rotate(self, par, cur):
        """左右双旋转"""
        # 先左旋转 cur 和 cur的右子节点
        cur_right = cur.right           # 获得cur的右子节点的引用
        bf = cur_right.bf
        self.l_rotate(cur, cur_right)
        # 继续右旋转
        self.r_rotate(par, cur_right)
        if bf == 0:                     # 根据cur_right的平衡因子更新操作的3个节点
            cur.bf = cur_right.bf = par.bf = 0
        elif bf == -1:
            par.bf = 1
            cur.bf = cur_right.bf = 0
        else:
            cur.bf = -1
            cur_right.bf = par.bf = 0
            
    def rl_rotate(self, par, cur):
        """右左双旋转"""
        # 先右旋转 cur 和 cur的左子节点
        cur_left = cur.left                         # 获得cur的右子节点的引用
        bf = cur_left.bf
        self.r_rotate(cur, cur_left)
        self.l_rotate(par, cur_left)                # 继续右旋转       
        if bf == 0:                                 # 根据cur_left的平衡因子更新操作的3个节点
            cur.bf = cur_left.bf = par.bf = 0
        elif bf == -1:
            cur.bf = 1
            par.bf = cur_left.bf = 0
        else:
            par.bf = -1
            cur_left.bf = cur.bf = 0
            
    def find(self, key):
        """寻找键值"""
        p = self.root
        while p:
            if p.key == key:
                return p
            if key < p.key:
                p = p.left
            else:
                p = p.right
        return None
```

---

### 五、红黑树
详细关于红黑树请阅读：[浅谈【树】：红黑树、AVL树、B树、B+树](https://www.jianshu.com/p/5f7f89451b58)

```
#定义红黑树
class RBTree(object):
    def __init__(self):
        self.nil = RBTreeNode(0)
        self.root = self.nil

class RBTreeNode(object):
    def __init__(self, x):
        self.key = x
        self.left = None
        self.right = None
        self.parent = None
        self.color = 'black'
        self.size=None

#左旋转
def LeftRotate( T, x):
    y = x.right
    x.right = y.left
    if y.left != T.nil:
        y.left.parent = x
    y.parent = x.parent
    if x.parent == T.nil:
        T.root = y
    elif x == x.parent.left:
        x.parent.left = y
    else:
        x.parent.right = y
    y.left = x
    x.parent = y

#右旋转
def RightRotate( T, x):
    y = x.left
    x.left = y.right
    if y.right != T.nil:
        y.right.parent = x
    y.parent = x.parent
    if x.parent == T.nil:
        T.root = y
    elif x == x.parent.right:
        x.parent.right = y
    else:
        x.parent.left = y
    y.right = x
    x.parent = y

#红黑树的插入
def RBInsert( T, z):
    y = T.nil
    x = T.root
    while x != T.nil:
        y = x
        if z.key < x.key:
            x = x.left
        else:
            x = x.right
    z.parent = y
    if y == T.nil:
        T.root = z
    elif z.key < y.key:
        y.left = z
    else:
        y.right = z
    z.left = T.nil
    z.right = T.nil
    z.color = 'red'
    RBInsertFixup(T, z)
    return z.key, '颜色为', z.color


#红黑树的上色
def RBInsertFixup( T, z):
    while z.parent.color == 'red':
        if z.parent == z.parent.parent.left:
            y = z.parent.parent.right
            if y.color == 'red':
                z.parent.color = 'black'
                y.color = 'black'
                z.parent.parent.color = 'red'
                z = z.parent.parent
            else:
                if z == z.parent.right:
                    z = z.parent
                    LeftRotate(T, z)
                z.parent.color = 'black'
                z.parent.parent.color = 'red'
                RightRotate(T,z.parent.parent)
        else:
            y = z.parent.parent.left
            if y.color == 'red':
                z.parent.color = 'black'
                y.color = 'black'
                z.parent.parent.color = 'red'
                z = z.parent.parent
            else:
                if z == z.parent.left:
                    z = z.parent
                    RightRotate(T, z)
                z.parent.color = 'black'
                z.parent.parent.color = 'red'
                LeftRotate(T, z.parent.parent)
    T.root.color = 'black'


def RBTransplant( T, u, v):
    if u.parent == T.nil:
        T.root = v
    elif u == u.parent.left:
        u.parent.left = v
    else:
        u.parent.right = v
    v.parent = u.parent


def RBDelete(T, z):
    y = z
    y_original_color = y.color
    if z.left == T.nil:
        x = z.right
        RBTransplant(T, z, z.right)
    elif z.right == T.nil:
        x = z.left
        RBTransplant(T, z, z.left)
    else:
        y = TreeMinimum(z.right)
        y_original_color = y.color
        x = y.right
        if y.parent == z:
            x.parent = y
        else:
            RBTransplant(T, y, y.right)
            y.right = z.right
            y.right.parent = y
        RBTransplant(T, z, y)
        y.left = z.left
        y.left.parent = y
        y.color = z.color
    if y_original_color == 'black':
        RBDeleteFixup(T, x)


#红黑树的删除
def RBDeleteFixup( T, x):
    while x != T.root and x.color == 'black':
        if x == x.parent.left:
            w = x.parent.right
            if w.color == 'red':
                w.color = 'black'
                x.parent.color = 'red'
                LeftRotate(T, x.parent)
                w = x.parent.right
            if w.left.color == 'black' and w.right.color == 'black':
                w.color = 'red'
                x = x.parent
            else:
                if w.right.color == 'black':
                    w.left.color = 'black'
                    w.color = 'red'
                    RightRotate(T, w)
                    w = x.parent.right
                w.color = x.parent.color
                x.parent.color = 'black'
                w.right.color = 'black'
                LeftRotate(T, x.parent)
                x = T.root
        else:
            w = x.parent.left
            if w.color == 'red':
                w.color = 'black'
                x.parent.color = 'red'
                RightRotate(T, x.parent)
                w = x.parent.left
            if w.right.color == 'black' and w.left.color == 'black':
                w.color = 'red'
                x = x.parent
            else:
                if w.left.color == 'black':
                    w.right.color = 'black'
                    w.color = 'red'
                    LeftRotate(T, w)
                    w = x.parent.left
                w.color = x.parent.color
                x.parent.color = 'black'
                w.left.color = 'black'
                RightRotate(T, x.parent)
                x = T.root
    x.color = 'black'


def TreeMinimum( x):
    while x.left != T.nil:
        x = x.left
    return x

    
#中序遍历
def Midsort(x):
    if x!= None:
        Midsort(x.left)
        if x.key!=0:
            print('key:', x.key,'x.parent',x.parent.key)
        Midsort(x.right)

if __name__ == "__main__":   
    nodes = [11,2,14,1,7,15,5,8,4]
    T = RBTree()
    for node in nodes:
        print('插入数据',RBInsert(T,RBTreeNode(node)))
    print('中序遍历')
    Midsort(T.root)
    RBDelete(T,T.root)
    print('中序遍历')
    Midsort(T.root)
    RBDelete(T,T.root)
    print('中序遍历')
    Midsort(T.root)

```

---
