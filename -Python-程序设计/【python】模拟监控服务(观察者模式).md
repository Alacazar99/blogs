##### 程序设计：
有三台服务器，每隔5s向某台服务器发送请求，看是否获取正常的返回值。
如果返回正常`True`，则继续监控；如果连续3次没有返回正常值`False`，则向指定服务器负责人发送信息通知;

关于[python设计模式之观察者模式](https://www.jianshu.com/p/7adcd23cc112)（参考）
代码实现：
```
import random
class Server:
    def __init__(self,name):
        self.times = random.randint(3,10)
        self.name = name

    def connect(self):
        if self.times > 0:
            self.times -= 1
            return True
        return False

class Monitor:
    def __init__(self):
        # 监控列表
        self.servers = []
        self.admins = []

    def add_server(self,server):
        self.servers.append(server)
        return self

    def add_admin(self,admin):
        self.admins.append(admin)

    def test(self):
        for server in self.servers:
            if not server.connect():
                # 通知相关者
                for admin in self.admins:
                    Advisor.send_sms(admin,server.name + "链接异常~！")
                pass
            else :
                print(server.name + "测试正常...")

class Advisor:
    @staticmethod
    def send_sms(admin,msg):
        print('向' + admin + "发送信息：" + msg)

monitor = Monitor()
monitor.add_admin("zs")
monitor.add_admin("lisi")
monitor.add_server(Server("server1")).add_server(Server("server2"))

import time
while True:
    monitor.test()
    time.sleep(3)
```
![执行结果](https://upload-images.jianshu.io/upload_images/17476267-0908a6e47a7d4c27.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
