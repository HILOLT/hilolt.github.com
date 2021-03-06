---
layout: post
keywords: blog
description: Java线程中sleep()、wait()和notify()和notifyAll()、suspend和resume()、yield()、join()、interrupt()的用法和区别
title: "Java线程中sleep()、wait()和notify()和notifyAll()、suspend和resume()、yield()、join()、interrupt()的用法和区别"
categories: [Archive]
tags: [Archive]
group: archive
icon: file-alt
---

从操作系统的角度讲，os会维护一个ready queue（就绪的线程队列）。并且在某一时刻cpu只为ready queue中位于队列头部的线程服务。
但是当前正在被服务的线程可能觉得cpu的服务质量不够好，于是提前退出，这就是yield。
或者当前正在被服务的线程需要睡一会，醒来后继续被服务，这就是sleep。
sleep方法不推荐使用，可用wait。
线程退出最好自己实现，在运行状态中一直检验一个状态，如果这个状态为真，就一直运行，如果外界更改了这个状态变量，那么线程就停止运行。
sleep()使当前线程进入停滞状态，所以执行sleep()的线程在指定的时间内肯定不会执行；yield()只是使当前线程重新回到可执行状态，所以执行yield()的线程有可能在进入到可执行状态后马上又被执行。
sleep()可使优先级低的线程得到执行的机会，当然也可以让同优先级和高优先级的线程有执行的机会；yield()只能使同优先级的线程有执行的机会。
当调用wait()后，线程会释放掉它所占有的“锁标志”，从而使线程所在对象中的其它synchronized数据可被别的线程使用。
waite()和notify()因为会对对象的“锁标志”进行操作，所以它们必须在 synchronized函数或synchronized　block中进行调用。如果在non-synchronized函数或non- synchronized　block中进行调用，虽然能编译通过，但在运行时会发生IllegalMonitorStateException的异常。

## 线程的状态
线程有四种状态，任何一个线程肯定处于这四种状态中的一种：
1)    产生（New）：线程对象已经产生，但尚未被启动，所以无法执行。如通过new产生了一个线程对象后没对它调用start()函数之前。
2)    可执行（Runnable）：每个支持多线程的系统都有一个排程器，排程器会从线程池中选择一个线程并启动它。当一个线程处于可执行状态时，表示它可能正处于线程池中等待排排程器启动它；也可能它已正在执行。如执行了一个线程对象的start()方法后，线程就处于可执行状态，但显而易见的是此时线程不一定正在执行中。
3)    死亡（Dead）：当一个线程正常结束，它便处于死亡状态。如一个线程的run()函数执行完毕后线程就进入死亡状态。
4)    停滞（Blocked）：当一个线程处于停滞状态时，系统排程器就会忽略它，不对它进行排程。当处于停滞状态的线程重新回到可执行状态时，它有可能重新执行。如通过对一个线程调用wait()函数后，线程就进入停滞状态，只有当两次对该线程调用notify或notifyAll后它才能两次回到可执行状态。

## 用法
sleep()
在指定的毫秒数内让当前正在执行的线程休眠（暂停执行），此操作受到系统计时器和调度程序精度和准确性的影响。

由于sleep()方法是Thread类的方法，因此它不能改变对象的机锁。所以当在一个Synchronized方法中调用sleep（）时，线程虽然休眠了，但是对象的机锁没有被释放，其他线程仍然无法访问这个对象。sleep()方法不需要在同步的代码块中执行。但是sleep()可以通过interrupt()方法打断线程的暂停状态，从而使线程立刻抛出InterruptedException。

wait()和notify()和notifyAll()
wait()方法则会在线程休眠的同时释放掉机锁，其他线程可以访问该对象。wait()必须在同步的代码块中执行。当一个线程执行到wait()方法时，它就进入到一个和该对象相关的等待池中，同时失去了对象的机锁，可以允许其它的线程执行一些同步操作。但是wait()可以通过interrupt()方法打断线程的暂停状态，从而使线程立刻抛出InterruptedException。

wait可以让同步方法或者同步块暂时放弃对象锁,而将它暂时让给其它需要对象锁的人(这里应该是程序块,或线程)用,这意味着可在执行wait()期间调用线程对象中的其他同步方法!在其它情况下(sleep啊,suspend啊),这是不可能的.但是注意我前面说的,只是暂时放弃对象锁,暂时给其它线程使用,我wait所在的线程还是要把这个对象锁收回来的呀.wait什么?就是wait别人用完了还给我啊！

好,那怎么把对象锁收回来呢?

第一种方法,限定借出去的时间.在wait()中设置参数,比如wait(1000),以毫秒为单位,就表明我只借出去1秒中,一秒钟之后,我自动收回.

第二种方法,让借出去的人通知我,他用完了,要还给我了.这时,我马上就收回来.哎,假如我设了1小时之后收回,别人只用了半小时就完了,那怎么办呢?靠!当然用完了就收回了,还管我设的是多长时间啊.

那么别人怎么通知我呢?相信大家都可以想到了,notify(),这就是最后一句话"而且只有在一个notify()或notifyAll()发生变化的时候，线程才会被唤醒"的意思了.

notify()唤醒在此对象监视器上等待的单个线程。当它被一个notify()方法唤醒时，等待池中的线程就被放到了锁池中。该线程将等待从锁池中获得机锁，然后回到wait()前的中断现场。

notifyAll()唤醒在此对象监视器上等待的所有线程。


suspend和resume()
join()
join()方法使当前线程停下来等待，直至另一个调用join方法的线程终止。值得注意的是，线程的在被激活后不一定马上就运行，而是进入到可运行线程的队列中。但是join()可以通过interrupt()方法打断线程的暂停状态，从而使线程立刻抛出InterruptedException。

yield()
Yield()方法是停止当前线程，让同等优先权的线程运行。如果没有同等优先权的线程，那么Yield()方法将不会起作用。

interrupt()
interrupt()中断线程。需要注意的是，InterruptedException是线程自己从内部抛出的，并不是interrupt()方法抛出的。对某一线程调用interrupt()时，如果该线程正在执行普通的代码，那么该线程根本就不会抛出InterruptedException。但是，一旦该线程进入到wait()/sleep()/join()后，就会立刻抛出InterruptedException。


## 各个方法之间的区别
线程方法名称					sleep()		wait()		suspend		resume()	join()
是否释放同步锁		
是否需要在同步的代码块中调用	
方法是否已废弃	
是否可以被中断
sleep()	否	否	否	是
wait()	是	是	否	是
suspend	 	 	是	 
resume()	 	 	是	 
join()	 	 	否	是

