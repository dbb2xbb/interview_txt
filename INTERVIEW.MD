[TOC]
# Python面试题
## python语法以及其他基础
### 可变与不可变类型:
> - 可变类型：包括列表，字典，集合。即在处于相同内存地址的前提下，该地址中存放的内容可写。
> ```
以下对a的操作中，是在内存地址3054238252中先后两次修改了a的值，这种在同一个地址中可以修改内存内容的变量类型为可变类型。
a = [1]
id(a)
3054238252
a.append(2)
a
[1, 2]
id(a)
3054238252
> ```
> - 不可变类型： 包括int,float,str,tuple。即相同的值都由相同的内存地址来存储，值本身是不会改变的，对于值的改变都会体现在所python引用的地址的不同。例如：
> ```
对i的递增，本质上是指向不同的对象，存于不同的内存地址。而就对象本身而言，其值是无法更改的，这就是不可变对象的含义。
id(i)
139402144 
i += 1
id(i)
139402160
> ```

### 浅拷贝与深拷贝的实现方式、区别；deepcopy如果你来设计，如何实现； 
> - 复制：在python中的对象赋值本质上是`对象的应用`,即当一个对象创建后并复制给另一个对象时，其实是在拷贝这个对象的引用，并非是对象本身。
> ```
> 以下实例证明了复制本身就是地址的赋值（可类比为指针间的赋值），当被赋值对象b的内容改变了之后，a也会随之更改。
a = [1,2,3,['a','b']]
id(a)
3070107372
b = a
id(b)
3070107372
b.append(6)
a
[1, 2, 3, ['a', 'b'], 6]
b
[1, 2, 3, ['a', 'b'], 6]
> ```
> - 深拷贝：完全独立于原始对象，对象的地址和内中子对象（若有）的地址均为全新地址，可以说除了值相同以外，其余皆为原始对象的副本。
> ```
> 以下为深拷贝的实例:可以看出，当修改了变量a的深拷贝副本c后，无论如何修改c中的值（主对象和子对象的值）a的值均不会受其影响，
import copy
c = copy.deepcopy(a)
c
[1, 2, 3, ['a', 'b'], 6]
a.append(7)
c
[1, 2, 3, ['a', 'b'], 6]
a[3].append('c')
a
[1, 2, 3, ['a', 'b', 'c'], 6, 7]
c
[1, 2, 3, ['a', 'b'], 6]
> ```
> - 浅拷贝：使用全新地址去创建原始对象的副本，但对象其中的子对象依旧为原始对向中子对象地址的拷贝，即依旧指向原始对象中的子元素，若对其修改，原始对象亦会有相同修改。
>  ```
>  可以看出当修改了a的浅拷贝副本中的子对象后，a中的对象也跟着
d = copy.copy(a)
d
[1, 2, 3, ['a', 'b', 'c'], 6, 7]
d.append(8)
d
[1, 2, 3, ['a', 'b', 'c'], 6, 7, 8]
a
[1, 2, 3, ['a', 'b', 'c'], 6, 7]
d[3].append('d')
d
[1, 2, 3, ['a', 'b', 'c', 'd'], 6, 7, 8]
a
[1, 2, 3, ['a', 'b', 'c', 'd'], 6, 7]
>  ```

###\__new\__() 与 \__init\__()的区别； 
> - \__init\__方法是在当前实例对象被创建后调用的，然后设置对象属性的一些初始值
> - \__new\__方法是在\__init\__被调用之前调用，是一个用于创建对象实例的一个静态方法，创建的实例对象作为第一个参数传给\__init\__函数。

### 你知道几种设计模式： 
### 编码和解码：
> - 编码(encode)：将其他编码的字符串转换成unicode编码。
> - 解码(decode)：将unicode编码转换成其他编码。
> 亦即任意两种编码之间的转换，在python中都要通过unicode作为媒介。

### 列表推导list comprehension和生成器的优劣：
> - 列表推导：
> `[expr for iter_var in iterable]` 或 `[expr for iter_var in iterable if cond_expr]`
> 首先会迭代iterable中的所有数据，每一次的迭代后，都会将iterable中的内容放在iter_var中，最后在expr中应用、并根据expr的计算规则来生成一个列表。实现过程是将所有的数据一次性加载到内存中。
> 优点：代码可读性高，可重复遍历。
> 缺点：当数据量过大时，一次性会占据大量内存。
> - 生成器表达式：
> `(expr for iter_var in iterable)` 或 `(expr for iter_var in iterable if cond_expr)`
> 使用生成器表达式创建列表时，返回的并不是列表本身，而是一个存有列表元素计算方法的生成器，
> 优点：占用内存上，效率较高。
> 缺点：只能遍历一次。
### 什么是装饰器；如果想在函数之后进行装饰，应该怎么做；
> - 定义(个人理解)：对某个函数a在不改变其内部代码的前提下，将其作为另外一个函数b的一部分，最终执行的时候执行的是函数b，从而在客观上起到对该函数的一种装饰作用的**函数**，即为装饰器。
> -  函数之后修饰装饰器：
> ```
> def hello(func):
    def inner_hello():
        print('hello')
        func()
        print('python')
    return inner_hello
def test_dec():
	    print('happy')
def main():
    new_func = hello(test_dec)
    new_func()
if __name__ == '__main__':
    main()
> ```


### 手写个使用装饰器实现的单例模式:
>```
inslist = {}
def dec_class(A):
    def inner_dec(*args):
        if A not in inslist:
            inslist[A] = A(*args)
        return inslist[A]
    return inner_dec 
&ensp;
dec_class
class A(object):
    ins = None
	def __init__(self):
	    self.sum = 0
 &ensp;
def main():
   i1 = A()
   i1.sum = 1
   i2 = A()
   i2.sum = 2
   print('id1 addr is ', id(i1))
   print('id2 addr is ', id(i2))
&ensp;
if __name__ == '__main__':
    main()
>```
### 使用装饰器的单例和使用其他方法的单例，在后续使用中，有何区别； 
> - 常规方法改变了类本身的内容——重写了new方法。
### 手写正则邮箱地址：
> - `re.match(r'^[a-zA-Z0-9\u4e00-\u9fa5]+@[a-zA-Z0-9_-]+(\.[a-zA-Z0-9_-]+)+$','dbb2xbb@outlook.com.cn')`

介绍下垃圾回收：引用计数/分代回收/孤立引用环； 
> - 相比于C/C++由程序员手动分配并释放内存的机制，python提供了一种自动的资源回收机制，前者的好处是自由度比较高，但若操作不当会引起内存泄露、野指针、栈溢出等问题。后者则比较完美的解决了此类问题。
> - python GC机制：以引用计数为主，以分代回收为辅
> 1. 引用计数：python对当前正在使用的对象保有引用计数，当某个对象不再被引用指向的时候，垃圾收集器就会释放该对象占用的内存空间。
> **那么何时回收？**
> 当分配对象次数-取消分配对象次数大于某个阈值的时候，就会启动垃圾回收机制。
> 2. 分代回收：系统中存活时间越长的对象，原则上就越不可能成为垃圾。基于这个前提下，python解释器会在垃圾回收中减少对这类对象扫描的频率，而减少频率的方式就是引入分代回收的机制。
> **那么如何分代？**
> 所有的对象都会被分为0,1,2三代，当某一次垃圾回收机制完毕后，依旧存活的对象会“升级”进入到下一代对象列表中。每一次垃圾回收机制启动后，都会遍历所有的0代对象，当0代对象经过了一定次数的回收后，再进行一次1代的回收，当1代回收到一定次数后，再对2代进行回收。（<font color=red>其实对于分代回收的了解可以类比到大学时候的查寝。大一的时候由于学校的管制比较严格，会进行高频率的查寝，对于那些能够咬紧牙关忍受住大一时的各种摧残而没主动或被动退学的同学来说，到了大二之后就会享有较为宽松的管理方式，查寝次数可能从原来的一周一次，降为双周一次、一月一次。依次类推，到了大三可能就一季度一次、半年一次，到了大四就不会查了。</font>）
> 
> `gc.get_threshold()`函数返回的(700, 10, 10)就代表在阈值为700的情况下，每进行10次0代清理，就进行1次1代清理；每进行1次1代清理，就进行1次2代清理。
> 孤立引用环：将集合中对象引用的副本进行循环引用排查，若类似于A、B之间内部互相引用，则须从A出发，删除对B的引用;然后找到对象B，再删除其对A的引用。

### 多进程与多线程的区别?CPU密集型适合用什么:
> - **区别**:
>  - 系统资源层面：
> 进程：系统资源**分配**的最小单位，占用空间较大。
> 线程：CPU资源**调度**的最小单位，轻量级进程，占用空间相比于进程要小很多。
>  - 变量访问层面：
> 进程：进程间彼此地址隔离，不能访问全局变量。需要进行通信时须借助中间代理来实现（队列）
> 线程：线程间占用相同的地址空间，可以访问全局变量。需要进行通信时之间访问共享内存即可。
>  - 另外，除了上述在其他语言中也通用的进程线程间的区别外，针对python自身，由于GIL（全局解释器锁）的存在，导致python中的线程与其他语言的线程仍有不同之处：**即在任意时刻下，不管当前存在多少个处理器，有且仅有一个线程被执行**。所以在不借助调用第三方库（例如C语言的动态共享库）并且系统资源量较少的情况下，选择多进程处理并发程序更合适。
> - **CPU密集型适合用哪种方式？**
>  - 因为CPU密集型，顾名思义就是要求计算量很大的操作。应对这种情况的话，很显然只用一个CPU核心是不行的，就要使用多个CPU核心。然而前面提到了在python中，线程由于GIL的缘故导致只能使用单核，故如果选择了在CPU密集型的情况下使用多线程执行操作，那么这段代码很有可能是**线性执行**的，这样的话就与单进程无差别。而多进程则无此桎梏，能够充分的运用多核处理器资源。所以对于CPU密集型的程序来说，要采用**多进程**的方式。而针对I/O密集型的程序来说，GIL会在这个I/O调用之前被释放，以此来允许其他线程在这个线程等待I/O的时候运行，因此此时使用多线程进行各个线程间的切换比较合适。
> - **为什么GIL不会影响到多进程？**
>  - 因为GIL是针对当前进程来说的，同一个进程中的多线程均受控于当前进程的GIL，但多进程的话，则各有各自的GIL，一个进程不会获取到另一个进程的GIL，故无影响。

### 进程通信的方式有几种； 
> - 队列：能实现多个进程间的通信。
> - 管道：实现的是两个进程间的通信。

### 简要概述一下生成器:
> 首先，生成器就是一种迭代器，它具有和迭代器一样的性质——通过调用next()函数来获取下一个元素的功能，所以，<font color=red>在理解生成器之前，先要理解什么是**迭代**</font>
> 
> 迭代，意指可以逐项获取某个元素的集合操作（不严格），例如下方代码所示：
> ```
mylist = [x for x in range(10)]
for item in mylist:
    print(item)
 >```
 > 其中的mylist就是一个***可迭代对象***，而使用`for..in..`的语法叫做一个迭代器。
 > 转而说回生成器，它也是可以迭代的，但**只能读取一次**，因为并非将所有计算的值放在内存中，而是实时根据特定算法算出下一次的迭代值。
 > 下方代码中：
 > ```
 > def createGen():
    mylist = range(10)
    for i in mylist:
        yield i*i
&ensp;
cg = createGen()  
print(cg)
&ensp;
for i in cg: 
    print(i)
 > ```
 > **cg = createGen() 并未真正执行到createGen中的代码，只是返回了一个生成器。**
 > **执行到for i in cg时，每迭代以西都要调用一下createGen中的for部分，而createGen中的for中的内容是接着上一次循环后再执行。**

### 介绍下协程，为何比线程还快：
> 协程，即微线程。包含关系为：进程>线程>协程，系统并不能感受到协程的存在，因此其上下文的切换需要手动执行。
> 提到协程，不能不说——<a href="#简要概述一下生成器">生成器</a>。简而言之，协程是一种**在用户态中可以实现上下文切换的技术**其拥有自己的寄存器上下文及堆栈。每次过程重入时，都会进入上一次调用的状态。
> 协程相较于线程的优势在于：
>  1. 效率高：这主要体现在区别于线程间的切换所需的开销[^脚注1]，由程序自身去控制上下文切换避免了这种情况，因此效率会更高。
>  2. 不加锁：因为协程间的切换完全是根据程序自身的逻辑实现上下文的切换工作，因此不存在类似线程中的锁机制。
> 以下为协程的代表性实例：生产者-消费者问题：
> ```
import time
&ensp;
def consumer():
    r = ''
    while True:
        n = yield r
        if not n:
            return
        print('consumer %s'%n)
        time.sleep(1)
        r = 'OK'
&ensp;
def productor(consumer):
    next(consumer)
    n = 0
    for i in range(1,10):
        n = n+1;
        print('productor product :%d'%n)
        res = consumer.send(i)
        print('consumer eats :%s'%res)
&ensp;
&ensp;
def main():
    c = consumer()
    productor(c)
&ensp;
if __name__ == '__main__':
    main()
> ```

### range和xrange的区别:
> 注意在讨论两者的区别的时候，是在<font color=red>python2.x的前提下，py3中实际是将原有的xrange改成了现有的range</font>首先两者的用法基本相同，不同点在于range返回的直接就是列表元素，而xrange返回的则是一个生成器。**若开发者仍需使用一次性生成全部数据的py2版本的range，可采用`list[range(x)]`来实现**
 
### 用C来手写：将IP地址字符串（比如“172.0.0.1”）转为32位二进制数的函数:
> ```
> 
\#include <stdio.h>
\#include <string.h>
&ensp;
int t2b(int a)
{
    char tmp[32] = {0};
    int i;
    for(i=0; i<32; i++)
    {
        tmp[32-i-1] = '0' + a%2;
        a /= 2;
        printf("%c",tmp[32-1-i]);
        if (i && ((i+1)%8 == 0)) 
        {
            printf(" ");
        }   
    }   
    printf("\n");
}
&ensp;
int trans_ip(char *ip)
{
    int i=0;
    int j=0;
    int itmp=0;
    int res = 0;
    int cnt = 0;
    for(i=0; i<strlen(ip); i++)
    {
        //11.22.33.44
        if(ip[i] != '.' && ip[i+1] != '\0')
        {
            itmp += (ip[i] - '0');
            itmp *= 10; 
        }
        else
        {
            if(ip[i+1] == '\0')
            {
                itmp += (ip[i] - '0');
                itmp *= 10;
            }
&ensp;
            itmp /= 10;
            res |= itmp << (8*(3-cnt));
            printf(" %d\r\n",res);
            itmp = 0;
            cnt++;
        }
    }
    t2b(res);
    printf("\n\r");
    return 0;
}
&ensp;
&ensp;
int main()
{
    char ip[] = {"15.15.15.15"};
    int ip_int = trans_ip(ip);
    return 0;
}

> ```

## 算法排序部分
### 手写快排；堆排；几种常用排序的算法复杂度是多少；快排平均复杂度多少，最坏情况如何优化； 
> 快速排序：
> ```
def quicksort(data, low, hi):
    if low >= hi:
        return
    i = low
    j = hi
    v = data[i]
    while low < hi:
        while low < hi and data[hi] > v:
            hi -= 1
        data[low] = data[hi]
        while low < hi and data[low] <= v:
            low += 1
        data[hi] = data[low]
    data[hi] = v
    quicksort(data, i, low - 1)
    quicksort(data, low + 1, j)
> ```
> 堆排序：
> - 首先要理解何为完全二叉树：
> >若设二叉树的深度为h，除第 h 层外，其它各层 (1～h-1) 的结点数都达到最大个数，第 h 层所有的结点都连续集中在最左边，这就是**完全二叉树**。 
> 
> - 其次要了解何为堆：
> > 堆实际上是一棵完全二叉树，其任何一非叶节点满足性质：
 `Key[i]<=key[2i+1]&&Key[i]<=key[2i+2]`或者`Key[i]>=Key[2i+1]&&key>=key[2i+2]` 即可以看作任意父节点的值均大于其下两个子节点(大根堆)，或均小于其下两个子节点(小根堆)
> ```
>
>堆排序算法：
>def adjustHeap(data,i,n):
    j = 2*i + 1
    while(j<n):
        if(j+1<n and data[j]<data[j+1]):
            j += 1
        if data[i] > data[j]:
            break
        data[i],data[j] = data[j],data[i]
        i = j
        j = 2*i+1
&ensp;
def createHeap(data):
    i = 0
    n = (len(data)) / 2 - 1
    for i in range(int(n),-1,-1):
        adjustHeap(data,i,len(data))
&ensp;
def heapSort(data,len):
    i = 0;
    createHeap(data)
    for i in range(len-1,-1,-1):
        data[0],data[i] = data[i],data[0]
        adjustHeap(data,0,i)
&ensp;
def main():
      #     5
      #    / \
      #   6   4
      #  / \  /\
      # 10 2  1 8
    data = [5,6,4,10,7,1,8]
    # createHeap(data)
    heapSort(data,7)
    print(data)
&ensp;
&ensp;
if __name__ == '__main__':
    main()
> ```
> 
> 时间复杂度：
> 
> |排序方法	 |时间复杂度（平均）|时间复杂度（最坏)	|时间复杂度（最好)	|空间复杂度	|稳定性|	复杂性|
| :-------- : |  :-------- : |  :-------- : |  :-------- : |  :-------- : |  :-------- : |  :-------- : |
|直接插入排序	|O(n2)	|O(n2)	|O(n)	|O(1)	|稳定	|简单
|希尔排序	|O(nlog2n)|	O(n2)	|O(n)	|O(1)	|不稳定	|较复杂
|直接选择排序	|O(n2)|	O(n2)	|O(n2)	|O(1)	|不稳定	|简单
|堆排序	|O(nlog2n)|	O(nlog2n)	|O(nlog2n)	|O(1)	|不稳定	|较复杂
|冒泡排序|	O(n2)|	O(n2)	|O(n)	|O(1)	|稳定	|简单
|快速排序|O(nlog2n)|	O(n2)	|O(nlog2n)	|O(nlog2n)	|不稳定	|较复杂
|归并排序|	O(nlog2n)|	O(nlog2n)|	O(nlog2n)	|O(n)	|稳定	|较复杂
|基数排序|O(d(n+r))|	O(d(n+r))|	O(d(n+r))	|O(n+r)	|稳定	|较复杂
> - 最好(每次都能宣导中间值作枢轴)：$O(nlog_2n)$
> - 最坏(每次取最大或最小值作为枢轴)：$O(n^2)$
> - 平均：$O(nlog_2n)$
手写：已知一个长度n的无序列表，元素均是数字，要求把所有间隔为d的组合找出来，你写的解法算法复杂度多少； 
手写：一个列表A=[A1，A2，…,An]，要求把列表中所有的组合情况打印出来； 
手写：用一行python写出1+2+3+…+10**8 ； 
手写python：用递归的方式判断字符串是否为回文； 
单向链表长度未知，如何判断其中是否有环； 
单向链表如何使用快速排序算法进行排序； 
手写：一个长度n的无序数字元素列表，如何求中位数，如何尽快的估算中位数，你的算法复杂度是多少； 
如何遍历一个内部未知的文件夹（两种树的优先遍历方式）

网络基础部分
TCP/IP分别在模型的哪一层； 
> TCP:传输层
> IP:网络层

socket长连接是什么意思：
> 长连接：连接->传输数据->保持连接->传输数据->.....->关闭连接：
> 即不管是指建立连接后不管是否使用都会保持连接，但安全性较差。
> **那么在什么场景下使用长连接？**
> 一般来说，多用于操作频繁，点对点的通讯，且连接数不能过多。长连接存在的意义在于：对于数据的传送过程中，TCP三次握手、建立连接的操作比较耗时，当链接数足够多时，这将是一笔不小的开销。所以维持当前状态，每当连接传送时，只传递数据包即可。

select和epoll你了解么，区别在哪； 
TCP UDP区别；三次握手四次挥手讲一下； 
TIME_WAIT过多是因为什么； 
http一次连接的全过程：你来说下从用户发起request——到用户接收到response； 
http连接方式。get和post的区别，你还了解其他的方式么； 
restful你知道么； 
状态码你知道多少，比如200/403/404/504等等；

数据库部分
MySQL锁有几种；死锁是怎么产生的； 
为何，以及如何分区、分表； 
MySQL的char varchar text的区别； 
了解join么，有几种，有何区别，A LEFT JOIN B，查询的结果中，B没有的那部分是如何显示的（NULL）； 
索引类型有几种，BTree索引和hash索引的区别（我没答上来这俩在磁盘结构上的区别）； 
手写：如何对查询命令进行优化； 
NoSQL了解么，和关系数据库的区别；redis有几种常用存储类型；

Linux部分
讲一下你常用的Linux/git命令和作用； 
查看当前进程是用什么命令，除了文件相关的操作外，你平时还有什么操作命令； 
（因为我本人Linux本身就很水，只会基本的操作，所以这部分面试官也基本没怎么问。。反正问了就大眼瞪小眼呗）

django项目部分
都是让简单的介绍下你在公司的项目，不管是不是后端相关的，主要是要体现出你干了什么； 
你在项目中遇到最难的部分是什么，你是怎么解决的； 
你看过django的admin源码么；看过flask的源码么；你如何理解开源； 
### MVC / MVT：
> 分别阐述以下MVC和MVT的意义;
> MVC:
> <table>
	> <tr>
	> <td>M</td> <td>Model，主要封装对数据库层的访问，对数据库中的数据进行增删改查等操作。</td>
	> </tr>
	> <tr><td>V</td><td>View，主要用于封装结果，生成页面显示的html内容</td></tr>
	> <tr><td>C</td><td>Controller，用于接收请求，处理主要的业务逻辑，预model和view交互，返回结果。</td></tr></table>
> MVT
> <table>
	> <tr>
	> <td>M</td> <td>Model，与数据库进行交互，这点与MVC中的M相同。</td>
	> </tr>
	> <tr><td>V</td><td>View，用于接收请求，进行业务处理，返回应答，与MVC中的C相同。</td></tr>
	> <tr><td>T</td><td>Templates，负责封装构造要返回的HTML，与MVC中的V相同。</td></tr></table>


 
### 缓存怎么用:
> 访问一个网址时, 尝试从 cache 中找有没有缓存内容,如果有的话，则在缓存中直接返回该请求对应的网址；否则的话则生成新的页面内容，存到缓存中并返回，以下为伪代码：
>``` 
given a URL, try finding that page in the cache
if the page is in the cache:
    return the cached page
else:
    generate the page
    save the generated page in the cache (for next time)
    return the generated page 
```
> 用法：
> 1. 在settting中注册CACHES的使用方法。
> ```
> CACHES = {
    'default': {
        'BACKEND': 'django.core.cache.backends.filebased.FileBasedCache',
        'LOCATION': '/home/dbb2xbb/Django_proj/my_blog/django_cache',
        'TIMEOUT': 600,
        'OPTIONS': {
            'MAX_ENTRIES':1000
        }
    }
}
> ```
> 2. 导入响应的包，并且使用装饰器来装饰想要进行缓存的页面。
> ```
> from django.views.decorators.cache import cache_page
from django.core.paginator import Paginator, EmptyPage, PageNotAnInteger
cache_page(60 *50)
def home(request):
    pl = Article.objects.all()
    paginator = Paginator(pl, 2)
    page = request.GET.get('page')
    try:
        post_list = paginator.page(page)
    except PageNotAnInteger:
    ......
> ```
### 中间件是干嘛的：
> 中间件本质上就是类,并且这种类贯穿了请求到来至结束整个过程，django会根据自己的规则在适当的时机去执行相应的方法以达到对请求的干预作用。
> 中间件一共可以定义5种方法：
> process_request(self,request)
> process_view(self, request, callback, callback_args, callback_kwargs)
> process_template_response(self,request,response)
> process_exception(self, request, exception)
> process_response(self, request, response)	
> 其中前两种是从前之后执行的，后三种是从后往前执行的。
> 
> 中间件的途径顺序：
>![Alt text](./1526740732382.png)
>
> 中间件的运行流程：
> ![Alt text](./1526741467818.png)


CSRF是什么，django是如何避免的；XSS呢； 
如果你来设计login，简单的说一下思路； 
session和cookie的联系与区别；session为什么说是安全的； 
uWSGI和Nginx的作用； 

### 说一下git的使用方式：
> 从远程仓库上克隆项目到本地：
> $ ssh-keygen -t rsa -C “您的邮箱地址”

## 常见问题
### Pycharm中不能自动上传代码：
>CTRL+ALT+T-->Development，查看是否使用了正确的解释器，若未选中正确的解释器，则选中后应用再点OK。
>![Alt text](./1526049498957.png)

### 在正确配置了templates路径的前提下，运行程序提示`TemplateDoesNotExist`
> 这种情况一般出于项目迁移到另外开发机上时导致，可以重点观察一下，是否**django的版本在现有开发机和原开发机上不一致**导致，若不一致的话，可能还会提示`ImportError: No module named security`。

---
[^脚注1]:**线程切换间的开销：主要体现在线程的创建和上下文的切换。**
