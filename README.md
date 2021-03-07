# DevOps-Interview

## Linux

> [TLDR](https://github.com/tldr-pages/tldr)
> [Linux Performance](http://www.brendangregg.com/linuxperf.html)

![avatar](http://www.brendangregg.com/Perf/linux_observability_tools.png)
![avatar](http://www.brendangregg.com/BPF/bpf_performance_tools_book.png)

### command

#### 1. sed

```shell
 - Replace the first occurrence of a regular expression in each line of a file, and print the result:
   sed 's/{{regex}}/{{replace}}/' {{filename}}

 - Replace all occurrences of an extended regular expression in a file, and print the result:
   sed -r 's/{{regex}}/{{replace}}/g' {{filename}}

 - Replace all occurrences of a string in a file, overwriting the file (i.e. in-place):
   sed -i 's/{{find}}/{{replace}}/g' {{filename}}

 - Replace only on lines matching the line pattern:
   sed '/{{line_pattern}}/s/{{find}}/{{replace}}/' {{filename}}

 - Delete lines matching the line pattern:
   sed '/{{line_pattern}}/d' {{filename}}

 - Print the first 11 lines of a file:
   sed 11q {{filename}}

 - Apply multiple find-replace expressions to a file:
   sed -e 's/{{find}}/{{replace}}/' -e 's/{{find}}/{{replace}}/' {{filename}}

 - Replace separator / by any other character not used in the find or replace patterns, e.g., #:
   sed 's#{{find}}#{{replace}}#' {{filename}}

```

```shell
# sed 在第一行添加信息
sed -i '1i\{{text}}' {{filename}}
# -i edit files in place

# 在最后一行行前添加
sed -i '$i\{{text}}' {{filename}}

# 在最后一行行后添加
sed -i '$a\{{text}}' {{filename}}

```

#### 2. awk

```shell
 - Print the fifth column (a.k.a. field) in a space-separated file:
   awk '{print $5}' {{filename}}

 - Print the second column of the lines containing "foo" in a space-separated file:
   awk '/{{foo}}/ {print $2}' {{filename}}

 - Print the last column of each line in a file, using a comma (instead of space) as a field separator:
   awk -F ',' '{print $NF}' {{filename}}

 - Sum the values in the first column of a file and print the total:
   awk '{s+=$1} END {print s}' {{filename}}

 - Print every third line starting from the first line:
   awk 'NR%3==1' {{filename}}

 - Print different values based on conditions:
   awk '{if ($1 == "foo") print "Exact match foo"; else if ($1 ~ "bar") print "Partial match bar"; else print "Baz"}' {{filename}}

 - Print all lines where the 10th column value equals the specified value :
   awk '($10 == value)'

 - Print all the lines which the 10th column value is between a min and a max :
   awk '($10 >= min_value && $10 <= max_value)'

```

#### 3. top

```shell
- Start top:
   top

 - Do not show any idle or zombie processes:
   top -i

 - Show only processes owned by given user:
   top -u {{username}}

 - Sort processes by a field:
   top -o {{field_name}}

 - Show the individual threads of a given process:
   top -Hp {{process_id}}

 - Show only the processes with the given PID(s), passed as a comma-separated list. (Normally you wouldn't know PIDs off hand. This example picks the PIDs from the process name):
   top -p $(pgrep -d ',' {{process_name}})

 - Get help about interactive commands:
   ?

```

```shell
# 按 1 后会展开每个核的使用信息
top - 22:23:55 up 54 min,  0 users,  load average: 0.00, 0.00, 0.00
Tasks:  22 total,   1 running,  21 sleeping,   0 stopped,   0 zombie
%Cpu0  :  0.0 us,  0.0 sy,  0.0 ni,100.0 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
%Cpu1  :  0.0 us,  0.0 sy,  0.0 ni,100.0 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
%Cpu2  :  0.0 us,  0.0 sy,  0.0 ni,100.0 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
%Cpu3  :  0.0 us,  0.0 sy,  0.0 ni,100.0 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
%Cpu4  :  1.0 us,  0.0 sy,  0.0 ni, 99.0 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
%Cpu5  :  0.3 us,  0.0 sy,  0.0 ni, 99.7 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
%Cpu6  :  0.0 us,  0.0 sy,  0.0 ni,100.0 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
%Cpu7  :  0.0 us,  0.0 sy,  0.0 ni,100.0 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
%Cpu8  :  0.0 us,  0.0 sy,  0.0 ni,100.0 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
%Cpu9  :  0.0 us,  0.0 sy,  0.0 ni,100.0 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
%Cpu10 :  0.0 us,  0.0 sy,  0.0 ni,100.0 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
%Cpu11 :  0.0 us,  0.0 sy,  0.0 ni,100.0 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
%Cpu12 :  0.0 us,  0.0 sy,  0.0 ni,100.0 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
%Cpu13 :  0.0 us,  0.0 sy,  0.0 ni,100.0 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
%Cpu14 :  0.0 us,  0.0 sy,  0.0 ni,100.0 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
%Cpu15 :  0.0 us,  0.0 sy,  0.0 ni,100.0 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st

# c 展开进程 command 信息
 1505 root      20   0    1252    348     16 S   0.0   0.0   0:00.00 /init
 1506 kchou     20   0    2608    596    528 S   0.0   0.0   0:00.00 sh -c "$VSCODE_WSL_EXT_LOCATION/scripts/wslServer.sh" 622cb03f7e070a9670c94bae1a45d78d7181fbd4 stable .vscode-server 0
 1507 kchou     20   0    2608   1864   1752 S   0.0   0.0   0:00.00 sh /mnt/c/Users/kylec/.vscode/extensions/ms-vscode-remote.remote-wsl-0.53.4/scripts/wslServer.sh 622cb03f7e070a9670c94bae1a45d78d7181fbd4 stable .vscode-server 0
 1512 kchou     20   0    2608    540    468 S   0.0   0.0   0:00.00 sh /home/kchou/.vscode-server/bin/622cb03f7e070a9670c94bae1a45d78d7181fbd4/server.sh  --port=0 --use-host-proxy --enable-remote-auto-shutdown --print-ip-address
 1514 kchou     20   0  979152  65940  31608 S   0.0   0.3   0:01.86 /home/kchou/.vscode-server/bin/622cb03f7e070a9670c94bae1a45d78d7181fbd4/node /home/kchou/.vscode-server/bin/622cb03f7e070a9670c94bae1a45d78d7181fbd4/out/vs/server/main.js  --port=0 --use-host-proxy -+
 1542 kchou     20   0  856364  41412  28592 S   0.0   0.2   0:00.19 /home/kchou/.vscode-server/bin/622cb03f7e070a9670c94bae1a45d78d7181fbd4/node /home/kchou/.vscode-server/bin/622cb03f7e070a9670c94bae1a45d78d7181fbd4/out/bootstrap-fork --type=watcherService
 1808 kchou     20   0    6036    460      0 S   0.0   0.0   0:00.00 ssh-agent -s
 1874 kchou     20   0    9576   3996   3748 S   0.0   0.0   0:00.00 git fetch
 1875 kchou     20   0   12048   6292   5564 S   0.0   0.0   0:00.00 /usr/bin/ssh git@github.com git-upload-pack 'bernylinville/DevOps-Interview.git'
 1877 kchou     20   0  559556  37632  26416 S   0.0   0.1   0:00.15 /home/kchou/.vscode-server/bin/622cb03f7e070a9670c94bae1a45d78d7181fbd4/node /home/kchou/.vscode-server/bin/622cb03f7e070a9670c94bae1a45d78d7181fbd4/extensions/json-language-features/server/dist/node+
 1933 root      20   0     896     84     16 S   0.0   0.0   0:00.00 /init

# shift + c 按 cpu 排序

# m 切换内存显示
MiB Mem :  2.5/25621.4  [||                                                                                                  ]
MiB Swap:  0.0/7168.0   [                                                                                                    ]

# shift + m 按内存排序

```

##### 进程状态有哪几种

running/sleeping/stopped/zombie

##### 第三行 cpu 参数解释

* %us 用户空间占用CPU百分比
* %sy 内核空间占用CPU百分比
* %ni 用户进程空间内改变过优先级的进程占用CPU百分比
* %id 空闲CPU百分比
* %wa 等待输入输出（I/O）的CPU时间百分比 
* %hi 指的是cpu处理硬件中断的时间
* %si指的是cpu处理软中断的时间
* %st 用于有虚拟cpu的情况，用来指示被虚拟机偷掉的cpu时间

> 通常id%值可以反映一个系统cpu的闲忙程度。

##### 横栏说明

PID(进程号)、 USER（运行用户）、PR（优先级）、NI（任务nice值）、VIRT（虚拟内存用量）VIRT=SWAP+RES 、RES（物理内存用量）、SHR（共享内存用量）、S（进程状态）、%CPU（CPU占用比）、%MEM（物理内存占用比）、TIME+（累计CPU占 用时间)、　COMMAND 命令名/命令行。

##### top 命令

* d 指定每两次屏幕信息刷新之间的时间间隔。当然用户可以使用s交互命令来改变之。
* p 通过指定监控进程ID来仅仅监控某个进程的状态。
* q 该选项将使top没有任何延迟的进行刷新。如果调用程序有超级用户权限，那么top将以尽可能高的优先级运行。
* S 指定累计模式。
* s 使top命令在安全模式中运行。这将去除交互命令所带来的潜在危险。
* i 使top不显示任何闲置或者僵死进程。
* c 显示整个命令行而不只是显示命令名。

##### 交互命令

> 从使用角度来看，熟练的掌握这些命令比掌握选项还重要一些。
> 这些命令都是单字母的，如果在命令行选项中使用了s选项，则可能其中一些命令会被屏蔽掉。

* Ctrl+L 擦除并且重写屏幕。
* h或者? 显示帮助画面，给出一些简短的命令总结说明。
* k 终止一个进程。系统将提示用户输入需要终止的进程PID，以及需要发送给该进程什么样的信号。一般的终止进程可以使用15信号；如果不能正常结束那就使用信号9强制结束该进程。默认值是信号15。在安全模式中此命令被屏蔽。
* i 忽略闲置和僵死进程。这是一个开关式命令。
* q 退出程序。
* r 重新安排一个进程的优先级别。系统提示用户输入需要改变的进程PID以及需要设置的进程优先级值。输入一个正值将使优先级降低，反之则可以使该进程拥有更高的优先权。默认值是10。
* s 改变两次刷新之间的延迟时间。系统将提示用户输入新的时间，单位为s。如果有小数，就换算成m s。输入0值则系统将不断刷新，默认值是5 s。需要注意的是如果设置太小的时间，很可能会引起不断刷新，从而根本来不及看清显示的情况，而且系统负载也会大大增加。
* f或者F 从当前显示中添加或者删除项目。
* o或者O 改变显示项目的顺序。
* l 切换显示平均负载和启动时间信息。
* m 切换显示内存信息。
* t 切换显示进程和CPU状态信息。
* c 切换显示命令名称和完整命令行。
* M 根据驻留内存大小进行排序。
* P 根据CPU使用百分比大小进行排序。
* T 根据时间/累计时间进行排序。
* W 将当前设置写入~/.toprc文件中。这是写top配置文件的推荐方法。
* Shift+M 可按内存占用情况进行排序。

#### 4. ps

#### 5. netstat

#### 6. tail

#### 7. cat

#### 8. less

#### 9. df

#### 10. du

#### 11. lsof

#### 12. lspid

#### 13. dmesg

#### 14. iotop

#### 15. pidstat

#### 16. iostat

#### 17. mpstat

#### 18. free

#### 19. vmstat

#### 20. find

```shell
# 在指定目录删除 7 天之前的文件
find {{dir}}/ -type f -mtime +7 -exec rm -f {} \;
# 7 天之内
find {{dir}}/ -type f -mtime -7 -exec rm -f {} \;

```

#### 21. ulimit

> core dump ???
> [using_ulimt](https://wiki.archlinux.org/index.php/Core_dump#Using_ulimit)

### 1. 什么是并发、并行、阻塞、异步、同步

* 并发/并行: CPU在执行多个任务时的方式，并发表示同一段时间里面有多个进程在同一CPU执行，在极短的时间互相切换使人不会发觉。并行只会出现在多个CPU的情况下，表示同一时刻之内有多个进程在执行
例子： 在开车的时候有个电话，必须停下车才可以接电话，接完电话继续开车，这就是并发。 如果一边开车一遍接电话，这就是并行

* 同步/异步: 关注的是请求和响应的通讯机制，描述的是被调用方。当发出请求后，该请求是否等待结果后再返回，同步就是没有得到结果前不会返回，返回即得到请求结果。异步就是得到发出请求后就直接返回，也可能不会立即得到结果，服务得到结果后再通过通知或者回调函数等方法通知调用者
例子: 去买咖啡，付了钱在前台等待咖啡制作完毕，就是同步，付了钱不在前台等待，找位置坐下，服务员送过来就是异步

* 阻塞/非阻塞: 关注的是请求在等待结果时的状态，描述的是调用方。 阻塞就是在等待结果的时候，当前线程会被挂起，在得到结果之后返回；非阻塞则是没有得到结果之前也不会阻塞当前线程
例子: 阻塞的情况就是卖咖啡的时候什么都不能做，只能挂起；非阻塞的时候就是在等咖啡的时候可以玩着手机，过一会检查咖啡是否好了

## Python

### 常用的内置函数

| 函数名     | 参数            | 介绍                         | 返回值   | 举例                               |
| ---------- | --------------- | ---------------------------- | -------- | ---------------------------------- |
| abs        | Number          | 返回数字绝对值               | 正数     | abs(-10)                           |
| all        | List            | 判断列表内容是否全是 true    | Bool     | all(['', '123']                    |
| help       | Object          | 打印对象的帮助用法           | 无       | help(list)                         |
| enumerate  | Iterable        | 迭代时记录索引               | 无       | for index, item in enumerate(list) |
| input      | Str             | 命令行输入消息               | Str      | input('请输入信息：')              |
| isinstance | Object, type    | 判断对象是否是某种类型       | Bool     | isinstance('a', str)               |
| type       | Object          | 判断对象的类型               | Str      | type(10)                           |
| vars       | instance        | 返回实例化的字典信息         | dict     |                                    |
| dir        | Object          | 返回对象中所有可用方法和属性 | List     | dir('type')                        |
| hasattr    | Obj, key        | 判断对象中是否有某个属性     | Bool     | hasttr('1', 'upper')               |
| setattr    | Obj, key, value | 为实例化对象添加属性与值     | 无       | setattr(instance, 'run', 'go')     |
| getattr    | Obj, key        | 通过对象获取属性             | 任何类型 | getattr(Obj, key)                  |
| any        | Iterable        | 判断内容是否有 true 值       | Bool     | any([1, 0, ''])                    |

### 装饰器

* 也是一种函数
* 可以接受函数作为参数
* 可以返回函数
* 接受一个函数，内部对其处理，然后返回一个新函数，动态的增强函数功能
* 将 c 函数在 a 函数中执行，在 a 函数中可以选择执行或不执行 c 函数，也可以对 c 函数的结果进行二次加工处理

```python
def out(func_args): # 外围函数
    def inter(*args, **kwargs): # 内嵌函数
        return func_args(*args, **kwargs)
    return inter # 外围函数返回内嵌函数

```

#### 用法

* 将被调用的函数直接作为参数传入装饰器的外围函数括弧

```python
def a(func):
    def b(*args, **kwargs):
        return func(*args, **kwargs)
    return b

def c(name):
    print(name)

a(c('berny')) # berny

```

* 将装饰器与被调用函数绑定在一起

@符号 + 装饰器函数放在被调用函数的上一行，被调用的函数正常定义，只需要直接调用被执行函数即可

```python
@a
def c(name):
    print(name)

c('berny') # berny

```

#### classmethod

* 将类函数可以不经过实例化而直接被调用

```python
@classmethod
def func(cls, ...)
    do

```

```python
class Test(object):
    @classmethod
    def add(cls, a, b):
        return a + b

Test.add(1, 2)

```

#### staticmethod

* 将类函数可以不经过实例化而直接被调用，被该装饰器调用的函数不许传递 self 或 cls 参数，且无法在该函数内调用其他类函数或类变量

```python
@staticmethod
def func(...)
    do

```

```python
class Test(object):
    @staticmethod
    def add(a, b):
        return a + b

Test.add(1, 2)

```

#### property

将类函数的执行免去括弧，类似于调用属性（变量）

```python
@property
def funcself
    do

```

```python
class Test(object):
    def __init__(self, name):
        self.name = name

    @propperty
    def call_name(self):
        return 'hello {}'.format(self.name)

test = Test('berny')
result = test.call_name
print(result) # berny

```

```python
class Test(object):
    def __init__(self, __name):
        self.__name = name

    @propperty
    def name(self):
        return self.__name

    @name.setter
    def name(self, value):
        self.__name = value

test = Test('berny')
print(test.name) # berny
test.name = 'test'
print(test.name) # test

```

### 一些类中的函数说明

#### __str__

返回对于该类的描述

```python
class Test(object):
    def __str__(self):
        return '描述'

test = Test()
print(test) # 描述

```

#### __getattr__

当调用的属性或者方法不存在时，会返回该方法定义的信息

```python
class Test(object):
    def __getattr__(self, key):
        print('key: {}不存在'.format(key))

test = Test()
test.a

```

#### __setattr__

拦截当前类中不存在的属性与值

```python
class Test(object):
    def __setattr__(self, key, value):
        if key not in self.__dict__:
            self.__dict__[key] = value

test = Test()
test.name = 'berny'
t.name # 'berny'

```

#### __call__

本质是将一个类变成一个函数

```python
class Test(object):
    def __call__(self, **kwargs):
        print('args is {}'.format(kwargs))

test = Test()
test(name='berny') # args is {'name': 'berny'}

```

### python 有哪些设计模式

[python36种设计模式](https://github.com/ydf0509/python36patterns)

### 浅拷贝和深拷贝区别

#### 浅拷贝

b = a.copy()

a b 列表内部数据发生变化后，会相互影响

```shell
In [7]: a = [[1,2,3],[4,5,6]]

In [8]: b = a.copy()

In [9]: b[1].append(7)

In [10]: b
Out[10]: [[1, 2, 3], [4, 5, 6, 7]]

In [11]: a
Out[11]: [[1, 2, 3], [4, 5, 6, 7]]

In [12]: a[0].append(8)

In [13]: a
Out[13]: [[1, 2, 3, 8], [4, 5, 6, 7]]

In [14]: b
Out[14]: [[1, 2, 3, 8], [4, 5, 6, 7]]

```

#### 深拷贝

对深层数据也进行了拷贝，原是变量和新变量完全不共享数据

```b = copy.deepcopy(a)```

### 迭代器(Iterator)与生成器(Generator)的区别

迭代器是一个更抽象的概念，任何对象，如果它的类有next方法（__next__ python3)和__iter__方法返回自己本身。

每个生成器都是一个迭代器，但是反过来不行。通常生成器是通过调用一个或多个yield表达式构成的函数s生成的。同时满足迭代器的定义。

当你需要一个类除了有生成器的特性之外还要有一些自定义的方法时，可以使用自定义的迭代器，一般来说生成器更方便，更简单。

```python
    def squares(start, stop):
        for i in xrange(start, stop):
            yield i*i
```

等同于生成器表达式：

```python
    （i*i for i in xrange(start, stop))```

列表推倒式是：

```python
    [i*i for i in xrange(start, stop)]```

如果是构建一个自定义的迭代器：

```python
    class Squares(object):
        def __init__(self, start, stop):
            self.start = start
            self.stop = stop
        def __iter__(self):
            return self
        def next(self):
            if self.start >= self.stop:
                raise StopIteration
            current = self.start * self.start
            self.start += 1
            return current
```

此时，你还可以定义自己的方法如：

```python
    def current(self):
        return self.start
```

两者的相同点：对象迭代完后就不能重写迭代了。

#### Iterables, Iterators, Genrators

==============================

热身一下

--------------
如果你是来自其它语言比如c，很自然想到的方式是创建一个计数器，然后以自增的方式迭代list。

```python
    my_list = [17  23  47  51  101  173  999  1001]

    i = 0
    while i < len(my_list):
        v = my_list[i]
        print v,
        i += 1
输出：

    17 23 47 51 101 173 999 1001
```

也有可能会借用range，写一个类C语言的风格的for循环：

```python
    for i in range(len(my_list)):
        v = my_list[i]
        print v,
输出：

    17 23 47 51 101 173 999 1001
```

上面两种方法都不是Pythonic方式，取而代之的是：

```python
    for v in my_list:
        print v,
输出：

    17 23 47 51 101 173 999 1001
```

很多类型的对象都能通过这种方式来迭代，迭代字符串会生成单个字符：

```python
    for v in "Hello":
        print v,
输出：

    H e l l o
```

迭代字典，生成字典的key（以无序的方式）：

```python
    d = {
        'a': 1,
        'b': 2,
        'c': 3,
        }

    for v in d:
        print v,
    # 注意这里是无序的

输出：

    a c b
```

迭代文件对象，产生字符串行，包括换行符：

```python
    f = open("suzuki.txt")
    for line in f:
        print ">", line
输出：

    > On education

    > "Education has failed in a very serious way to convey the most important lesson science can teach: skepticism."

    > "An educational system isn't worth a great deal if it teaches young people how to make a living but doesn't teach them how to make a life."
```

以上可以看出列表、元祖、字符串、字典、文件都可以迭代，能被迭代的对象都称为可迭代对象（Iteratbles)，for循环不是唯一接收Iteratbles的东东，还有：

list构造器接收任何类型的Iteratbles，可以使用list()接收字典对象返回只有key的列表：

```python
    list(d)
输出：

    ['a', 'c', 'b']
还可以：

    list("Hello")
输出：

    ['H', 'e', 'l', 'l', 'o']

还可以用在列表推倒式中：

    ascii = [ord(x) for x in "Hello"]
    ascii
输出：

    [72, 101, 108, 108, 111]
```

sum()函数接收任何数字类型的可迭代对象:

```python
    sum(ascii)

输出：

    500
```

str.join()方法接收任何字符类型的可迭代对象 （这里的说法不严谨，总之原则是迭代的元素必须是str类型的)：

```python
    "-".join(d)
输出：

    ‘a-c-b'
```

[difference-between-python-generators-vs-iterators](http://stackoverflow.com/questions/2776829/difference-between-python-generators-vs-iterators)
[itergen1](http://excess.org/article/2013/02/itergen1/)

## Ansible

## Docker

## Kubernetes

### 1. Kubernetes Components

![avatar](https://d33wubrfki0l68.cloudfront.net/2475489eaf20163ec0f54ddc1d92aa8d4c87c96b/e7c81/images/docs/components-of-kubernetes.svg)

> [Kubernetes Components](https://kubernetes.io/docs/concepts/overview/components/)

#### Control Plane Components

* kube-apiserver

Kubernetes API，集群的统一入口，各组件协调者，以 RESTful API提供接口服务，所有对象资源的增删改查和监听操作都交给 APIServe 处理后再提交给 Etcd 存储。

* etcd

分布式键值存储系统。用于保存集群状态数据，比如 Pod、Service 等对象信息。

* kube-scheduler

根据调度算法为新创建的 Pod 选择一个 Node 节点，可以任意部署,可以部署在同一个节点上,也可以部署在不同的节点上。

* kube-controller-manager/cloud-controller-manager

处理集群中常规后台任务，一个资源对应一个控制器，而 ControllerManager 就是负责管理这些控制器的。

#### Node Components

* kubelet

kubelet 是 Master 在 Node 节点上的 Agent，管理本机运行容器的生命周期，比如创建容器、Pod 挂载数据卷、下载 secret、获取容器和节点状态等工作。kubelet 将每个 Pod 转换成一组容器。

* kube-proxy

在 Node 节点上实现 Pod 网络代理，维护网络规则和四层负载均衡工作。

* Container Runtime

### 2. Pod 的生命周期

> [Pod 的生命周期](https://kubernetes.io/docs/concepts/workloads/pods/pod-lifecycle/)

* Pod 生命期: 和一个个独立的应用容器一样，Pod 也被认为是相对临时性（而不是长期存在）的实体。 Pod 会被创建、赋予一个唯一的 ID（UID）， 并被调度到节点，并在终止（根据重启策略）或删除之前一直运行在该节点。
* Pod 阶段: Pending -> Running -> Succeeded/Failed
* 容器状态: Waiting（等待）、Running（运行中）和 Terminated（已终止）
* 容器重启策略: Always、OnFailure 和 Never
* Pod 状况
  * PodScheduled：Pod 已经被调度到某节点；
  * ContainersReady：Pod 中所有容器都已就绪；
  * Initialized：所有的 Init 容器 都已成功启动；
  * Ready：Pod 可以为请求提供服务，并且应该被添加到对应服务的负载均衡池中。
* 容器探针
  * livenessProbe：指示容器是否正在运行。如果存活态探测失败，则 kubelet 会杀死容器， 并且容器将根据其重启策略决定未来。如果容器不提供存活探针， 则默认状态为 Success。
  * readinessProbe：指示容器是否准备好为请求提供服务。如果就绪态探测失败， 端点控制器将从与 Pod 匹配的所有服务的端点列表中删除该 Pod 的 IP 地址。 初始延迟之前的就绪态的状态值默认为 Failure。 如果容器不提供就绪态探针，则默认状态为 Success。
  * startupProbe: 指示容器中的应用是否已经启动。如果提供了启动探针，则所有其他探针都会被 禁用，直到此探针成功为止。如果启动探测失败，kubelet 将杀死容器，而容器依其 重启策略进行重启。 如果容器没有提供启动探测，则默认状态为 Success。
* Pod 的终止： 由于 Pod 所代表的是在集群中节点上运行的进程，当不再需要这些进程时允许其体面地 终止是很重要的。一般不应武断地使用 KILL 信号终止它们，导致这些进程没有机会 完成清理操作。

## CI/CD

## Network

### 1. HTTP HTTPS 区别

* HTTP 三次握手建立连接
* HTTPS （三次握手中）客户端请求 HTTPS 连接后，服务器返回证书（公钥），客户端生成随机对称密钥，使用公钥对密钥加密，再发送给服务器加密后的对称密钥，最后两边通过对称密钥加密的密文通信
* HTTPS 是HTTP 协议的安全版本，HTTP 协议的数据传输是明文的，是不安全的，HTTPS 使用了 SSL/TLS 协议进行了加密处理。
* HTTP 和 HTTPS 使用连接方式不同，默认端口也不一样，HTTP 是80，HTTPS 是443

### 2. tcp 三次握手，状态

![avatar](https://camo.githubusercontent.com/02987d37d9ecf26ec08473c5b3f530836c490a06a598ee4b0d3cd6032e430513/68747470733a2f2f67697465652e636f6d2f6875696875742f696e746572766965772f7261772f6d61737465722f696d616765732f5443502545342542382538392545362541432541312545362538462541312545362538392538422545352542422542412545372541422538422545382542462539452545362538452541352e706e67)

1. 客户端发送 SYN 给服务器，说明客户端请求建立连接；
2. 服务端收到客户端发的 SYN，并回复 SYN+ACK 给客户端（同意建立连接）；
3. 客户端收到服务端的 SYN+ACK 后，回复 ACK 给服务端（表示客户端收到了服务端发的同意报文）；
4. 服务端收到客户端的 ACK，连接已建立，可以数据传输。

### 3. 四层和七层负载均衡区别

1. 四层负载均衡工作在OSI模型中的四层，即传输层。四层负载均衡只能根据报文中目标地址和源地址对请求进行转发，而无法修改或判断所请求资源的具体类型，然后经过负载均衡内部的调度算法转发至要处理请求的服务器。四层负载均衡单纯的提供了终端到终端的可靠连接，并将请求转发至后端，连接至始至终都是同一个。LVS就是很典型的四层负载均衡。
2. 七层负载均衡工作在OSI模型的第七层应用层，所以七层负载均衡可以基于请求的应用层信息进行负载均衡，例如根据请求的资源类型分配到后端服务器，而不再是根据IP和端口选择。七层负载均衡的功能更丰富更灵活，也能使整个网络更智能。如上图所示，在七层负载均衡两端(面向用户端和服务器端)的连接都是独立的。
3. 简言之，四层负载均衡就是基于IP+端口实现的。七层负载均衡就是通过应用层资源实现的。

### 4. OSI 七层模型

* 物理层：主要定义物理设备标准，如网线的接口类型、光纤的接口类型、各种传输介质的传输速率等。它的主要作用是传输比特流（就是由1、0转化为电流强弱来进行传输,到达目的地后在转化为1、0，也就是我们常说的数模转换与模数转换），这一层的数据叫做比特。
* 数据链路层：定义了如何让格式化数据以进行传输，以及如何让控制对物理介质的访问，这一层通常还提供错误检测和纠正，以确保数据的可靠传输。
* 网络层：在位于不同地理位置的网络中的两个主机系统之间提供连接和路径选择，Internet的发展使得从世界各站点访问信息的用户数大大增加，而网络层正是管理这种连接的层。
* 传输层：定义了一些传输数据的协议和端口号（WWW端口80等），如：TCP（传输控制协议，传输效率低，可靠性强，用于传输可靠性要求高，数据量大的数据），UDP（用户数据报协议，与TCP特性恰恰相反，用于传输可靠性要求不高，数据量小的数据，如QQ聊天数据就是通过这种方式传输的）， 主要是将从下层接收的数据进行分段和传输，到达目的地址后再进行重组，常常把这一层数据叫做段。
* 会话层：通过传输层（端口号：传输端口与接收端口）建立数据传输的通路，主要在你的系统之间发起会话或者接受会话请求（设备之间需要互相认识可以是IP也可以是MAC或者是主机名）。
* 表示层：可确保一个系统的应用层所发送的信息可以被另一个系统的应用层读取。例如，PC程序与另一台计算机进行通信，其中一台计算机使用扩展二一十进制交换码（EBCDIC），而另一台则使用美国信息交换标准码（ASCII）来表示相同的字符。如有必要，表示层会通过使用一种通格式来实现多种数据格式之间的转换。
* 应用层： 是最靠近用户的OSI层，这一层为用户的应用程序（例如电子邮件、文件传输和终端仿真）提供网络服务。

### 5. TCP/IP 四层

* 应用层：负责向用户提供应用程序，比如HTTP、FTP、Telnet、DNS、SMTP等。
* 传输层：负责对报文进行分组和重组，并以TCP或UDP协议格式封装报文。
* 网络层：负责路由以及把分组报文发送给目标网络或主机。
* 链路层：负责封装和解封装IP报文，发送和接受ARP/RARP报文等 下面的是两者之间的对比:

两者比较图：

![avatar](https://raw.githubusercontent.com/bernylinville/DevOps-Interview/main/tcp-ip.png)

### 6. 常见的http状态码

* 503 服务不可用
* 404 未找到
* 499 客户端主动断开了连接
* 500 服务器内部错误
* 504 网关超时
* 301 永久转移
* 302 临时转移

### 7. ssl 握手过程

基于RSA握手和密钥交换的客户端验证服务器为实例详解TLS/SSL握手过程

![avatar](https://raw.githubusercontent.com/bernylinville/DevOps-Interview/main/ssl1.png)
![avatar](https://raw.githubusercontent.com/bernylinville/DevOps-Interview/main/ssl2.png)

## Monitoring

## Nginx/LVS

### 1. nginx 优化

```shell
# 隐藏版本号
server_tokens off;
# 修改用户和组
# expires缓存(一般是图片)
# 日志切割
# 设置超时时间
keepalive_timeout   #设置超时时间
client_header_timeout  #指定请求头的超时时间
client_body_timeout    #设置请求体超时时间
* Gzip压缩
Nginx_http_gzip_module压缩模块提供了对文件压缩的功能，以节约网站的带宽，提高用户的访问体验
默认Nginx已经安装该模块
    gzip on    #开启gzip压缩输出
    gzip_buffers 4 64;   #表示申请了4个单位为64kb的内存作为压缩结果流缓存
    gzip_http_version 1.1  #用于设置http协议版本
    gzip_min_length     #设置允许页面的最小字节
    gzip_vary on;       #让前端的缓存服务器缓存就经过的gzip压缩页面
* 防盗链
Nginx防盗链的原理是加入location项，用正则表达式过滤图片类型文件，对于信任的网站可以正常使用，对于不信任的网址则返回相应的错误页面
     [root@localhost ~]# vim /usr/local/nginx/conf/nginx.conf
          location ~*.(jpg|gif|swf)$ {
            valid_referers none blocked *.benet.com benet.com;
            if ( $invalid_referer ) {
               rewrite ^/ http://www.benet.com/error.png;
            }
         }
    ~*.(jpg|gif|swf)$: 匹配不区分大小写，以.jpg 或.gif或 .swf结尾的文件。
    valid_referers：设置信任的网站，可以正常使用图片。
    none：浏览器中refer为空的情况，就是直接在浏览器访问图片。
    blocked：浏览器中refer不为空的情况，但是值被代理或防火墙删除了，这些值不以http://或 https://开头。
    后面的网址或域名：refer包含相关字符串的网址。
    if语句：如果链接的来源域名不在valid_referers所列出的列表中， $invalid_referer 为1，则执行后面的操作，即进行重写或返回403页面

```

### 2. nginx upsteam 轮询方式

* RR 轮询: 默认的反向代理模式，用以平衡各个服务器的负载，若某个服务器宕机，会自动从轮询中剃掉。同时，可以手动指定某台服务器脱离轮询，用于离线检查
* weight 权重: 针对服务器性能不通，用来控制服务器被访问的比例，以实现老客户访问时的快速调度
* ip hash：主要记录了客户端IP访问的目标主机，以实现老用户访问的快速调度

### 3. nginx 跨域

首先我们要知道什么是跨域，跨域指的是浏览器不能执行其他网站的脚本，它是由浏览器的同源策略造成的，是浏览器对JavaScript施加的安全限制

举例:A页面想获取B页面资源，如果a,b页面的协议、端口、域名、子域名不同，所进行的所有访问请求都是跨域的，而浏览器一般为了安全都限制跨域请求，也就是不允许跨域请求资源。注意：跨域其实是浏览器的限制！

```shell
#如何解决跨域请求？
Access-Control-Allow-Origin: http://api.bob.com
Access-Control-Allow-Credentials: true
Access-Control-Expose-Headers: FooBar
Content-Type: text/html; charset=utf-8

Access-Control-Allow-Origin
# 这里大概意思是允许跨域的域名，可以写一个域名，或者写* 代表所有
Access-Control-Allow-Credentials
# 表示是否允许发送Cookie，默认情况下，Cookie不包括在CORS请求之中。设置为true，表示服务器明确许可，Cookie可以包含在请求中，一起发给服务器，如果服务器不需要浏览器发送的Cookie，删除该字段即可
Access-Control-Expose-Headers
# 表示请求头的字段 动态获取

```

### 4. nginx 反向代理和正向代理区别

* 正向代理

正向代理 是一个位于客户端和原始服务器(origin server)之间的服务器，为了从原始服务器取得内容，客户端向代理发送一个请求并指定目标(原始服务器)，然后代理向原始服务器转交请求并将获得的内容返回给客户端。客户端必须要进行一些特别的设置才能使用正向代理。就像要访问google用vpn代理翻墙去访问

* 反向代理

对于客户端而言它就像是原始服务器，并且客户端不需要进行任何特别的设置。客户端向反向代理 的命名空间(name-space)中的内容发送普通请求，接着反向代理将判断向何处(原始服务器)转交请求，并将获得的内容返回给客户端，就像这些内容 原本就是它自己的一样。（用户不知道要访问真正的服务器

#### 以租房为例解释正向代理和反向代理

正向代理 客户端 <一> 代理 一>服务端
A(客户端)想租C(服务端)的房子,但是A(客户端)并不认识C(服务端)租不到。
B(代理)认识C(服务端)能租这个房子所以你找了B(代理)帮忙租到了这个房子。
这个过程中C(服务端)不认识A(客户端)只认识B(代理)
C(服务端)并不知道A(客户端)租了房子，只知道房子租给了B(代理)。

反向代理 客户端 一>代理 <一> 服务端
A(客户端)想租一个房子,B(代理)就把这个房子租给了他。
这时候实际上C(服务端)才是房东。
B(代理)是中介把这个房子租给了A(客户端)。
这个过程中A(客户端)并不知道这个房子到底谁才是房东
他都有可能认为这个房子就是B(代理)的

> 由上的例子我们可以知道正向代理和反向代理的区别在于代理的对象不一样,正向代理的代理对象是客户端,反向代理的代理对象是服务端。

### 5. Nginx 和LVS 区别

1. 高并发连接：官方测试能够支撑5万并发连接，在实际生产环境中跑到2——3万并发连接数。
2. 内存消耗少：在3万并发连接数下，开启的10个nginx进程才消耗150M内存（150*10=150M）。
3. 配置文件非常简单：风格跟程序一样通俗易懂。
4. 成本低廉：nginx为开源软件，可以免费使用。而购买F5 big-ip、netscaler等硬件负载均衡交换机则需要十多万至几十万人民币。
5. 支持rewrite重写规则：能够根据域名、url的不同，将http请求分到不同的后端服务器群组。
6. 内置的健康检查功能：如果nginx proxy后端的某台web服务器宕机了，不会影响前端访问。
7. 节省带宽：支持gzip压缩，可以添加浏览器本地缓存的header头。

### 6. Nginx、LVS、haproxy区别

#### Nginx 优点

* 工作在网络的7层之上，可以针对http应用做一些分流的策略，比如针对域名、目录结构，它的正则规则比HAProxy更为强大和灵活，这也是它目前广泛流行的主要原因之一，Nginx单凭这点可利用的场合就远多于LVS了。
* Nginx对网络稳定性的依赖非常小，理论上能ping通就就能进行负载功能，这个也是它的优势之一；相反LVS对网络稳定性依赖比较大。
* Nginx安装和配置比较简单，测试起来比较方便，它基本能把错误用日志打印出来。LVS的配置、测试就要花比较长的时间了，LVS对网络依赖比较大。
* 可以承担高负载压力且稳定，在硬件不差的情况下一般能支撑几万次的并发量，负载度比LVS相对小些。
* Nginx可以通过端口检测到服务器内部的故障，比如根据服务器处理网页返回的状态码、超时等等，并且会把返回错误的请求重新提交到另一个节点，不过其中缺点就是不支持url来检测。比如用户正在上传一个文件，而处理该上传的节点刚好在上传过程中出现故障，Nginx会把上传切到另一台服务器重新处理，而LVS就直接断掉了，如果是上传一个很大的文件或者很重要的文件的话，用户可能会因此而不满。
* Nginx不仅仅是一款优秀的负载均衡器/反向代理软件，它同时也是功能强大的Web应用服务器。LNMP也是近几年非常流行的web架构，在高流量的环境中稳定性也很好。
* Nginx现在作为Web反向加速缓存越来越成熟了，速度比传统的Squid服务器更快，可以考虑用其作为反向代理加速器。
* Nginx可作为中层反向代理使用，这一层面Nginx基本上无对手，唯一可以对比Nginx的就只有 lighttpd了，不过 lighttpd目前还没有做到Nginx完全的功能，配置也不那么清晰易读，社区资料也远远没Nginx活跃。
* Nginx也可作为静态网页和图片服务器，这方面的性能也无对手。还有Nginx社区非常活跃，第三方模块也很多。

#### Nginx 缺点

Nginx仅能支持http、https和Email协议，这样就在适用范围上面小些，这个是它的缺点。
对后端服务器的健康检查，只支持通过端口来检测，不支持通过url来检测。不支持Session的直接保持，但能通过ip_hash来解决。

#### LVS 优点

* 抗负载能力强、是工作在网络4层之上仅作分发之用，没有流量的产生，这个特点也决定了它在负载均衡软件里的性能最强的，对内存和cpu资源消耗比较低。
* 配置性比较低，这是一个缺点也是一个优点，因为没有可太多配置的东西，所以并不需要太多接触，大大减少了人为出错的几率。
* 工作稳定，因为其本身抗负载能力很强，自身有完整的双机热备方案，如LVS+Keepalived。
* 无流量，LVS只分发请求，而流量并不从它本身出去，这点保证了均衡器IO的性能不会受到大流量的影响。
* 应用范围比较广，因为LVS工作在4层，所以它几乎可以对所有应用做负载均衡，包括http、数据库、在线聊天室等等。

#### LVS 缺点

* 软件本身不支持正则表达式处理，不能做动静分离；而现在许多网站在这方面都有较强的需求，这个是Nginx/HAProxy+Keepalived的优势所在。
* 如果是网站应用比较庞大的话，LVS/DR+Keepalived实施起来就比较复杂了，特别后面有 Windows Server的机器的话，如果实施及配置还有维护过程就比较复杂了，相对而言，Nginx/HAProxy+Keepalived就简单多了。

#### HAProxy优点

* HAProxy也是支持虚拟主机的。
* HAProxy的优点能够补充Nginx的一些缺点，比如支持Session的保持，Cookie的引导；同时支持通过获取指定的url来检测后端服务器的状态。
* HAProxy跟LVS类似，本身就只是一款负载均衡软件；单纯从效率上来讲HAProxy会比Nginx有更出色的负载均衡速度，在并发处理上也是优于Nginx的。
* HAProxy支持TCP协议的负载均衡转发，可以对MySQL读进行负载均衡，对后端的MySQL节点进行检测和负载均衡，大家可以用LVS+Keepalived对MySQL主从做负载均衡。
* HAProxy负载均衡策略非常多，HAProxy的负载均衡算法现在具体有如下8种：
  * roundrobin，表示简单的轮询，这个不多说，这个是负载均衡基本都具备的；
  * static-rr，表示根据权重，建议关注；
  * leastconn，表示最少连接者先处理，建议关注；
  * source，表示根据请求源IP，这个跟Nginx的IP_hash机制类似，我们用其作为解决session问题的一种方法，建议关注；
  * ri，表示根据请求的URI；
  * rl_param，表示根据请求的URl参数’balance url_param’ requires an URL parameter name；
  * hdr(name)，表示根据HTTP请求头来锁定每一次HTTP请求；
  * rdp-cookie(name)，表示根据据cookie(name)来锁定并哈希每一次TCP请求。

### 7. LVS 几种模式

1. DR模型 -- （Director Routing-直接路由）
2. NAT模型 -- (NetWork Address Translation-网络地址转换)
3. fullNAT -- （full NAT，双向数据包都进行SNAT与DNAT）
4. ENAT --（enhence NAT 或者叫三角模式/DNAT）
5. IP TUN模型 -- (IP Tunneling - IP隧道)

### 8. LVS负载均衡调度算法

* 轮询调度

轮询调度（Round Robin 简称’RR’）算法就是按依次循环的方式将请求调度到不同的服务器上，该算法最大的特点就是实现简单。轮询算法假设所有的服务器处理请求的能力都一样的，调度器会将所有的请求平均分配给每个真实服务器。

* 加权轮询调度

加权轮询（Weight Round Robin 简称’WRR’）算法主要是对轮询算法的一种优化与补充，LVS会考虑每台服务器的性能，并给每台服务器添加一个权值，如果服务器A的权值为1，服务器B的权值为2，则调度器调度到服务器B的请求会是服务器A的两倍。权值越高的服务器，处理的请求越多。

* 最小连接调度

最小连接调度（Least Connections 简称’LC’）算法是把新的连接请求分配到当前连接数最小的服务器。最小连接调度是一种动态的调度算法，它通过服务器当前活跃的连接数来估计服务器的情况。调度器需要记录各个服务器已建立连接的数目，当一个请求被调度到某台服务器，其连接数加1；当连接中断或者超时，其连接数减1。

（集群系统的真实服务器具有相近的系统性能，采用最小连接调度算法可以比较好地均衡负载。)

* 加权最小连接调度

加权最少连接（Weight Least Connections 简称’WLC’）算法是最小连接调度的超集，各个服务器相应的权值表示其处理性能。服务器的缺省权值为1，系统管理员可以动态地设置服务器的权值。加权最小连接调度在调度新连接时尽可能使服务器的已建立连接数和其权值成比例。调度器可以自动问询真实服务器的负载情况，并动态地调整其权值。

* 基于局部的最少连接

基于局部的最少连接调度（Locality-Based Least Connections 简称’LBLC’）算法是针对请求报文的目标IP地址的 负载均衡调度，目前主要用于Cache集群系统，因为在Cache集群客户请求报文的目标IP地址是变化的。这里假设任何后端服务器都可以处理任一请求，算法的设计目标是在服务器的负载基本平衡情况下，将相同目标IP地址的请求调度到同一台服务器，来提高各台服务器的访问局部性和Cache命中率，从而提升整个集群系统的处理能力。LBLC调度算法先根据请求的目标IP地址找出该目标IP地址最近使用的服务器，若该服务器是可用的且没有超载，将请求发送到该服务器；若服务器不存在，或者该服务器超载且有服务器处于一半的工作负载，则使用’最少连接’的原则选出一个可用的服务器，将请求发送到服务器。

* 带复制的基于局部性的最少连接

带复制的基于局部性的最少连接（Locality-Based Least Connections with Replication 简称’LBLCR’）算法也是针对目标IP地址的负载均衡，目前主要用于Cache集群系统，它与LBLC算法不同之处是它要维护从一个目标IP地址到一组服务器的映射，而LBLC算法维护从一个目标IP地址到一台服务器的映射。按’最小连接’原则从该服务器组中选出一一台服务器，若服务器没有超载，将请求发送到该服务器；若服务器超载，则按’最小连接’原则从整个集群中选出一台服务器，将该服务器加入到这个服务器组中，将请求发送到该服务器。同时，当该服务器组有一段时间没有被修改，将最忙的服务器从服务器组中删除，以降低复制的程度。

* 目标地址散列调度

目标地址散列调度（Destination Hashing 简称’DH’）算法先根据请求的目标IP地址，作为散列键（Hash Key）从静态分配的散列表找出对应的服务器，若该服务器是可用的且并未超载，将请求发送到该服务器，否则返回空。

* 源地址散列调度U

源地址散列调度（Source Hashing 简称’SH’）算法先根据请求的源IP地址，作为散列键（Hash Key）从静态分配的散列表找出对应的服务器，若该服务器是可用的且并未超载，将请求发送到该服务器，否则返回空。它采用的散列函数与目标地址散列调度算法的相同，它的算法流程与目标地址散列调度算法的基本相似。

* 最短的期望的延迟

最短的期望的延迟调度（Shortest Expected Delay 简称’SED’）算法基于WLC算法。举个例子吧，ABC三台服务器的权重分别为1、2、3 。那么如果使用WLC算法的话一个新请求进入时它可能会分给ABC中的任意一个。使用SED算法后会进行一个运算

A：（1+1）/1=2 B：（1+2）/2=3/2 C：（1+3）/3=4/3 就把请求交给得出运算结果最小的服务器。

* 最少队列调度

最少队列调度（Never Queue 简称’NQ’）算法，无需队列。如果有realserver的连接数等于0就直接分配过去，不需要在进行SED运算。

### 9. Nginx 调度算法

* 轮询 rr
按时间顺序逐一分配到不同的后端服务器

* 加权轮询 wrr
在nginx server后面配置权重，权重越高分配的概率越大

* ip_hash
每个请求按访问IP的hash分配，这样来自同一IP固定访问一个后端服务器

* least_hash
最少连接数，哪个机器连接数少就发给哪台机器

* url_hash
按访问的url的hash结果分配请求，是每个url定向到同一后端服务器上

* hash关键值
自定义hash的key

> 注: 调度算法在设置upstream中配置，例如在此大括号里写如ip_hash表示使用ip_hash的方式
> 1.轮询只是简单实现请求的顺序转发，并没有考虑不同服务器的性能差异；
> 2.加权轮询设置了初始时服务器的全站，但是没有考虑运行过程中的服务器状态
> 3.IP Hash保证同一客户端请求转发到同一后台服务器实现了session保存，然而当某一后台服务器发生故障时，某些客户端将访问失败；
> 4.最少连接数只是考虑了后端服务器的连接数情况，并没有完全考虑服务器的整体性能
