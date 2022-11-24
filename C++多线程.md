# C++多线程

# C++11之前示例

W3school：https://wizardforcel.gitbooks.io/w3school-cpp/content/Text/84.html

https://www.cnblogs.com/skynet/archive/2010/10/30/1865267.html

三个线程顺序执行：pthread_join传入参数来接受pthread_exit传出的参数，

**pthread_join**用于等待一个线程的结束，也就是主线程中要是加了这段代码，就会在加代码的位置卡住，直到这个线程执行完毕才往下走。

**pthread_exit**用于强制退出一个线程（非执行完毕退出），一般用于线程内部。

**结合用法：**

一般都是pthread_exit在线程内退出，然后返回一个值。这个时候就跳到主线程的pthread_join了（因为一直在等你结束），这个返回值会直接送到pthread_join，实现了**主与分线程的通信**。

```cpp
#include <iostream>
#include <cstdlib>
#include <pthread.h>

using namespace std;

#define NUM_THREADS     5

void *PrintHello(void *threadid)
{
	long tid;
	tid = (long)threadid;
	cout << "Thread ID, " << tid << endl;
	pthread_exit(NULL);
}

void *thread1(void* s){
	cout<<"this is thread1"<<endl;
	printf("s=%s\n", (char*)s);
	pthread_exit((void*)"Thread1 is returning");
}

void *thread2(void* s){
	//int *a= new(2);
	cout<<"this is thread2"<<endl;
	//printf("a=%d\n", *a);
	pthread_exit((void*)2);
}
int main ()
{
	pthread_t t1, t2, t3;
	int rc;
	void *a1, *a2, *a3;
	int i=0;
	cout << "main() : creating thread, " << i << endl;
	rc = pthread_create(&t1, NULL, PrintHello, (void *)0);
	if (rc){
		cout << "Error:unable to create thread0," << rc << endl;
		exit(-1);
	}
	pthread_join(t1, &a1);
	cout<<"a1="<<endl;//cout<<*(int*)a1<<endl;
	rc=pthread_create(&t2, NULL, thread1, (void*)"hehehe");
	if(rc){
		cout << "Error:unable to create thread1," << rc << endl;
		exit(-1);
	}
	pthread_join(t2, &a2);
	printf("a2=%s\n", (char*)a2);
	rc=pthread_create(&t3, NULL, thread2, (void*)666);
	if(rc){
		cout << "Error:unable to create thread2," << rc << endl;
		exit(-1);
	}
	pthread_join(t3, &a3);
	cout<<"a3="<<(int*)a3<<endl;
	//delete a3;
	pthread_exit(NULL);
}
```

## 示例

创建五个线程并且分别join，用于回收资源，如果是detach的线程资源由系统自动回收，即自生自灭，joinable()函数可以判断该线程是否处于分离状态(bool).

```c++
#include <iostream>
#include <pthread.h>
#include <unistd.h>
using namespace std;
#define NUM_THREAD 5
void* my(void *args){
    cout<<"start..........    ";
    //pthread_exit(0);
    sleep(1);//让线程执行慢一点，太快会乱顺序
    int tid= *((int*)args);
    cout<<"tid="<<tid<<endl;
    pthread_exit(NULL);
    return NULL;
}
int main(){
    pthread_t my1[NUM_THREAD];
    int index[5];
    pthread_attr_t attr;
    void* status;
    // 初始化线程是可连接的
    pthread_attr_init(&attr);
    pthread_attr_setdetachstate(&attr, PTHREAD_CREATE_JOINABLE);
    for(int i=0; i<NUM_THREAD; i++){
	    index[i]=i;
        int ret = pthread_create(&my1[i], NULL, my, (void*)&(index[i]));
        if(ret!=0){
            cout<<"create error, ret="<<ret<<endl;
        }
    }
    // 删除属性，并等待线程
    pthread_attr_destroy(&attr);
    for(int i=0; i<NUM_THREAD; i++){
    	int ret = pthread_join(my1[i], &status);
    	if(ret){
    		cout<<"unable to join"<<endl;
    		exit(-1);
    	}
    	cout<<"completed tid="<<i<<endl;;
    	cout<< "exiting with status: "<<status<<endl;
    }
    
    pthread_exit(NULL);
    return 0;
}
```

# C++11的多线程

https://paul.pub/cpp-concurrency/#id-macos

## C++11的thread库

**call_once函数**，在多线程下面让一个语句只能执行一次，

template <class Fn, class... Args>

​    void call_once (once_flag& flag, Fn&& fn, Args&&...args);//一个once_flag对象只能被执行一次

需要包含头文件：<mutex>

用途：

std::call_once主要用于多线程的只初始化一次资源的情景，典型的例子就是**完美的解决线程安全的单例模式。**

**std::thread::native_handle**

native_handle_type native_handle();    (since C++11) (not always present)

Returns the implementation defined underlying thread handle. Uses `native_handle` to enable realtime scheduling of C++ threads on a POSIX system，比如返回一个pthread_t的tid，这个在C++11中的thread库是没有的，这样可以获得更高的执行权限，即可以利用以前的底层操作

## 如何保证线程安全

1、volatile

2、原子操作

3、线程同步（锁，mutex）：访问之前加锁，访问之后解锁

### mutex:

mutex：

#include <mutex>，mutex类：lock(), unlock(), trylock();

trylock:如果没有加锁成功，就会立即返回，不会阻塞等待

timed_mutex: 

带超时机制的锁，try_lock_for(时间长度)，try_lock_util(时间点)。

recursive_mutex：

递归互斥锁，可以解决同一线程多次加锁造成的死锁问题。在一个线程中，一个地方加了锁，再去申请锁就会造成死锁。

recursive_timed_mutex:

带超时机制的递归互斥锁。

### lock_guard：

构造函数的参数是Mutex。

简化互斥锁的使用，在构造函数中加锁，在析构函数中解锁。采用了RALL思想（在类构造函数中分配资源，在析构函数中释放资源，保证资源在离开作用域时自动释放)。

```c++
#include <iostream>
#include <thread>
#include <unistd.h>
#include <mutex>
using namespace std;
int a=0;
mutex mtx;
void func(){
//    mtx.lock();
    //cout<<"this task is finished!!!";
    for(int i=0; i<1000000; i++){
        //sleep(1);
	lock_guard<mutex> mlock(mtx);
	a++;
    }
//    mtx.unlock();
}//这里才会执行lock_guard的析构函数。
int main(){
    thread t1(func);
    thread t2(func);
    cout<<"tid="<<t1.get_id();
    //for(int i=0; i<10; i++){
	//cout<<"i="<<i<<endl;
	//sleep(1);
    //}
    t1.join();
    t2.join();
    cout<<"a="<<a<<endl;
    return 0;
}
```

### unipue_lock

https://blog.csdn.net/fengbingchun/article/details/78638138

与lock_guard类似，都是对mutex的一个封装，都是**采用对象独占所有权**的方式来管理mutex，即在unique_lock生命周期内，这个锁一直有效，直到其生命周期结束，并且都是禁止拷贝构造。

与lock_guard不同的是，这个锁支持移动语义（moveable），允许移动构造。

.wait有两种用法：

void wait( std::unique_lock\<std::mutex>& lock );

template< class Predicate >

void wait( std::unique_lock\<std::mutex>& lock, Predicate stop_waiting );//Predicate 断言

第二种用法相当于:

```c++
while(!stop_waiting()){
    wait(lock);
}
```



### 条件变量

当条件不满足时，就会一直阻塞，需要和互斥锁一起使用，常见的就是生产消费者模型（高速缓冲队列）

#include <condition_variable>

方法m_cond.notify_one()//唤醒一个线程去处理因为这个条件不满足而阻塞的线程

方法m_cond.notify_all()

wait():1、将互斥锁加锁，2、阻塞等待线程被唤醒，3、给互斥锁解锁。

让两个线程交替打印1，2，利用两个条件变量

```c++
#include <iostream>
#include <pthread.h>
#include <condition_variable>
#include <thread>
#include <mutex>
#include <unistd.h>

using namespace std;
mutex m;
std::condition_variable cv1, cv2;
bool c=true;
#define NUM_THREAD 5
//class Print{
//public:
void printA(){
    for(int i=0; i<7; i++) {
        std::unique_lock<std::mutex> lk(m);
        cv1.wait(lk, [] { return c; });
        sleep(1);
        cout << 1 << " ";
        c = false;
        cv2.notify_one();
    }
}
void printB(){
    for(int i=0; i<7; i++) {
        std::unique_lock<std::mutex> lk(m);
        cv2.wait(lk, [] { return !c; });
        cout << 2 << endl;
        sleep(0.1);
        c = true;
        cv1.notify_one();
    }
}

void my(){
    cout<<"start.........."<<endl;
    //pthread_exit(0);
    return ;
}
int main(){
    //thread t1(my);
    //Print a=new Print();
    thread t2(printA);
    thread t3(printB);

    t2.join();
    t3.join();
//    signal(SIGINT, []{my();});
    //t1.join();
    return 0;
}
```

## thread::hardware_concurrency()

`thread::hardware_concurrency()`可以获取到当前硬件支持多少个线程并行执行。



https://blog.csdn.net/c_base_jin/article/details/89761718

https://zhuanlan.zhihu.com/p/444375447

https://zhuanlan.zhihu.com/p/367309864

https://en.cppreference.com/w/cpp/utility/move



https://blog.csdn.net/yesyes120/article/details/78944991

## 守护进程

https://cloud.tencent.com/developer/article/1868252

守护进程（Daemon）是一种在后台运行，并且生存周期很长的进程，它周期性的执行某种任务或者等待某个事件的发生。

特点：

* 长期运行
* 与控制终端分离

在 Linux 下使用 `ps -ajx`来查看所有的进程，
