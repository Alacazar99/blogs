####selenium 
【介绍】：selenium 是一套完整的web应用程序测试系统，包含了**测试的录制**<sup>（selenium IDE）</sup>，**编写及运行**<sup>（Selenium Remote Control）</sup>和**测试的并行处理**<sup>（Selenium Grid）</sup>。

【通俗的解读】：Selenium最初是为网站自动化测试而开发的，Selenium 可以直接运行在浏览器上，它支持所有主流的浏览器（包括PhantomJS），当然啦，也可以接收指令，让浏览器自动加载页面，获取需要的数据，甚至页面截屏，功能丰富，不容错过哦 。

安装：
```
pip install selenium
```
（最好是加一个清华源,安装速度Plus）：
```
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple selenium
```
##### Chrome浏览器驱动的安装
可参考：http://blog.csdn.net/huilan_same/article/details/51896672
【注意】：要下载对应版本的**chromedriver**：[http://npm.taobao.org/mirrors/chromedriver/](http://npm.taobao.org/mirrors/chromedriver/)
#### Chromedriver 驱动
没有驱动的状态下，报错：`selenium.common.exceptions.WebDriverException: Message: 'chromedriver' executable needs to be in PATH.`（需要给'chromedriver'配置路径）
【处理办法】：在你编译环境python中配置一下路径。分为以下两个步骤：
- 找到你编译环境python的文件路径；
- 将你对应版本的安装文件（.exe）放入python目录下的Scripts目录中；

![目录](https://upload-images.jianshu.io/upload_images/17476267-601a4337216dc703.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


---
#####页面元素定位符：
在爬虫的一系列操作中，如何给**目标元素定位**是十分总要的基础操作。
selenium中 最核心的技巧是**WebElement类型的识别和定位**，主要介绍以下八类：

######（1）By.id()  通过id定位
```
 browser.find_element_by_id("kw")
```
【注解】：元素的id属性来定位元素——**id是唯一标识**
这里的两种形式是同样的效果..
```
    browser.find_element_by_id("kw")
    # 另一种形式：
    browser.find_element((By.id,"kw"))
```
【重点】：这里的两种形式是等价的！

###### （2）By.name()  通过name 定位
```
search=browser.find_element_by_name("wd")

search=browser.find_elements_by_name("wd")
```
【解释】：
- 如果name属性的值kw**是唯一**的，用`find_element_by_name`定位元素，返回值是一个值；
- 如果name属性的值kw**不是唯一**的，用`find_elements_by_name`定位元素，返回符合条件的多个值，保存在列表中，即返回的是列表。


######（3）By.xpath() 通过xpath定位
```
browser.find_element_by_xpath("//*[@id='kw']")
```

######（4） By.className() 通过className定位
【介绍】：通过元素的class属性来定位元素，class属性不是绝对唯一的。（标签的class属性可能相同）
```
browser.find_element_by_class_name("s_ipt")
```

######（5） By.cssSelector() 通过CSS 定位
```
browser.find_element_by_css_selector()
```
######（6）By.linkText() 通过linkText
```
browser.find_element_by_link_text()
```

- 精确匹配——文本内容唯一；
- 模糊匹配——通过文本内容的部分内容，文本内容不唯一；

###### （7）By.tagName() 通过tagName
```
 browser.find_element_by_tag_name("input")
```
###### （8）By.partialLinkText() 通过匹到的部分linkText
```
browser.find_element_by_partial_link_text()
```
---

【注意】：查找**多个元素节点**和**元素单个节点**相同，只是将element + s。例如：
```
    browser.find_elements_by_css_selector()
    browser.find_elements_by_id()
    browser.find_elements_by_name()
    browser.find_elements_by_link_text()
    browser.find_element_by_partial_link_text()
```
---


【小案例】：运行小程序，在百度搜索简书。
```
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.support import expected_conditions as EC
from selenium.webdriver.support.wait import WebDriverWait

browser = webdriver.Chrome()
try:
    browser.get("https://www.baidu.com")
    input = browser.find_element_by_id('kw')
    input.send_keys('简书')
    input.send_keys(Keys.ENTER)
    # 等待对象反应
    wait = WebDriverWait(browser,100)
    locator = (By.ID,"content")
    wait.until(EC.presence_of_element_located((locator)))
    print(browser.current_url)
    print(browser.get_cookies())
    # print(browser.page_source)
    ele = browser.find_element_by_id("content_left")
    print(ele)

finally:
    browser.close()
```
【小案例】：运行小程序，在京东搜索商品。
网址：https://www.jd.com
```
from time import sleep
from selenium import webdriver
# 京东商城搜索
def simulate_jd_search():
    browser = webdriver.Chrome()
    browser.get("https://www.jd.com")
    # 模拟输入问题
    input = browser.find_element_by_id("key")
    input.send_keys("手机")
    time.sleep(3)
    # 模拟清空内容
    input.clear()
    input.send_keys("小猪")
    button = browser.find_element_by_css_selector("button.button")
    button.click()
    time.sleep(10)

simulate_jd_search()
```
【小案例】：运行小程序，模拟鼠标拖拽
网址：https://www.runoob.com/try/try.php?filename=jqueryui-example-droppable
```
from time import sleep
from selenium import webdriver
from selenium.webdriver import ActionChains
def simulate_drop():
    browser = webdriver.Chrome()
    browser.get("https://www.runoob.com/try/try.php?filename=jqueryui-example-droppable")
    # 切换到id是iframeResult的iframe标签中；
    browser.switch_to.frame("iframeResult")
    source = browser.find_element_by_css_selector("#draggable")
    target = browser.find_element_by_css_selector("#droppable")
    actions = ActionChains(browser)

    actions.drag_and_drop(source,target)
    actions.perform()   #执行
    time.sleep(10)      #延迟10秒

simulate_drop()
```
---

##### 关于selenium.webdrive之ActionChains
ActionChains是自动执行低级交互的一种方式，例如：鼠标移动，鼠标点按，键盘操作，文本操作等。

当我们调用这里的方法时，这些操作会被先储存在一个队列中，当我们调用perform()方法时，队列中的操作会被按顺序执行，执行后队列被清空。

关于ActionChains 类提供的鼠标操作的常用方法见下表：
常用方法|鼠标操作|参数
|:-:|:-:|:-:|
**click**(self, element=None)|点击 |若参数None，那么点击当前位置；若参数是element，那么点击此元素；
**click_and_hold(**self, element=None)|鼠标左键按住某个元素 |同上
**context_click**(self, element=None) |右键点击 | 同上
** double_click()**| 双击 |同上
**drag_and_drop**(source, target)| 拖动 |【source】: 元素位置；【target】: 目标位置；
**move_to_element**(self, to_element):| 鼠标悬停 |to_element: 定位需要悬停的元素
**drag_and_drop_by_offset**(self, source, xoffset, yoffset):|按住元素，然后移动目标偏移量|【source】: 元素位置； 【xoffset】: X 轴的偏移量；【yoffset】: Y 轴的偏移量
**key_down**(self, value, element=None)|应用于修饰键（控制、alt和shift）| 【value】： 要发送的修饰符键；【element】：定位的元素
 **send_keys**(self, *keys_to_send）|发送到当前焦点元素|要发送的按键。（修饰符键常数可以在“Keys”类）
【注】：更多鼠标操作可参考：[http://www.51testing.com/index.php?action-viewnews-itemid-3725836-php-1](http://www.51testing.com/index.php?action-viewnews-itemid-3725836-php-1)
