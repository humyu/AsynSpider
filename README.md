# AsynSpider
## python并发与异步

#### 阻塞
- 程序未得到所需计算资源时被挂起的状态。程序在等待某个操作完成期间，自身无法继续干别的事情，则称该程序在该操作上是阻塞的。

#### 非阻塞
- 程序在等待某操作过程中，自身不被阻塞，可以继续运行干别的事情，则称该程序在该操作上是非阻塞的。非阻塞的存在是因为阻塞存在，
  正因为某个操作阻塞导致的耗时与效率低下，我们才要把它变成非阻塞，以提高效率。
  
#### 同步
- 不同程序单元为了完成某个任务，在执行过程中需靠某种通信方式以协调一致，称这些程序单元是同步执行的。

#### 异步
- 不同程序单元之间过程中无需通信协调，也能完成任务的方式，不相关的程序单元之间可以是异步的。

### IO密集型:CPU经常等待IO
- 网络后台服务
- 网络爬虫
  - 多协程
  - 多线程
  
### CPU密集型：计算密集型，CPU计算为主
- 加密解密
  - 使用多进程
  
### 全局解释器锁GIL
- 即使使用了多线程，同一时刻也只有单个线程使用CPU，导致多核CPU的浪费
- GIL只会对CPU密集型的程序产生影响

### 线程池
- 由于在切换线程的时候，需要切换上下文环境，依然会造成cpu的大量开销。
- 预先创建好一个较为优化的数量的线程，让过来的任务立刻能够使用，就形成了线程池。
- 从Python3.2开始，标准库为我们提供了 concurrent.futures 模块，它提供了 ThreadPoolExecutor (线程池)和ProcessPoolExecutor (进程池)两个类。
- 原理：提前创建好线程/进程放在池子里，新的task到来可以重用这些资源，减少了新建、终止线程/进程的开销
- 提升性能：因为减去了大量新建、终止线程的开销，重用了线程资源  
- 适用场景：适合处理突发性大量请求或需要大量线程完成任务、但实际任务处理时间较短
- 防御功能：能有效避免系统因为创建线程过多，而导致系统负荷过大响应变慢等问题
- 代码简洁：使用线程池的语法比自己新建线程执行线程更加简洁


### 协程
- 协程是一种用户态的轻量级线程。拥有自己的寄存器上下文和栈。协程调度切换时，将寄存器上下文和栈保存到其他地方，在切回来的时候，恢复先前保存的寄存器上下文和栈。
- 协程本质上是个单进程，协程相对于多进程来说，无需线程上下文切换的开销，无需原子操作锁定及同步的开销，编程模型也非常简单。
- 单线程实现并发
- IO多路复用

#### 协程爬虫
- 网络爬虫场景下，我们发出一个请求之后，需要等待一定的时间才能得到响应，但其实在这个等待过程中，程序可以干许多其他的事情，等到响应得到之后才切换回来继续处理，
  这样可以充分利用 CPU 和其他资源，这就是异步协程的优势

### 多任务爬虫的数据解析
- 一定要是用任务对象的回调函数实现数据解析
- 多任务的架构中数据的爬取是封装在特殊函数中的，我们一定要保证数据请求结束后，在实现数据解析
- 使用多任务的异步协程爬取数据实现套路：
  - 可以先试用request模块将待请求的数据对应的url封装到urls列表中【同步】
  - 可以使用aiohttp模块将列表中的url进行异步的请求和数据解析【异步】
  

  