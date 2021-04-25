---
title: OO-Unit2-总结
date: 2021-04-24 10:08:50
tags: [BUAA-OO-2021]
---

OO第二单元电梯调度总结与分析。

# OO-Unit2-总结

## 一、程序结构分析

### 第五次作业

#### 代码可视化与数据统计

##### 程序类图

<img src="https://fzc-1300590701.cos.ap-nanjing.myqcloud.com/blogImages/OO-Unit2/hw5.png" style="zoom:80%;" />

由于本次只有一台电梯，因此没有使用调度器，而是只设计了一个策略类`Scheduler`类来实现每到一个楼层根据当前等待的乘客和电梯中的乘客请求来确定目标楼层的功能。而电梯类和输入线程类则是本次作业的两个线程类，分别对应消费者和生产者。

##### 程序复杂度分析

* 类复杂度

| Class       | OCavg | OCmax | WMC  |
| ----------- | ----- | ----- | ---- |
| Elevator    | 4.5   | 7     | 18   |
| InitQueue   | 1     | 1     | 7    |
| InputThread | 2     | 3     | 4    |
| Main        | 2     | 2     | 2    |
| Scheduler   | 8     | 16    | 40   |
| WaitQueue   | 1     | 1     | 7    |

可以看出，在`Elevator`类和`Scheduler`类中代码复杂度较高。两者构成了电梯的核心逻辑，包含较多分支。

* 方法复杂度

| Method                                           | CogC | ev(G) | iv(G) | v(G) |
| ------------------------------------------------ | ---- | ----- | ----- | ---- |
| Elevator.Elevator(WaitQueue,InitQueue,String)    | 0    | 1     | 1     | 1    |
| Elevator.move(int)                               | 6    | 3     | 3     | 5    |
| Elevator.personFlow()                            | 11   | 1     | 8     | 8    |
| Elevator.run()                                   | 25   | 4     | 11    | 11   |
| InitQueue.InitQueue()                            | 0    | 1     | 1     | 1    |
| InitQueue.addRequest(PersonRequest)              | 0    | 1     | 1     | 1    |
| InitQueue.clearQueue()                           | 0    | 1     | 1     | 1    |
| InitQueue.close()                                | 0    | 1     | 1     | 1    |
| InitQueue.getRequests()                          | 0    | 1     | 1     | 1    |
| InitQueue.isEnd()                                | 0    | 1     | 1     | 1    |
| InitQueue.noWaiting()                            | 0    | 1     | 1     | 1    |
| InputThread.InputThread(WaitQueue,ElevatorInput) | 0    | 1     | 1     | 1    |
| InputThread.run()                                | 7    | 3     | 4     | 4    |
| Main.main(String[])                              | 5    | 1     | 5     | 5    |
| Scheduler.Scheduler(WaitQueue,InitQueue,String)  | 0    | 1     | 1     | 1    |
| Scheduler.getDesFloor(int)                       | 49   | 13    | 15    | 21   |
| Scheduler.getFloor(int,int)                      | 26   | 7     | 3     | 15   |
| Scheduler.getMorningFloor(int,int)               | 13   | 4     | 2     | 8    |
| Scheduler.getNightFloor(int,int)                 | 13   | 4     | 2     | 8    |
| WaitQueue.WaitQueue()                            | 0    | 1     | 1     | 1    |
| WaitQueue.addRequest(PersonRequest)              | 0    | 1     | 1     | 1    |
| WaitQueue.clearQueue()                           | 0    | 1     | 1     | 1    |
| WaitQueue.close()                                | 0    | 1     | 1     | 1    |
| WaitQueue.getRequests()                          | 0    | 1     | 1     | 1    |
| WaitQueue.isEnd()                                | 0    | 1     | 1     | 1    |
| WaitQueue.noWaiting()                            | 0    | 1     | 1     | 1    |

可以看到，`getDesFloor、getFloor`方法和电梯类中的`run`方法由于需要处理电梯的运行，调用了很多其他方法和变量，复杂度较高。

##### 程序行数统计

![](https://fzc-1300590701.cos.ap-nanjing.myqcloud.com/blogImages/OO-Unit2/hw5line.png)

与之前代码复杂度分析保持一致，仍然是`Elevator`类和`Scheduler`类的行数最多。总有效代码函数为423行。

#### 代码分析

##### 原理分析

本次作业定义了一个`waitqueue`作为生产者消费者之间的托盘，在各个类之间进行共享。当`Main`类开始后，先读入这份数据的到达模式，之后启动输入线程和电梯线程，输入线程不断读入新的请求，将请求加入到等待队列中，同时`Elevator`类不断处理等待队列中的请求，处理时每到一个新的楼层就会询问`Scheduler`类获得新的目标楼层。当输入到EOF时，输入线程结束。电梯内外的全部请求都处理结束时，电梯线程也结束，之后`Main`类主线程就会结束。

具体对于三种到达模式。`Random`直接使用了`ALS`进行调度。而`Morning`模式下则是先等待2s再开始接送乘客，且当电梯内没有乘客时自动返回1楼。`Night`模式下则是对当前的等待队列进行从大到小的排序，先接高楼层的乘客，接满后直接返回1层，反复如此进行下去。

##### 同步块的设置和锁的选择

本次的共享变量主要有`waitqueue`和`initqueue`。前者被电梯类，策略类和输入类三者共享，后者只被电梯类和策略类两者共享（此处忽略了同样共享的主类）。

```java
//Elevator.run
if (initQueue.noWaiting() && waitQueue.noWaiting()) {
    synchronized (waitQueue) { 
        try {
            waitQueue.wait(); //等待队列为空，需要先等输入线程接受新的输入
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
if (pattern.equals("Night")) {
    synchronized (waitQueue.getRequests()) { //Night模式下需要排序，给waitQueue类里的请求列表加锁
        waitQueue.getRequests()
            .sort(Comparator.comparingInt(PersonRequest::getFromFloor)); 
    }
}

//InputThread.run
synchronized (waitQueue) { //给waitQueue加锁保证线程安全
    if (request == null) {
        waitQueue.close();
        waitQueue.notifyAll(); //输入结束时唤醒等待队列，让Elevator类继续处理请求
        try {
            elevatorInput.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
        return;
    } else {
        waitQueue.addRequest(request);
        waitQueue.notifyAll(); //有新的输入时唤醒等待队列，让Elevator类继续处理请求
    }
}

//Main.main 
try {
    inputThread.join();
} catch (InterruptedException e) {
    e.printStackTrace();
}
try {
    elevator.join();
} catch (InterruptedException e) {
    e.printStackTrace();
} //等两个线程均结束，主线程才结束
```

##### 优点

架构较为清晰，每个类的功能都比较独立，且将策略类和电梯类分开，易于更换策略。对一个电梯的建模比较完整，便于第六次作业中增加电梯。

##### 缺点

`Random`模式下只采用了标准的ALS调度方式，导致性能较差。

### 第六次作业

#### 代码可视化与数据统计

##### 程序类图

![](https://fzc-1300590701.cos.ap-nanjing.myqcloud.com/blogImages/OO-Unit2/hw6.png)

本次作业和上次作业相比改动幅度不大，只增加了一个调度器类。

##### 程序复杂度分析

* 类复杂度

| Class       | OCavg | OCmax | WMC  |
| ----------- | ----- | ----- | ---- |
| Dispatcher  | 3.5   | 10    | 21   |
| Elevator    | 2.5   | 8     | 25   |
| InitQueue   | 1     | 1     | 7    |
| InputThread | 3     | 5     | 6    |
| Main        | 4     | 4     | 4    |
| Scheduler   | 8     | 16    | 40   |
| WaitQueue   | 1     | 1     | 7    |

可以看出`Dispatcher`和`Elevator、Scheduler`类的复杂度较高。这三个类也是本次作业的核心类。

* 方法复杂度

| Method                                                       | CogC | ev(G) | iv(G) | v(G) |
| ------------------------------------------------------------ | ---- | ----- | ----- | ---- |
| Dispatcher.Dispatcher(WaitQueue,ArrayList<Elevator>,String)  | 0    | 1     | 1     | 1    |
| Dispatcher.dispatch()                                        | 39   | 3     | 12    | 12   |
| Dispatcher.getMinElevatori()                                 | 3    | 1     | 2     | 3    |
| Dispatcher.getMorningMinElevatori()                          | 3    | 1     | 2     | 3    |
| Dispatcher.getNightMinElevatori()                            | 3    | 1     | 2     | 3    |
| Dispatcher.run()                                             | 0    | 1     | 1     | 1    |
| Elevator.Elevator(WaitQueue,InitQueue,String,String)         | 0    | 1     | 1     | 1    |
| Elevator.addRequest(PersonRequest)                           | 0    | 1     | 1     | 1    |
| Elevator.close()                                             | 0    | 1     | 1     | 1    |
| Elevator.getInitReqNum()                                     | 0    | 1     | 1     | 1    |
| Elevator.getReqNum()                                         | 0    | 1     | 1     | 1    |
| Elevator.getWaitQueue()                                      | 0    | 1     | 1     | 1    |
| Elevator.getWaitReqNum()                                     | 0    | 1     | 1     | 1    |
| Elevator.move(int)                                           | 6    | 3     | 3     | 5    |
| Elevator.personFlow()                                        | 11   | 1     | 8     | 8    |
| Elevator.run()                                               | 26   | 4     | 12    | 12   |
| InitQueue.InitQueue()                                        | 0    | 1     | 1     | 1    |
| InitQueue.addRequest(PersonRequest)                          | 0    | 1     | 1     | 1    |
| InitQueue.clearQueue()                                       | 0    | 1     | 1     | 1    |
| InitQueue.close()                                            | 0    | 1     | 1     | 1    |
| InitQueue.getRequests()                                      | 0    | 1     | 1     | 1    |
| InitQueue.isEnd()                                            | 0    | 1     | 1     | 1    |
| InitQueue.noWaiting()                                        | 0    | 1     | 1     | 1    |
| InputThread.InputThread(WaitQueue,ElevatorInput,ArrayList<Elevator>,String) | 0    | 1     | 1     | 1    |
| InputThread.run()                                            | 11   | 3     | 6     | 6    |
| Main.main(String[])                                          | 6    | 1     | 6     | 6    |
| Scheduler.Scheduler(WaitQueue,InitQueue,String)              | 0    | 1     | 1     | 1    |
| Scheduler.getDesFloor(int)                                   | 49   | 13    | 16    | 22   |
| Scheduler.getFloor(int,int)                                  | 26   | 7     | 3     | 15   |
| Scheduler.getMorningFloor(int,int)                           | 13   | 4     | 2     | 8    |
| Scheduler.getNightFloor(int,int)                             | 13   | 4     | 2     | 8    |
| WaitQueue.WaitQueue()                                        | 0    | 1     | 1     | 1    |
| WaitQueue.addRequest(PersonRequest)                          | 0    | 1     | 1     | 1    |
| WaitQueue.clearQueue()                                       | 0    | 1     | 1     | 1    |
| WaitQueue.close()                                            | 0    | 1     | 1     | 1    |
| WaitQueue.getRequests()                                      | 0    | 1     | 1     | 1    |
| WaitQueue.isEnd()                                            | 0    | 1     | 1     | 1    |
| WaitQueue.noWaiting()                                        | 0    | 1     | 1     | 1    |

本次复杂度较高的是调度器的`dispatch`方法，以及获得目标楼层的``getDesFloor`方法和电梯运行的`run`方法。

##### 程序行数统计

![](https://fzc-1300590701.cos.ap-nanjing.myqcloud.com/blogImages/OO-Unit2/hw6line.png)

本次作业代码量较大，总有效行数是571行。

#### 代码分析

##### 原理分析

本次作业在上一次作业基础上主要增加了调度器类，用来实现各个电梯的负载均衡调度，采用了分布式调度，即将一个大的请求队列分配给各个电梯自己的请求队列，由各个电梯对自己的请求队列进行处理。相当于采用了两对消费者-生产者，一对是输入线程和调度器线程，一对是调度器线程和各个电梯。输入线程终止条件与上次作业相同，而调度器线程终止条件则是没有新的输入且总的请求队列为空。各个电梯自己的终止条件是自身的请求等待队列为空。具体策略上，本次作业采用了和上次作业一致的策略。

##### 同步块的设置和锁的选择

本次作业共享的变量为总请求等待队列，各个电梯自身的请求等待队列和电梯内请求队列。`Elevator`和`Scheduler`类的同步与上次一致，不再赘述。

```java
//Dispatcher.dispatch
if (waitQueue.isEnd() && waitQueue.noWaiting()) {
    for (Elevator elevator : elevators) {
        synchronized (elevator.getWaitQueue()) { //遍历时给每个电梯的等待队列类加上锁保证安全
            elevator.getWaitQueue().close();
            elevator.getWaitQueue().notifyAll(); 
            //输入线程结束且总等待队列为空将所有电梯的等待队列关闭，同时唤醒所有电梯
        }
    } 
    return;
} else {
    if (waitQueue.noWaiting()) {
        synchronized (waitQueue) {
            try {
                waitQueue.wait(); //总等待队列为空但输入线程并未结束时，先等待
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    } else {...}
}

//InputThread.run
synchronized (waitQueue) { //给总等待队列加锁，防止与调度器线程冲突
    if (request == null) {
        waitQueue.close(); //关闭总请求队列
        waitQueue.notifyAll(); //输入结束时唤醒调度器内的总等待队列
        try {
            elevatorInput.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
        return;
    } else {
        if (request instanceof PersonRequest) {
            waitQueue.addRequest((PersonRequest) request);
        } else if (request instanceof ElevatorRequest) {
            Elevator elevator = new Elevator(new WaitQueue(), new InitQueue(),
                                             pattern, ((ElevatorRequest) request).getElevatorId());
            synchronized (elevators) { //给电梯数组加锁，保证线程安全
                elevators.add(elevator);
                elevator.start();
            }
        }
        waitQueue.notifyAll(); //在新的请求进入等待队列中或者加入了新电梯后唤醒所有的等待队列。
    }
}
```

##### 优点

在没有非必要的对第五次作业的修改基础上，只通过新增一个调度器线程类对请求进行处理，复用了电梯类和策略类，层次架构较清晰，是一次比较好的增量开发。同时，调度器每次分配请求时对现有的每个电梯内的请求人数进行比较，较好的考虑了负载均衡。

##### 缺点

因为写代码不谨慎，出现了一处潜在的死循环发生区。

### 第七次作业

#### 代码可视化与数据统计

##### 程序类图

![](https://fzc-1300590701.cos.ap-nanjing.myqcloud.com/blogImages/OO-Unit2/hw7.png)

本次作业的架构与上次一致。

##### 程序复杂度分析

* 类复杂度

| Class       | OCavg | OCmax | WMC  |
| ----------- | ----- | ----- | ---- |
| Dispatcher  | 3.75  | 9     | 15   |
| Elevator    | 2.93  | 8     | 41   |
| InitQueue   | 1     | 1     | 7    |
| InputThread | 3     | 5     | 6    |
| Main        | 3     | 3     | 3    |
| Scheduler   | 6.6   | 14    | 33   |
| WaitQueue   | 1     | 1     | 7    |

* 方法复杂度

| Method                                                       | CogC | ev(G) | iv(G) | v(G) |
| ------------------------------------------------------------ | ---- | ----- | ----- | ---- |
| Dispatcher.Dispatcher(WaitQueue,ArrayList<Elevator>,HashSet<Integer>) | 0    | 1     | 1     | 1    |
| Dispatcher.dispatch()                                        | 34   | 4     | 11    | 12   |
| Dispatcher.getMinElevatori(PersonRequest)                    | 6    | 1     | 2     | 4    |
| Dispatcher.run()                                             | 0    | 1     | 1     | 1    |
| Elevator.Elevator(WaitQueue,InitQueue,String,String,String,WaitQueue,HashSet<Integer>) | 3    | 1     | 2     | 3    |
| Elevator.addRequest(PersonRequest)                           | 5    | 1     | 5     | 6    |
| Elevator.canArrive(PersonRequest)                            | 10   | 3     | 1     | 12   |
| Elevator.canBOut(PersonRequest)                              | 4    | 1     | 1     | 5    |
| Elevator.canCOut(PersonRequest)                              | 7    | 2     | 1     | 8    |
| Elevator.close()                                             | 0    | 1     | 1     | 1    |
| Elevator.getInitReqNum()                                     | 0    | 1     | 1     | 1    |
| Elevator.getReqNum()                                         | 3    | 1     | 2     | 3    |
| Elevator.getType()                                           | 0    | 1     | 1     | 1    |
| Elevator.getWaitQueue()                                      | 0    | 1     | 1     | 1    |
| Elevator.getWaitReqNum()                                     | 0    | 1     | 1     | 1    |
| Elevator.move(int)                                           | 8    | 3     | 3     | 6    |
| Elevator.personFlow()                                        | 17   | 4     | 11    | 12   |
| Elevator.run()                                               | 25   | 4     | 11    | 11   |
| InitQueue.InitQueue()                                        | 0    | 1     | 1     | 1    |
| InitQueue.addRequest(PersonRequest)                          | 0    | 1     | 1     | 1    |
| InitQueue.clearQueue()                                       | 0    | 1     | 1     | 1    |
| InitQueue.close()                                            | 0    | 1     | 1     | 1    |
| InitQueue.getRequests()                                      | 0    | 1     | 1     | 1    |
| InitQueue.isEnd()                                            | 0    | 1     | 1     | 1    |
| InitQueue.noWaiting()                                        | 0    | 1     | 1     | 1    |
| InputThread.InputThread(WaitQueue,ElevatorInput,ArrayList<Elevator>,String,HashSet<Integer>) | 0    | 1     | 1     | 1    |
| InputThread.run()                                            | 11   | 3     | 6     | 6    |
| Main.main(String[])                                          | 5    | 1     | 5     | 5    |
| Scheduler.Scheduler(WaitQueue,InitQueue,String,int)          | 0    | 1     | 1     | 1    |
| Scheduler.getBFloor(int,int)                                 | 4    | 2     | 1     | 3    |
| Scheduler.getCFloor(int,int)                                 | 7    | 2     | 1     | 6    |
| Scheduler.getDesFloor(int)                                   | 38   | 10    | 14    | 17   |
| Scheduler.getTakeFloor(int,int)                              | 28   | 7     | 4     | 16   |
| WaitQueue.WaitQueue()                                        | 0    | 1     | 1     | 1    |
| WaitQueue.addRequest(PersonRequest)                          | 0    | 1     | 1     | 1    |
| WaitQueue.clearQueue()                                       | 0    | 1     | 1     | 1    |
| WaitQueue.close()                                            | 0    | 1     | 1     | 1    |
| WaitQueue.getRequests()                                      | 0    | 1     | 1     | 1    |
| WaitQueue.isEnd()                                            | 0    | 1     | 1     | 1    |
| WaitQueue.noWaiting()                                        | 0    | 1     | 1     | 1    |

##### 程序行数统计

![](https://fzc-1300590701.cos.ap-nanjing.myqcloud.com/blogImages/OO-Unit2/hw7line.png)

本次代码量较上次增加了60行，主要增加了与电梯性质和换乘相关的代码。

#### 代码分析

##### 原理分析

本次作业架构与上次一致。对调度器内部的负载均衡判定方法除了考虑电梯现有请求队列人数，加入了判断是否能停留的逻辑和优先级的逻辑（速度快的优先级高）,加入了换乘的处理逻辑。同时，改变了调度器线程的终止条件，当且仅当输入线程终止且所有换乘乘客均已到达目的地，才结束。而调度器线程的结束也才会导致若干个电梯线程的结束。具体电梯的策略类和前两次作业保持一致。

##### 同步块的设置和锁的选择

本次作业与前两次作业比起来，由于要处理换乘的情况，加入了一个记录换乘乘客id的集合`checkidset`用来判断调度器线程的终止条件。核心的线程安全问题也主要集中在对这个集合的处理上。

```java
//Elevator.run
if (d == 0) { //乘客到达目的地时
    ...
    TimableOutput.println(
        String.format("OUT-%d-%d-%s", request.getPersonId(), floor, id));
    synchronized (checkidset) {
        checkidset.remove(request.getPersonId()); //将这个id从换乘id集合中移除
        checkidset.notifyAll(); //唤醒换乘人集合，让调度器继续作业
    }
} else if ((type.equals("B") && canBOut(request))) { //到达中转站
  	...
    TimableOutput.println(String.format("OUT-%d-%d-%s",
                                        request.getPersonId(), floor, id));
    synchronized (allWaitQueue) {
        PersonRequest newrequest = new PersonRequest(floor,
                                                    request.getToFloor(), request.getPersonId());
        allWaitQueue.addRequest(newrequest); //总请求等待队列新增请求
        allWaitQueue.notifyAll(); //唤醒总请求等待对列，让调度器继续分配请求
    }
    synchronized (checkidset) { checkidset.notifyAll(); } //唤醒换乘人集合，让调度器继续作业
}

//Dispatcher.dispatch
if (waitQueue.isEnd() && waitQueue.noWaiting()) { //当输入线程已经终止时
    synchronized (checkidset) { //给换乘人集合加锁
        if (checkidset.size() > 0) { //当前还有换乘的乘客没有到达目的地
            try {
                checkidset.wait(); //等待乘客到达目的地或者新的换乘请求加入
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            synchronized (waitQueue) { //处理现在的请求
                ArrayList<PersonRequest> requests = waitQueue.getRequests();
                for (int i = 0; i < requests.size(); i++) {
                    int mini = getMinElevatori(requests.get(i));
                    elevators.get(mini).addRequest(requests.get(i));
                    synchronized (elevators.get(mini).getWaitQueue()) {
                        elevators.get(mini).getWaitQueue().notifyAll();
                    }
                    requests.remove(i);
                    i--;
                }
            }
        }
        if (checkidset.size() > 0) { //如果还有没到达的换乘乘客，继续下一轮循环
            continue;
        }
    }
    for (Elevator elevator : elevators) { //所有换乘乘客均已到达，可以关闭各个线程的等待队列了
        synchronized (elevator.getWaitQueue()) {
            elevator.getWaitQueue().close(); 
            elevator.getWaitQueue().notifyAll();
        }
    }
    return;
} else {...} //输入线程未终止，正常分配请求
```

##### UML时序图

![](https://fzc-1300590701.cos.ap-nanjing.myqcloud.com/blogImages/OO-Unit2/hw7-0.png)

时序图说明：只保留了几个线程类，主要展示了整个时序流程而省略了实现细节。图中Elevator类是所有elevator的代表

##### 可扩展性分析

1. **功能设计**上。由于上次作业架构较为合理，本次作业整体改动不大，同时，在完善了对电梯类型和电梯属性的扩展后，现在的电梯的可扩展性较强，之后改动时只需要针对不同的电梯种类在电梯类做出相应的改变和扩展即可。其他地方不用改变。
2. **性能设计**上。本次作业实现了换乘，但是由于各种原因没有实现对所有电梯状态的整体把握和换乘考虑，因此性能表现一般。

## 二、bug分析

### 第五次作业

#### 自己的bug

##### bug描述

本次作业在强测中AK，在互测中被找出一个bug

原因是线程安全没考虑周全

```java
Exception in thread "Thread-1" java.util.ConcurrentModificationException
	at java.util.ArrayList.sort(ArrayList.java:1464)
	at Elevator.run(Elevator.java:68)
```

问题原因其实就是源于以下的代码。即是因为`Elevator`类在`Night`模式下对等待请求队列排序的处理有误。

```java
if (pattern.equals("Night")) {
        waitQueue.getRequests()
            .sort(Comparator.comparingInt(PersonRequest::getFromFloor));
}
```

##### bug修复总结

是非常明显的线程安全问题，只要给`waitQueue.getRequests()`加锁就没问题了。出现原因本质上还是自己对多线程的理解不深刻，没有养成勤加锁的好习惯。

#### 他人的bug

##### bug描述

本次共分别找到三个人的共计三个bug，都是由于线程安全所致，均导致了RTLE。

```shell
#1 Random hack了3个同学 
[1.0]Random
[1.0]1-FROM-19-TO-1
[1.0]2-FROM-19-TO-1
[1.0]3-FROM-19-TO-1
[1.0]4-FROM-19-TO-1
[1.0]5-FROM-19-TO-1
[1.0]133-FROM-19-TO-1
[1.0]134-FROM-19-TO-1
[1.0]135-FROM-19-TO-1
[1.0]136-FROM-19-TO-1
[1.0]137-FROM-19-TO-1
[1.0]138-FROM-19-TO-1
[1.0]139-FROM-19-TO-1
[1.0]130-FROM-19-TO-1
[1.0]6-FROM-19-TO-1
[1.0]131-FROM-19-TO-1
[1.0]132-FROM-19-TO-1
[2.0]7-FROM-19-TO-1
[2.0]8-FROM-19-TO-1
[2.0]9-FROM-19-TO-1
[2.0]10-FROM-19-TO-1
[2.0]11-FROM-19-TO-1
[2.0]12-FROM-19-TO-1
[2.0]13-FROM-19-TO-1
[10.0]14-FROM-19-TO-17
[10.0]15-FROM-15-TO-19
[10.0]16-FROM-18-TO-1
[10.0]17-FROM-2-TO-20
[10.0]18-FROM-20-TO-4

#2 Night hack了1个同学
[1.0]Night
[1.0]1-FROM-19-TO-1
[1.0]2-FROM-19-TO-1
[1.0]3-FROM-19-TO-1
[1.0]4-FROM-19-TO-1
[1.0]5-FROM-19-TO-1
[1.0]133-FROM-19-TO-1
[1.0]134-FROM-19-TO-1
[1.0]135-FROM-19-TO-1
[1.0]136-FROM-19-TO-1
[1.0]137-FROM-19-TO-1
[1.0]138-FROM-19-TO-1
[1.0]139-FROM-19-TO-1
[1.0]130-FROM-19-TO-1
[1.0]6-FROM-19-TO-1
[1.0]131-FROM-19-TO-1
[1.0]132-FROM-19-TO-1
[1.0]7-FROM-19-TO-1
[1.0]8-FROM-19-TO-1
[1.0]9-FROM-19-TO-1
[1.0]10-FROM-19-TO-1
[1.0]11-FROM-19-TO-1
[1.0]12-FROM-19-TO-1
[1.0]13-FROM-19-TO-1
[1.0]14-FROM-19-TO-1
[1.0]15-FROM-19-TO-1
[1.0]16-FROM-19-TO-1
[1.0]17-FROM-19-TO-1
[1.0]18-FROM-19-TO-1
[1.0]19-FROM-19-TO-1
```

##### 测试方式

本单元由于与很多事情相撞，没完成评测机，所以只能一直采用阅读代码加上手动构造复杂的测试用例的方式来进行互测。基本原则就是构造请求数量和最大限制一样，且出现很多楼层跨度很大的电梯请求的测试数据。

### 第六次作业

#### 自己的bug

本次强测中被hack1个点，互测中未被hack。

##### bug描述

RTLE，运行时间超过了210s

##### bug修复总结

原因如下所示

```java
while (floor != desFloor1) {
    try {
        sleep(400);
    } catch (InterruptedException e) {
        e.printStackTrace();
    }
    floor += step;
    TimableOutput.println(
        String.format("ARRIVE-%d-%s", floor, id));
    desFloor1 = scheduler.getDesFloor(floor);
    //缺少这行:step=floor<desFloor1?:1:-1;
}
```

即我每到一个楼层都会更新下一个目标楼层，而与此同时却没有更新方向。因此出现了问题。当出现新的请求与当前请求方向不一致时，会一直死循环，导致电梯通向天堂或者地狱。

#### 他人的bug

本次未发现他人bug。

尝试用上次的强测用例修改后进行hack，没有成功。

### 第七次作业

#### 自己的bug

本次强测被hack四个点，互测被hack四个点。主要原因是因为换乘导致的唤醒遗漏问题以及两处代码逻辑问题。

**唤醒遗漏**主要发生在B电梯的换乘处理上。

```java
...
TimableOutput.println(String.format("OUT-%d-%d-%s",request.getPersonId(), floor, id));
synchronized (allWaitQueue) {
    PersonRequest newrequest = new PersonRequest(floor,request.getToFloor(), request.getPersonId());
    allWaitQueue.addRequest(newrequest);
    allWaitQueue.notifyAll();
}
...
```

代码中，显然只对总的等待请求队列做了唤醒处理，却没有对checkidset也就是换乘乘客id的集合做唤醒处理，导致了在Dispatcher类中一直卡死，无法结束线程。

**处理逻辑**主要是对B电梯的处理上有问题。一处是当B电梯中有偶数层目的地的乘客时，在到达偶数层会一直循环卡死在这一层。通过设置B电梯且为偶数层跳过的逻辑就解决了。即：`if (type.equals("B") && floor % 2 == 0) { continue; }`。另一处如下所示

```java
if (lenInit == maxnum || (!initReqs.contains(mainRequest) && lenInit == maxnum - 1)) {
    return mainDesFloor;
}
mainDesFloor = maxnum == 6 ? getBFloor(floor, mainDesFloor) : mainDesFloor;
```

当B电梯进行换乘时，我的代码逻辑在某些情况直接返回了主请求的目的地，而不是经过`getBFloor`函数转换后再做将进一步处理。于是导致了问题。只需要将转换的代码放在前面即可

如下所示：

```
mainDesFloor = maxnum == 6 ? getBFloor(floor, mainDesFloor) : mainDesFloor;
if (lenInit == maxnum || (!initReqs.contains(mainRequest) && lenInit == maxnum - 1)) {
    return mainDesFloor;
}
```

#### 他人的bug

本次未发现他人bug。

## 三、心得体会

#### 线程安全

* 一定要勤给变量加锁，同时也要注意锁的先后顺序避免出现死锁。
* 加锁后如果进行了`wait`操作一定要注意在合适的地方进行唤醒，否则就会一直卡死。这个时候最好枚举出所有需要唤醒的情况。
* 共享变量的维护很重要，尽量减少共享变量，同时对于共享变量一定要加锁进行操作。
* 一定要注意每个线程的终止条件，要考虑到尽可能全面的情况，比如第七次作业的输入结束后又产生新的换乘请求的情况。一般需要加一个新的变量来处理。

#### 层次化设计

* 在进行系统设计时，往往需要多线程模式。而这就往往需要对一些对象进行精细的不依赖于上层的建模，比如本次的单台电梯。只要写好一个电梯线程，就可以加无数个电梯，便于之后的扩展。建模过程中要注意不同模块之间的解耦，比如本单元作业就可以将电梯类和策略类分开，便于之后更换策略。
* 写之前要理清不同线程、不同类之间的交互关系和共享变量，并尽可能使得关系更加简洁，共享变量尽可能少。关系清晰了，才不容易出线程安全错误。

#### 经验教训

* 对一个版本底气不足时不如老老实实交把握大的版本。第7次作业最后一次交的时候自知换乘的版本有很多问题，但是时间原因没法进一步debug了，就交上去了。但其实选择更稳妥的没换乘的方式至少正确性可能会好很多。

* 评测机很重要！！上一单元因为有较完善的评测机互测自测都很顺利。本单元没有实现评测机导致很多时候比较被动。
* 修复bug的时候最重要的是通过，所以修改的时候可以不考虑性能，纯考虑正确性来进行修改。另外~~修复bug的时候不要按`Alt+Ctrl+L`~~，原因懂得都懂。