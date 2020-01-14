Flask是一个用Python编写的Web应用程序框架。 
##### 用到的库和安装方法
【主角】： flask库
```python
安装方式： pip install flask 
```
【数据库】：flask-sqlalchemy库
这是flask的一个数据库ORM（对象关系映射）库，封装了一系列对数据库的操作API。
```
安装方式： pip install flask-sqlalchemy 
```
#####数据Model类

#####
#####
#####
#####
#####
#####
#####

WTForms支持的HTML标准字段
|字段类型 |说明|
|:-:|:-:|
 |BooleanField |复选框，值为  True  和  False  |
 |DateField  |文本字段，值为  datetime.date  格式
 | DateTimeField  |文本字段，值为  datetime.datetime  格式  |
 |DecimalField  |文本字段，值为  decimal.Decimal  |
 |FileField  |文件上传字段 |
 | HiddenField  |隐藏的文本字段 |
 |MultipleFileField  |多文件上传字段 |
 |FieldList  |一组指定类型的字段 |
  |FloatField | 文本字段，值为浮点数 |
 |FormField  |把一个表单作为字段嵌入另一个表单 |
 |IntegerField  |文本字段，值为整数 |
 | PasswordField  |密码文本字段 |
 | RadioField  |一组单选按钮 |
  |SelectField  |下拉列表 |
 | SelectMultipleField  |下拉列表，可选择多个值 |
 | SubmitField  |表单提交按钮 |
 | StringField  |文本字段 |
  |TextAreaField | 多行文本字段 |

---
【表】：WTForms验证函数
 | 验证函数  | 说明 | 
|:-:|:-:|
 |DataRequired |确保转换类型后字段中有数据|
 | Email  |验证电子邮件地址 |
 | EqualTo  |比较两个字段的值；常用于要求输入两次密码进行确认的情况 |
 | InputRequired  |确保转换类型前字段中有数据   |
|IPAddress  |验证 IPv4 网络地址  |
 | Length  |验证输入字符串的长度 MacAddress 验证 MAC 地址 | 
 | NumberRange  |验证输入的值在数字范围之内  |
 | Optional  |允许字段中没有输入，将跳过其他验证函数 |
 | Regexp  |使用正则表达式验证输入值 URL 验证   
|URL  |UUID 验证 UUID
  |AnyOf  |确保输入值在一组可能的值中 |
  |NoneOf | 确保输入值不在一组可能的值中 |
