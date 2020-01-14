【必须写在前面】：还为自己单身找理由吗？
【💬】：当你学会了python爬虫技术之后，特别是看了本篇**安利**（案例）之后，审美提高了，乐趣转移了，就不再为单身而苦恼了😎

---
#### 单线程实现：爬取百度“校花”贴吧的图片
文件 [download.py](http://download.py/)
```
from queue import Queue
import threading
import requests

def down_file(url,type='content'):
    headers = {
        'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; WOW64; Trident/7.0; rv:11.0) like Gecko'
    }
    r = requests.get(url, headers=headers)
    if type == 'text':
        return r.text

    return r.content


class DownLoadExecutor(threading.Thread):
    def __init__(self):
        super().__init__()
        self.q = Queue(maxsize=50)
        # 图片保存目录
        self.save_dir = './xiaohua/'
        # 图片计数
        self.index = 0

    def put_task(self,urls):
        if isinstance(urls,list):
            for url in urls:
                self.q.put(url)
        else:
            self.q.put(urls)

    def run(self):
        while True:
            url = self.q.get()
            content = down_file(url)

            # 截取图片名称
            index = url.rfind('/')
            file_name = url[index+1:]
            save_file = self.save_dir + file_name
            with open(save_file,'wb+') as f:
                f.write(content)
                self.index += 1
                print(save_file + "下载成功,下载图片总数：" + str(self.index))
```
文件 [xiaohua.py](http://xiaohua.py/)

```
import requests
from lxml import etree

from download import DownLoadExecutor, down_file

class XiaoHua:
    def __init__(self,init_url):
        self.init_url = init_url
        self.download_executor = DownLoadExecutor()

    def start(self):
        self.download_executor.start()
        self.download(self.init_url)


    def download(self,url):
        html_text = down_file(url,type='text')
        html = etree.HTML(html_text)
        img_urls = html.xpath("//a[contains(@class,'thumbnail')]/img/@bpic")
        self.download_executor.put_task(img_urls)

        # 获取下一页的连接
        next_page = html.xpath("//div[@id='frs_list_pager']/a[contains(@class,'next')]/@href")
        next_page = "http:" + next_page[0]
        self.download(next_page)


if __name__ == '__main__':
    x = XiaoHua("http://tieba.baidu.com/f?kw=校花&ie=utf-8")
    x.start()
```
---
#### 多线程版本 ：百度 “校花”贴吧 图片的爬取
文件： download_pool.py
```
import requests
from concurrent import futures
import threading


def down_file(url,type='content'):
    headers = {
        'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; WOW64; Trident/7.0; rv:11.0) like Gecko'
    }
    r = requests.get(url, headers=headers)
    if type == 'text':
        return r.text

    return r.content

class MultiTask:
    def __init__(self):
        self.task_executor = futures.ThreadPoolExecutor(3)
        self.index = 0
        self.save_dir = "./xiaohua/"
        self.lock = threading.Lock()

    def download_img(self,url):
        content = down_file(url)
        # 截取图片名称
        index = url.rfind('/')
        file_name = url[index + 1:]
        save_file = self.save_dir + file_name
        with open(save_file, 'wb+') as f:
            f.write(content)
            with self.lock:
                self.index += 1
            print(save_file + "下载成功,下载图片总数：" + str(self.index))

    def add_download_task(self,imgs):
        if isinstance(imgs,list):
            todo = []
            for url in imgs:
                # 提交任务
                print(url)
                future = self.task_executor.submit(self.download_img,url)
                future.result()
                # todo.append(future)
```
文件：[xiaohua.py](http://xiaohua.py/)
```
import requests
from lxml import etree

from download_pool import MultiTask ,down_file

class XiaoHua:
    def __init__(self,init_url):
        self.init_url = init_url
        self.downloader = MultiTask()

    def start(self):
        self.download(self.init_url)


    def download(self,url):
        html_text = down_file(url,type='text')
        html = etree.HTML(html_text)
        img_urls = html.xpath("//a[contains(@class,'thumbnail')]/img/@bpic")
        self.downloader.add_download_task(img_urls)

        # 获取下一页的连接
        next_page = html.xpath("//div[@id='frs_list_pager']/a[contains(@class,'next')]/@href")
        print(next_page)
        next_page = "http:" + next_page[0]
        self.download(next_page)


if __name__ == '__main__':
    x = Xiao("http://tieba.baidu.com/f?kw=校花&ie=utf-8")
    x.start()
```
![别误会，🙈🤦‍♂️🙈我不是那种人...](https://upload-images.jianshu.io/upload_images/17476267-d0322423e1a05c22.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![只是单纯🙈🐵🙈分享技术](https://upload-images.jianshu.io/upload_images/17476267-08a54c8c9694df6f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
