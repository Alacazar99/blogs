##### 时间处理模块
1.1. time.time()
time time() 返回当前时间的时间戳
```
import time
print(time.time())
```
输出：1560661640.7444859

在Python的时间处理模块中，`time`这个模块主要侧重于时间戳格式的处理，而`datetime则相当于time模块的高级封装`，提供了更多关于日期处理的方法。
##### 关于datetime
datetime主要由五个模块组成：
- datetime.date：表示日期的类。常用的属性有year, month, day。
- datetime.time：表示时间的类。常用的属性有hour, minute, second, microsecond。
- datetime.datetime：表示日期+时间。
- datetime.timedelta：表示时间间隔，即两个时间点之间的长度，常常用来做时间的加减。
- datetime.tzinfo：与时区有关的相关信息

使用的最多的就是`datetime.datetime模块`

-------------------------
1.2. datetime.datetime
```
class datetime.datetime(year, month, day, hour=0, minute=0, second=0, microsecond=0, tzinfo=None, *, fold=0)
```
- datetime.datetime参数的取值范围，如果设定的值超过这个范围，那么就会抛出ValueError异常。

> MINYEAR <= `year` <= MAXYEAR
1 <=` month` <= 12
1 <= `day` <= number of days in the given month and year
0 <= hour < 24
0 <= minute < 60
0 <= second < 60
0 <= `microsecond` < 1000000

`【注意】`year，month，day是必须参数。

--------------------------------
1.3. datetime类方法

1.3.1. classmethod datetime.today()
获取今天的时间。
```
d = datetime.datetime.today()
print(d)
```
输出：
```
2019-06-16 13:14:19.660522
```
1.3.2. classmethod datetime.now(tz=None)
获取当前的时间。
```
import datetime
d =  datetime.datetime.now()
print(d)
----------
输出：2019-06-16 13:16:55.559174
```
1.3.3.classmethod datetime.fromtimestamp(timestamp, tz=None)
 用一个时间戳来生成datetime对象。
` 【注】`：时间戳需要为10位的
```
print(datetime.datetime.fromtimestamp(15301325475.18224))
------------
输出：2454-11-18 00:11:15.182240
```
-------------------------
1.4. datetime实例方法
> 这些方法大多是一个datetime对象能进行的操作。

1.4.1  关于datetime.date() 和 datetime.time()
获取datetime对象的日期或者时间部分。
```
print(datetime.datetime.now().date())
print(datetime.datetime.now().time())

-------------
输出：
2019-06-16
13:22:50.023383
```
1.4.2.  datetime.replace(year=self.year, month=self.month, day=self.day, hour=self.hour, minute=self.minute, second=self.second, microsecond=self.microsecond, tzinfo=self.tzinfo, * fold=0)
用于替换datetime对象的指定数据。
```
now = datetime.datetime.now()
print(now)
print(now.replace(year=2000))

------
输出：
2019-06-16 13:25:36.802372
2000-06-16 13:25:36.802372
```
1.4.3.  datetime.timestamp()
转换成时间戳。
```
print(datetime.datetime.now().timestamp())

---------
输出：1560662835.175996
```
----------------------------

1.5. datetime.timedelta

**以下是格式化的符号**：
在实际的使用中，常常会遇到这样的需求：需要给某个时间增加或减少一天，甚至是增加或减少一天三小时二十分钟。
那么在遇到这样的需求时，去计算时间戳是非常的麻烦的，所以datetime.timedelta这个模块使能够非常方便的对时间做加减。
```
class datetime.timedelta(days=0, seconds=0, microseconds=0, milliseconds=0, minutes=0, hours=0, weeks=0)
```
```

from datetime import timedelta

now = datetime.datetime.now()
print( now - timedelta(days=1))
print(now + timedelta(days=1))
print(now + timedelta(days=-1))
----------------
输出：
2019-06-15 13:33:34.826878
2019-06-17 13:33:34.826878
2019-06-15 13:33:34.826878
```
----------------------------
1.6. strftime() 和 strptime()
这是datetime中提供的两个方法，可以方便的把datetime对象转换成格式化的字符串或者把字符串转换成datetime对象。

- 由datetime转换成字符串：datetime.strftime()
-- strftime()是datetime对象的实例方法：
```
import datetime
print(datetime.datetime.now().strftime('%Y-%m-%d %H:%M:%S %f'))
-------
输出：
2019-06-16 13:36:20 580174
```
- 由字符串转换成datetime：datetime.datetime.strptime()
-- strptime()则是一个类方法：
```
import datetime
newsTime='Sun, 23 Apr 2017 05:15:05 GMT'
GMT_FORMAT = '%a, %d %b %Y %H:%M:%S GMT'
print(datetime.datetime.strptime(newsTime,GMT_FORMAT))
----
输出：
2017-04-23 05:15:05
```
---------------------------------
**【以下是格式化的符号】：**

- %y 两位数的年份表示（00-99）
- %Y 四位数的年份表示（000-9999）
- %m 月份（01-12）
 - %d 月内中的一天（0-31）
- %H 24小时制小时数（0-23）
- %I 12小时制小时数（01-12）
- %M 分钟数（00=59）
- %S 秒（00-59）
- %a 本地简化星期名称
- %A 本地完整星期名称
- %b 本地简化的月份名称
- %B 本地完整的月份名称
- %c 本地相应的日期表示和时间表示
- %j 年内的一天（001-366）
- %p 本地A.M.或P.M.的等价符
- %U 一年中的星期数（00-53）星期天为星期的开始
- %w 星期（0-6），星期天为星期的开始
 - %W 一年中的星期数（00-53）星期一为星期的开始
- %x 本地相应的日期表示
- %X 本地相应的时间表示
- %Z 当前时区的名称
 -%% %号本身
