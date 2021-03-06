# learn_async

我们以用map函数操作一个list为例：

如果是单进程单线程，那就是逐个操作，肯定是同步的过程，由于没有并发，也没有阻塞、非阻塞的问题（可以认为肯定是阻塞）。

如果是并发（多线程或者多进程），就可能讨论同步、异步，也可以讨论阻塞、非阻塞。

同步的话，是多线程处理的结果最后汇总到主线程的时候，是按顺序来汇总，而异步的话，多线程处理的结果最后汇总到主线程的时候，是不讲究顺序的，谁先完成谁先汇总（或者每个结果都带有一个之前的标签，可以按照标签来调整顺序）。

阻塞的话，在子线程执行的时候，主线程是等着的，不往下执行的，而非阻塞的话，主线程继续执行，子线程的结果可以通过回调函数传给主线程。

同步、异步与阻塞、非阻塞的关系是这样：同步的话，一般是阻塞的，异步的话，可以是阻塞的，也可以是非阻塞的；阻塞的话，有可能是同步，也可能是异步，非阻塞的话，一定是异步的（也一定是并发的情况），所以，非阻塞是最高级的一种用法。

如果子程序就是多线程任务，母程序用多进程当然没问题，但是，如果母程序也用多线程，子线程的多线程就是在母线程的基础上另外多开子线程，比如子线程本身是2个线程的任务，母线程也是2个线程的任务，总的线程就是2乘以2等于4个线程。

在写程序时，可能会用到多线程（比如为了及时响应第二个请求，或者为了处理IO密集型任务），这时候必须考虑所用函数或数据结构的线程安全。所谓函数的线程安全，是因为函数中可能会改变全局变量或者改变传进来的参数。所谓数据结构的线程安全，指的是全局的对象、或者单例的线程安全。要保证数据结构（对象）的线程安全，一般来说，要使得只有通过类内的函数才能修改对象的属性（而不是直接能赋值给属性，也就是封装），这样的话，只要保证类内函数的线程安全，就能保证类的对象的线程安全。所谓一个对象的线程安全，指的就是即使把对象作为可供多个线程并发操作的全局变量，也是没问题的。

还有一个概念叫做函数的可重入，指的是是否读入全局变量，如果是读入全局变量的，就是不可重入的，因为万一中断在读入全局变量的那一行，重入时整个情况就完全变掉了。

在写程序时，未来该程序可能会变成一个子线程，所以就要尽量避免static，或者使用static final，还需要特别注意的是单例。单例之所以必要，是因为即使在一个线程中，某些对象也应该只创建一次来提高程序运行的效率。

在程序运行时，首先是类加载器运行，创建类对象，然后才是构造函数运行，创建具体的对象，如果是多线程，也是共用一个类加载器的。

celery之类的工具，其实提供的多进程，所以可以跑在多个机器上，构建集群cluster. redis, kafka之类的中间件，就是celery用来分派任务时用到的。

