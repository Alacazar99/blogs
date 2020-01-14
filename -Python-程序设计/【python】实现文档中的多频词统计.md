#### 案例：三国小说主要人物出场频率统计

首先导入必要的模块：
```py
import jieba
from collections import Counter
from wordcloud import WordCloud
```
注释：导入模块之前，需要先下载 **jieba模块** 和 **WordCloud模块**
```
pip install jieba
pip install WordCloud
```
先举个栗子：
```py
import jieba
# 使用 jieba 分词
txt = '我来到北京清华大学'
seg_list = jieba.lcut(txt)
print(seg_list)
```
> 这个代码块主要使用了 ***jieba库*** 来实现对字符串的分割

输出：
```py
Building prefix dict from the default dictionary ...
Building prefix dict from the default dictionary ...
Loading model from cache C:\Users\ADMINI~1\AppData\Local\Temp\jieba.cache
Loading model cost 0.810 seconds.
Prefix dict has been built succesfully.
['我', '来到', '北京', '清华大学']

Process finished with exit code 0
```
##### 那么，说完了上面的基本操作，现在进入正题。
- 第一步：导入电子版文档，存储在sanguo.txt中（当然，也可以使用迭代对象来分次导入，好处是节约内存）
```py
      with open('sanguo.txt','r', encoding='utf-8') as f:
        # 打开文件路径
        txt = f.read() # 这里直接导入
```
- 第二步：将字符串分割成等量的中文
```py
words = jieba.lcut(txt)
    print(words)
    # ‘人物名字’：出现次数
```
输出：
```py
', '汉中', '；', '达', '与', '李严', '曾结', '生死之交', '；', '臣', '回', '成都', '时留', '李', '严守', '永安', '宫', '；', '臣', '已作', '一书', '、', '只', '做', '李严', '亲笔', '令人', '送与', '孟达',, '；', '达', '必然', '推病', '不出', '以慢', '军心', '：', '此', '一路', '又', '不足', '忧', '矣', '。', '又', '知', '曹真', '引兵', '犯', '阳平关', '；', '此地', '险峻',  # 这里只是一部分
```
- 第三步：一个小说中，按照常理，主角的名字不会是一个字，所以要筛选出大于1的字符串
```py
counts = {}   #创建空字典
    for word in words:   # 遍历字典
        if len(word) == 1:
            continue
        else:
            # 往字典里添加元素
            #counts['key'] = 888
            counts[word] = counts.get(word, 0) + 1
            # counts['曹操'] = counts.get('曹操', 0) + 1
    print(counts)
```
输出：
```
{'正文': 1, '第一回': 1, '桃园': 19, '豪杰': 22, '结义': 14, '黄巾': 40, '英雄': 82, '立功': 22, '滚滚': 5, '长江': 25, '逝水': 1, '浪花': 1, '淘尽': 1, '是非成败': 1, '转头': 2, '青山': 1, '依旧': 11, '几度': 1, '夕阳红': 1, '渔樵': 1, '江渚': 2, '上惯': 1, '秋月春风': 1, }   # 这里只是一部分
```
- 第四步：删除 无关词。例如：将军", "却说", "丞相", "二人", "不可", "荆州", "不能", "如此", "商议", "如何", "主公", "军士", "军马", "左右", "次日", "引兵", "大喜", "天下","东吴", "于是", "今日", "不敢", "魏兵", "陛下", "都督", "人马", "不知",...等。
> 在一篇文章中，除一个字符之外的其他多字符若不是人物名字，需要删除
```py
# 定义无关词集合
    excludes = {"将军", "却说", "丞相", "二人", "不可", "荆州", "不能", "如此", "商议",
                "如何", "主公", "军士", "军马", "左右", "次日", "引兵", "大喜", "天下",
                "东吴", "于是", "今日", "不敢", "魏兵", "陛下", "都督", "人马", "不知",
                '玄德曰', '孔明曰', '刘备', '关公', '孔明曰', '玄德曰', '先主', '此人',
                '上马', '大叫', '后主', '众将', '只见', '一人', '汉中', '太守', '天子',}
    for word in excludes:
        del counts[word]
```
- 第五步：将一些字符串整合在一起，比如‘孔明’和‘孔明曰’，‘玄德’和‘玄德曰’等等
```py
    counts['孔明'] = counts.get('孔明') + counts.get('孔明曰')
    counts['玄德'] = counts.get('玄德') + counts.get('玄德曰')
    counts['玄德'] = counts.get('玄德') + counts.get('刘备')
    counts['云长'] = counts.get('关公') + counts.get('云长')
```
- 第六步：统计出现频率最高的前10个词
```py
    items = list(counts.items())
    items.sort(key=lambda x: x[1], reverse=True)  # 使用了隐函数
    # print('排序后：', items)
    for i in range(10):
        character, count = items[i]
        print(character, count)
```
输出：
```
孔明 1204
玄德 1159
曹操 910
张飞 340
孙权 259
吕布 258
赵云 254
云长 241
司马懿 221
周瑜 216
```
**拓展** :  统计出现频率最高的前10个词--方法2
```py
    roles = Counter(counts)
    role = roles.most_common(10)
    print(role)
```
输出：
```py
[('孔明', 1204), ('玄德', 1159), ('曹操', 910), ('张飞', 340), ('孙权', 259), ('吕布', 258), ('赵云', 254), ('云长', 241), ('司马懿', 221), ('周瑜', 216)]
```
- 第七步：构造词云字符串
```py
    li =[]
    for i in range(10):
        character, count = items[i]
        for _ in range(count):
            li.append(character)
    # print(li)
    cloud_txt = " ".join(li)
    wc = WordCloud(
        background_color='white',
        font_path='msyh.ttc',
        collocations=False  # 是否包含两个词的搭配，默认为Ture
    ).generate(cloud_txt)
    wc.to_file('三国中出现的前十大人物.png')
```
生成图片如下：
![三国中出现的前十大人物.png](https://upload-images.jianshu.io/upload_images/17476267-59188741252cc54e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
完整代码如下：
```py
# 案例：三国小说人物出场词频率统计
import jieba
from collections import Counter
from wordcloud import WordCloud

def parse():
    # 定义无关词集合
    excludes = {"将军", "却说", "丞相", "二人", "不可", "荆州", "不能", "如此", "商议",
                "如何", "主公", "军士", "军马", "左右", "次日", "引兵", "大喜", "天下",
                "东吴", "于是", "今日", "不敢", "魏兵", "陛下", "都督", "人马", "不知",
                '玄德曰', '孔明曰', '刘备', '关公', '孔明曰', '玄德曰', '先主', '此人',
                '上马', '大叫', '后主', '众将', '只见', '一人', '汉中', '太守', '天子',}
    """三国小说人物出场词频率统计"""
    with open('sanguo.txt','r', encoding='utf-8') as f:
        # 打开文件路径
        txt = f.read()
    # print(txt)
    # 将字符串分割成等量的中文
    words = jieba.lcut(txt)
    print(words)
    # ‘人物名字’：出现次数
    counts = {}   #创建空字典
    for word in words:   # 遍历字典
        if len(word) == 1:
            continue
        else:
            # 往字典里添加元素
            #counts['key'] = 888
            counts[word] = counts.get(word, 0) + 1
            # counts['曹操'] = counts.get('曹操', 0) + 1
    print(counts)

    counts['孔明'] = counts.get('孔明') + counts.get('孔明曰')
    counts['玄德'] = counts.get('玄德') + counts.get('玄德曰')
    counts['玄德'] = counts.get('玄德') + counts.get('刘备')
    counts['关公'] = counts.get('关公') + counts.get('云长')

    # 删除 无关词
    for word in excludes:
        del counts[word]
    # 统计出现频率最高的前20个词
    items = list(counts.items())
    items.sort(key=lambda x: x[1], reverse=True)  # 使用了隐函数
    # print('排序后：', items)
    for i in range(10):
        character, count = items[i]
        print(character, count)

    # 统计出现频率最高的前20个词--方法2(拓展)
    roles = Counter(counts)
    role = roles.most_common(10)
    print(role)

#     构造词云字符串
    li =[]
    for i in range(10):
        character, count = items[i]
        for _ in range(count):
            li.append(character)
    # print(li)
    cloud_txt = " ".join(li)
    wc = WordCloud(
        background_color='white',
        font_path='msyh.ttc',
        collocations=False  # 是否包含两个词的搭配，默认为Ture
    ).generate(cloud_txt)
    wc.to_file('三国中出现的前十大人物.png')
parse()
```
```py
孔明 1204
玄德 1159
曹操 910
张飞 340
孙权 259
吕布 258
赵云 254
云长 241
司马懿 221
周瑜 216
[('孔明', 1204), ('玄德', 1159), ('曹操', 910), ('张飞', 340), ('孙权', 259), ('吕布', 258), ('赵云', 254), ('云长', 241), ('司马懿', 221), ('周瑜', 216)]

```
