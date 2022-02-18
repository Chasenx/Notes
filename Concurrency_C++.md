# C++多线程编程

## 基本库

- `include <thread>`

- `include <mutex>`

- `include <future>`


## 线程基本代码

```c++
void function1() {
    cout << "I am function1" << endl;
}
void function2() {
    cout << "I am function2" << endl;
}
int main() {
    thread t1(function1);
    thread t2(function2);
    t1.join();   // 等待子线程执行完毕
    t2.join();
    // t1.detach(); // 放飞子线程
    return 0;
}
```

- 主要有 `join()` 和 `detach()` 这两个函数，前者等待结束，后者相当于分离，主线程一定要有这两个中的一个，要不会出错
- 多线程编程，同步和互斥是最主要的两个问题，例如 `cout` 、`cin` 这种互斥资源，没有加锁的话会乱七八糟
- 如果 `join()` 之前的代码出错了，会影响到后面的线程，一般会对异常进行处理
- `thread` 传递参数的时候是值传递，要使用引用的时候应该使用 `ref()` 进行包装

## Functor-可调用对象

- 一个类重载了 `()` 运算符，这个类构造的对象可以当成一个函数来处理，和函数的区别是这个函数有了更多的状态，猜测和闭包有关

```c++
class Fctor {
public:
	void operator()(string& msg) {
		cout << "ttt say: " << msg << endl;
		msg = "hello world";	
	}
};
// 调用过程
Fctor a = Fctor();
a();
```

 ## 使用 Mutex

- 使用 `mutex`的 `lock()` 和 `unlock()`，但是这种方法当代码里面出错的时候，会一直锁定资源，无法让其他程序访问资源

 ```c++
 #include <mutex>
 mutex m_mutex;
 void function() {
     mutex.lock();
     // do something
     mutex.unlock();
 }
 ```

- 使用 `lock_guard<std::mutex>`，会自动加锁和解锁

```c++
class LogFile {
	mutex m_mutex;
	ofstream f;
public:
	LogFile() {
		f.open("log.txt");
	}
	void shared_print(int id, string msg) {
		std::lock_guard<mutex> locker(m_mutex);
		f << "From: " << id << ": " << msg << endl;
	}
};
```

- 使用 `unique_lock<std::mutex>`

```c++
void shared_print(int id, string msg) {
    std::unique_lock<mutex> locker(m_mutex);
    f << "From: " << id << ": " << msg << endl;
    locker.unlock();
    //...
    locker.lock();
    // 好处就是可以随便加锁解锁，还安全
}
```

## 死锁问题

- 有多个资源互斥的时候可能会出现死锁，锁定时要按照一定顺序
- 使用 `lock_guard<mutex> locker(m_mutex, adopt_lock)`，`adopt_lock`让系统自动来选择锁的顺序
- 锁定资源后避免调用用户的函数，这些函数可能会锁定别的资源

## 对文件打开操作加锁

- 可以使用一个信号量来锁定这个操作

- 使用 `once_flag`

```c++
once_flag _flag;
// 调用过程
call_once(_flag, function); // function 可以使用一个 lambda 函数 --> [&]() {//instruction;}
```

  ## Condition

- 适用于生产者消费者问题，然程序自动唤醒， 解决空等待问题

- 这部分内容有点复杂

```c++
// 生产者与消费者模型
queue<int> q;
mutex queue_mutex;
condition_variable cond;

// producer
void function3() {
	int count = 10;
	while (count > 0) {
		unique_lock<mutex> locker(queue_mutex);
		q.push(count);
		locker.unlock();
		cond.notify_one();  // 唤醒消费者
		this_thread::sleep_for(chrono::seconds(1));  // 延时等待
		count--;
	}
}
//consumer
void function4() {
	int data = 0;
	while (data != 1) {
		unique_lock<mutex> locker(queue_mutex);
		cond.wait(locker, []() {return !q.empty(); });
		data = q.front();
		q.pop();
		locker.unlock();
		cout << "t2 got a value from t1:" << data << endl;
	}
}
```

## Future 和 Promise

- `future` 用于对子线程数据的获取，获取函数的返回值

````c++
#include<future>
future<int> fu = async(factorial, 4);
int x = fu.get();
````

- 使用 `promise` 先传递给子线程一个 `future`，子进程会一直被阻塞直到获得主线程给`promise` 赋值

```c++
int factorial(future<int>& f) {
	int res = 1;
	int N = f.get();
	for (int i = N; i > 1; i--) {
		res *= i;
	}
	cout << "result is:" << res << endl;
	return res;
}
// 调用过程
promise<int> p;
future<int> f = p.get_future();
future<int> fu = async(launch::async, factorial, ref(f));
//do something else
p.set_value(4);
int x = fu.get();
```

- 有时候主线程会给不出 `promise` 的值，而且是确定给不出的，这时候可以抛出一个异常

```c++
p.set_exception(make_exception_ptr(runtime_error("To err is human")));
```

- `shared_future` 可以共享，复制，传递给 `async` 函数的时候就不用先转成引用

## Packaged_task

- 用于将一个函数和对应的参数打包成一个任务，这个任务可以在别的函数、类、线程里面被执行
- 和 bind() 的最大区别是可以链接到一个 `future`，通过这个 `future` 获取函数的返回值

```c++
deque<packaged_task<int()>> task_queue;
mutex pack_mu;
condition_variable cond_queue;

void thread_1() {
	packaged_task<int()> t;
	{
		unique_lock<mutex> locker(pack_mu);
		cond_queue.wait(locker, []() {return !task_queue.empty(); });
		t = move(task_queue.front());
		task_queue.pop_front();
	}
	t();
}

thread t1(thread_1);
packaged_task<int()> t(bind(factorial, 6)); //产生一个任务
future<int> fu = t.get_future();
{
    lock_guard<mutex> locker(pack_mu);
    task_queue.push_back(move(t));
}
cond_queue.notify_one();
cout << fu.get() << endl;
t1.join();
```

