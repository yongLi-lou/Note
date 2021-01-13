# c# 锁的使用

## 1 互斥锁lock（基于Monitor实现）

定义：

 ```c#
private static readonly object Lock = new object();
 ```



使用：

```c#
lock (Lock)
{
　　//todo
}
```

作用：将会锁住代码块的内容，并阻止其他线程进入该代码块，直到该代码块运行完成，释放该锁。

**注意：定义的锁对象应该是 私有的，静态的，只读的，引用类型的对象，这样可以防止外部改变锁对象**

## 2 互斥锁Monitor

定义：

```c#
private static readonly object Lock = new object();
```

使用：

```c#
Monitor.Enter(Lock);
//todo
Monitor.Exit(Lock);
```

作用：将会锁住代码块的内容，并阻止其他线程进入该代码块，直到该代码块运行完成，释放该锁。

**注意：定义的锁对象应该是 私有的，静态的，只读的，引用类型的对象，这样可以防止外部改变锁对象**

<font color="red">Monitor有TryEnter的功能，可以防止出现死锁的问题，lock没有</font>

## 3 互斥锁Mutex

定义：

```c#
private static readonly Mutex mutex = new Mutex();
```

使用：

```c#
mutex.WaitOne();
//todo
mutex.ReleaseMutex();
```

作用：将会锁住代码块的内容，并阻止其他线程进入该代码块，直到该代码块运行完成，释放该锁。

**注意：定义的锁对象应该是 私有的，静态的，只读的，引用类型的对象，这样可以防止外部改变锁对象**

<font color="red">Mutex本身是可以系统级别的，所以是可以跨越进程的</font>

## 4 读写锁ReaderWriterLockSlim 

定义：

```c#
private static readonly ReaderWriterLockSlim LockSlim = new ReaderWriterLockSlim();
```

使用：

写锁

```c#
try
{
LockSlim.EnterWriteLock();

//todo

}
catch (Exception ex)
{
}
finally
{
LockSlim.ExitWriteLock();
}
```



 

读锁

```c#
try
{
LockSlim.EnterReadLock();

}
catch (Exception ex)
{
}
finally
{
LockSlim.ExitReadLock();
}
```



基本规则：  读读不互斥 读写互斥 写写互斥

作用：当同一个资源被多个线程读，少个线程写的时候，使用读写锁

引用：https://blog.csdn.net/weixin_40839342/article/details/81189596 

问题： 既然读读不互斥，为何还要加读锁

答：   如果只是读，是不需要加锁的，加锁本身就有性能上的损耗

​      如果读可以不是最新数据，也不需要加锁

​      如果读必须是最新数据，必须加读写锁

​      读写锁相较于互斥锁的优点仅仅是允许读读的并发，除此之外并无其他。

注意：不要使用ReaderWriterLock，该类有问题

 