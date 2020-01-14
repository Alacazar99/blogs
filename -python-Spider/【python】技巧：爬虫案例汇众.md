【小案例💬】：运行小程序，实现 “在线翻译”
```
class JinShan(object):
    def __init__(self,word):
        self.url = 'http://fy.iciba.com/ajax.php?a=fy'
        self.headers = {
            "User-Agent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_13_3) AppleWebKit/537. 36 (KHTML, like Gecko) Chrome/65.0.3325.181 Safari/537.36"
        }
        self.post_data = {"f": "auto", "t": "auto", "w": word}

    def request_post(self):
        response = requests.post(url=self.url, headers=self.headers,data=self.post_data)
        # print(response.content.decode())
        return response.content.decode()

    def translates(self,data):
        dict_data = json.loads(data)
        if dict_data['status'] == 1:
            print(dict_data['content']['out'])
        if dict_data['status'] == 0:
            print(dict_data['content']['word_mean'])

    def run(self):
        data = self.request_post()
        print('译文：')
        self.translates(data)

if __name__ == "__main__":
    print('输入您要翻译的内容：')
    word = input()
    jinshan = JinShan(word)
    jinshan.run()
```
运行结果：
- 汉译英
```
输入您要翻译的内容：
使用requests可以模拟浏览器的请求。
译文：
Use requests to simulate browser requests.
```
- 英译汉
```
输入您要翻译的内容：
Use requests to simulate browser requests.
译文：
使用请求模拟浏览器请求。
```
【小案例💬】：运行小程序，执行js代码
```
# 执行js代码
def execute_js():
    # 执行js代码：execute_script()
    browser = webdriver.Chrome()
    browser.get("https://www.jd.com")
    while True:
        browser.execute_script("window.scrollTo(0,document.body.scrollHeight)")
        time.sleep(3)
        browser.execute_script("alert('滚动到底部')")
    # time.sleep(100)
execute_js()
```

【小案例💬】：运行小程序，获取属性信息
```

def get_node_info():
    browser = webdriver.Chrome()
    browser.get("https://www.jd.com")
    input= browser.find_element_by_id("key")
    # 获取属性信息
    print(input.get_attribute("class"))

    # 获取属性节点的文本值，使用text属性
    a_node = browser.find_element(By.ID,"navitems-group2")
    print(a_node.text)

    # 获取id、位置、标签、大小
    print(input.id)
    print(input.location)
    print(input.tag_name)
    print(input.size)
    time.sleep(3)

get_node_info()
```

【小案例💬】：延时等待
```
# 延时等待；
def wait_page():
    pass
    # 隐式等待：设置脚本在查找元素时的最大等待时间
    browser = webdriver.Chrome()
    browser.get("https://www.zhihu.com/explore")
    browser.implicitly_wait(10)
    elements = browser.find_elements(By.CLASS_NAME,"ExploreFollowButton")
    print(elements)


    # 显示等待：就是明确的要等待的元素在规定的时间之内都没找到,那么就抛出Exception.
    browser.get("https://www.taobao.com")
    wait = WebDriverWait(browser,10)
    input(wait.until(EC.presence_of_all_elements_located((By.ID, "q"))))
    button = wait.until(EC.element_to_be_clickable((By.CSS_SELECTOR, ".btn-search")))
    print(input,button)
```
关于延时等待的方式
【隐式等待💬】：设置脚本在查找元素时的最大等待时间；
【显示等待💬】：就是明确的要等待的元素在规定的时间之内都没找到,那么就抛出Exception.
等待的条件       |含义
|:-:|:-:|
title_is     | 标题是某内容
title_contains     | 标题包含某内容
presence_of_element_located | 节点加载出来，需要传入定位元组，如：(By.ID,'q')
visibility_of_element_located | 节点可见，传入定位元组
visibility_of  | 传入的节点对象可见
presence_of_all_elements_located | 所有节点可见
text_to_be_present_in_element | 某个节点包含某文字
element_to_be_clickable | 节点可被点击
staleness_of | 判断一个节点是否仍在DOM中，可以判断页面是否被刷新
alert_is_present | 判断警告框是否弹出


【小案例💬】：运行小程序，网页前进一步、后退一步
```
def control_browser():
    browser = webdriver.Chrome()
    browser.get("https://www.jd.com")
    browser.get("https://www.baidu.com")
    browser.get("https://www.tianmao.com")
    browser.get("https://www.taobao.com")

    browser.back()
    sleep(2)
    browser.forward()
    browser.forward()
    sleep(2)
    browser.close()

control_browser()
```

【小案例💬】：运行小程序，获取网页的cookie
```
# 获取cookie
def opt_cookie():
    browser = webdriver.Chrome()
    browser.get("https://www.baidu.com")
    # 获取所有cookie
    print(browser.get_cookies())
    # 添加cookie
    browser.add_cookie({"name":"username", "value":"Zurich"})
    print(browser.get_cookies())
    # 删除 cookie
    browser.delete_all_cookies()
    print(browser.get_cookies())

opt_cookie()
```

【小案例💬】：运行小程序，js新打开另外网页
```
# js新打开一个标签页
def opt_tag():
    browser = webdriver.Chrome()
    browser.get("https://www.tianmao.com")
    # 用js新打开一个标签页
    sleep(5)
    browser.execute_script("window.open('https://www.jd.com')")
    sleep(5)
    browser.execute_script("window.open('https://www.taobao.com')")
    # 获取所有的标签
    print(browser.window_handles)
    browser.switch_to.window(browser.window_handles[1])
    sleep(5)
    browser.switch_to.window(browser.window_handles[0])
    sleep(5)
    browser.switch_to.window(browser.window_handles[2])
    sleep(5) #直接延迟5秒

opt_tag()
```
【小案例💬】：运行小程序，网站截图
```
def opt_phantom_js():
    # 网站首页截图：方式 1
    from selenium.webdriver.chrome.options import Options
    chrome_options= Options()
    chrome_options.add_argument("——headless")
    broswer = webdriver.Chrome(options=chrome_options)

    # 网站首页截图：方式 2
    # broswer = webdriver.PhantomJS()

    broswer.get("http://www.jd.com")
    broswer.save_screenshot("jd.png")

opt_phantom_js()
```
运行效果：
![jd.png](https://upload-images.jianshu.io/upload_images/17476267-2d12223d495c8e4d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
