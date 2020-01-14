观察者模式：观察者模式定义了对象之间的一对多依赖，这样当一个对象改变状态时，它的所有对象都会收到依赖并且自动更新。
- 模式图如下：
![观察者模式图](https://upload-images.jianshu.io/upload_images/17476267-9cc9dddedb6588a3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

图片参考：https://blog.csdn.net/huangkangying/article/details/7901602
举例说明：
```
class Observer:
    """
    观察者
    """
    def __init__(self,name):
        self.name = name

    def update(self,msg):
        print(self.name + '收到信息：' + msg)

class Subject:
    def __init__(self):
        self.observers = []

    def add_observer(self,observer):
        self.observers.append(observer)

    def remove_observer(self,observer):
        self.observers.remove(observer)


    def notify(self,msg):
        for observer in self.observers:
            observer.update(msg)

Zurich = Observer("Zurich")
Alzacar = Observer('Alzacar')

rain = Subject()
# Zurich 订阅该主题
rain.add_observer(Zurich)
# Alzacar 订阅该主题
rain.add_observer(Alzacar)
rain.notify("\n【北京日报】\n今日下午将会出现大面积暴晒！\n")
# Zurich 取消此主题
rain.remove_observer(Zurich)
rain.notify("\n【北京日报】\n今夜午时将会打雷!")
```
- 通过`add() 函数`和`remove() 函数`来实现对主题的订阅和退订
- 关于notify():和notifyAll():
  （1）**notify():**

唤醒在此对象监视器上等待的`单个线程`。如果所有线程都在此对象上等待，则会选择唤醒其中一个线程。`选择是任意性的`，并在对实现做出决定时发生。线程通过调用其中一个 wait 方法，在对象的监视器上等待。

直到当前线程放弃此对象上的锁定，才能继续执行被唤醒的线程。被唤醒的线程将以常规方式与在该对象上主动同步的其他所有线程进行竞争；例如，唤醒的线程在作为锁定此对象的下一个线程方面没有可靠的特权或劣势。

 （2）**notifyAll():**

唤醒在此对象监视器上等待的`所有线程`。线程通过调用其中一个 `wait `方法，在对象的监视器上等待。

直到当前线程放弃此对象上的锁定，才能继续执行被唤醒的线程。被唤醒的线程将以常规方式与在该对象上主动同步的其他所有线程进行竞争；例如，唤醒的线程在作为锁定此对象的下一个线程方面没有可靠的特权或劣势。

 

执行结果：
```py
Zurich收到信息：
【北京日报】
今日下午将会出现大面积暴晒！

Alzacar收到信息：
【北京日报】
今日下午将会出现大面积暴晒！

Alzacar收到信息：
【北京日报】
今夜午时将会打雷!
```
