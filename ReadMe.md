### python多线程

##### 创建线程的两种方法

+ 直接使用`threading.Thread`

```python
import threading

def run_other_thread():
    print(threading.currentThread().getName())

if __name__ == '__main__':
    thread1 = threading.Thread(target=run_other_thread,name="new thread")
    thread1.start()
```

+ 继承`threading.Thread`

```python
import threading

class NewThread(threading.Thread):

    def __init__(self, thread_name):
        super().__init__(name=thread_name)

    def run(self):
        print(threading.currentThread().getName())


if __name__ == '__main__':
    thread1 = NewThread("thread class")
    thread1.start()
    thread1.join()
```


#####  线程同步的方法

+ 使用Lock

```python
from threading import Lock
local = Lock()
local.acquire()
# do something
local.release()
```

**必须成对调用**，不然会造成死锁，并且`local.acquire()`不能连续调用两次，不然也会死锁

+ 使用RLock

RLock于Lock类似。不同之处在于，RLock可以连续多次调用`acquire()`方法而不会死锁。

```python
from threading import RLock
local = RLock()
local.acquire()
#do something
local.acquire()
#do something
local.release()
local.release()
```
同样的，**acquire与release必须成对调用**

在上面我们是使用手动的方式调用Lock以及RLock的acquire以及release方法，其实我们也可以使用with语法

```
from threading import Lock
local = RLock()
#自动调用acquire/release
with local:
    time.sleep(3)
    print(threading.currentThread().getName())
```

+ 使用Condition

Condition类似于Java中Object对象自带的wait以及notify方法。可以用来实现生产者/消费者模型

```python
from threading import Condition

condition = Condition()
    with condition:
    	#wait others call notify
        condition.wait()
        
#notify wait thread resume
condition.notify()
```

+ 使用 Semaphore

类似于操作系统原理里面的互斥信号量，可以用来控制同时访问某资源或者某操作的总数

```
from threading import Semaphore

# 信号量为5
sem = Semaphore(5)
# 信号量-1 当信号量为0 锁住
sem.acquire()
# 信号量+1
sem.release()

#其实我们也可以使用这种方法！！！
#sem = Semaphore(5)
#with sem:
#	pass
```











