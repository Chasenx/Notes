# Python笔记八 （进程和线程编程）

## 1. `fork()` 调用

`Unix` / `Linux` 系统可以使用系统提供的函数 `fork()` 来创建新的进程

## 2. `multiprocessing` 模块实现多进程

使用 `Process` 类来创建一个子进程，创建子进程时，只需要传入一个执行函数和函数的参数，创建一个 `Process实例` ，用 `start()` 方法启动，`join()` 方法可以等待子进程结束后再继续往下运行，通常用于进程间的同步

```python
from multiprocessing import Process
import os

# 子进程要执行的代码
def run_proc(name):
    print('Run child process %s (%s)...' % (name, os.getpid()))

if __name__=='__main__':
    print('Parent process %s.' % os.getpid())
    p = Process(target=run_proc, args=('test',))
    print('Child process will start.')
    p.start()
    p.join()
    print('Child process end.')
```

当要启动大量的子进程的时候，可以用进程池的方式批量创建子进程

```python
from multiprocessing import Pool
import os, time, random

def long_time_task(name):
    print('Run task %s (%s)...' % (name, os.getpid()))
    start = time.time()
    time.sleep(random.random() * 3)
    end = time.time()
    print('Task %s runs %0.2f seconds.' % (name, (end - start)))

if __name__=='__main__':
    print('Parent process %s.' % os.getpid())
    p = Pool(4)
    for i in range(5):
        p.apply_async(long_time_task, args=(i,))
    print('Waiting for all subprocesses done...')
    p.close()
    p.join()
    print('All subprocesses done.')
```

使用 `subprocess` 模块来启动一个子进程，同时可以控制其输入和输出

```py
# 得到子进程的输出结果
import subprocess

print('$ nslookup www.python.org')
r = subprocess.call(['nslookup', 'www.python.org'])
print('Exit code:', r)
```

如果子进程还需要输入，则可以通过 `communicate()` 方法输入

```python
import subprocess

print('$ nslookup')
p = subprocess.Popen(['nslookup'], stdin=subprocess.PIPE, stdout=subprocess.PIPE, stderr=subprocess.PIPE)
output, err = p.communicate(b'set q=mx\npython.org\nexit\n')
print(output.decode('utf-8'))   # windows下面要改成gbk
print('Exit code:', p.returncode)
```

## 3. 进程间的通信

进程间的通信使用 `Queue` 、`Pipes` 等方式来交换数据

```python
from multiprocessing import Process, Queue
import os, time, random

# 写数据进程执行的代码:
def write(q):
    print('Process to write: %s' % os.getpid())
    for value in ['A', 'B', 'C']:
        print('Put %s to queue...' % value)
        q.put(value)
        time.sleep(random.random())

# 读数据进程执行的代码:
def read(q):
    print('Process to read: %s' % os.getpid())
    while True:
        value = q.get(True)
        print('Get %s from queue.' % value)

if __name__=='__main__':
    # 父进程创建Queue，并传给各个子进程：
    q = Queue()
    pw = Process(target=write, args=(q,))
    pr = Process(target=read, args=(q,))
    # 启动子进程pw，写入:
    pw.start()
    # 启动子进程pr，读取:
    pr.start()
    # 等待pw结束:
    pw.join()
    # pr进程里是死循环，无法等待其结束，只能强行终止:
    pr.terminate()
```

## 4. 多线程编程

Python的标准库提供了两个模块： `_thread` 和 `threading` ， `_thread` 是低级模块， `threading` 是高级模块，对 `_thread` 进行了封装

```python
import time, threading

# 新线程执行的代码:
def loop():
    print('thread %s is running...' % threading.current_thread().name)
    n = 0
    while n < 5:
        n = n + 1
        print('thread %s >>> %s' % (threading.current_thread().name, n))
        time.sleep(1)
    print('thread %s ended.' % threading.current_thread().name)

print('thread %s is running...' % threading.current_thread().name)
t = threading.Thread(target=loop, name='LoopThread')
t.start()
t.join()
print('thread %s ended.' % threading.current_thread().name)
```

创建新的线程，都会有一个线程的名字，可以自己指定

## 5. Lock

可以给某个函数加锁，使得这一段代码不能同时被不同的进程执行，要记得上锁后要释放锁

```python
import threading

balance = 0
lock = threading.Lock()

def run_thread(n):
    for i in range(100000):
        # 先要获取锁:
        lock.acquire()
        try:
            # 放心地改吧:
            change_it(n)
        finally:
            # 改完了一定要释放锁:
            lock.release()
```

## 6. ThreadLocal ()

`ThreadLocal` 相当于给每个进程都提供一个局部变量，这样就不需要总是将一个变量一直传给子函数

```python
import threading

# 创建全局ThreadLocal对象:
local_school = threading.local()

def process_student():
    # 获取当前线程关联的student:
    std = local_school.student
    print('Hello, %s (in %s)' % (std, threading.current_thread().name))

def process_thread(name):
    # 绑定ThreadLocal的student:
    local_school.student = name
    process_student()

t1 = threading.Thread(target= process_thread, args=('Alice',), name='Thread-A')
t2 = threading.Thread(target= process_thread, args=('Bob',), name='Thread-B')
t1.start()
t2.start()
t1.join()
t2.join()


Hello, Alice (in Thread-A)
Hello, Bob (in Thread-B)
```

## 7. 分布式进程

分布式进程需要一个进程来进行分配任务，然后其他的进程执行任务，同时返回结果。

使用 `managers` 子模块可以把多进程分布到多台机器上

