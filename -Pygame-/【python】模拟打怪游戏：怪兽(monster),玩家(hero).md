游戏模式：
- 假设有怪兽（monster）和英雄（hero）两个角色，二者为敌对状态
假设两个角色初始血量为100，攻击力的伤害服从随机分布（7，17），二者相互攻击，判断谁获胜？
```py
# # 定义精灵
class Sprite:
    def __init__(self, name):
        self.blood = 100  # 假设初始血量为：100
        self.power = 12  # 假设基础攻击能力：12
        self.name = name

    def attack(self, monster):
        # 假设每一次攻击的伤害 服从随机分布（7,17）
        minus = rn.randrange(self.power - 5, self.power + 5)
        print(minus)
        if monster.has_living():
            monster.minus_blood(minus)
        print(monster.name + ' 剩余血量:\n' + str(monster.blood) + "\n")
        print('---------------------')
        # 输出剩余血量

    def minus_blood(self, num):
        self.blood -= num

    def has_living(self):  # 判断是否还有血量
        if self.blood > 0:
            return True
        return False
###########################################################
m = Sprite('【魔王】')  # 创建实例"魔王"
h = Sprite('【圣骑士】')  # 创建实例“圣骑士”
print(m.name + '的初始血量：(保密 @-@)')
print(h.name + '的初始血量：100')
while m.has_living() and h.has_living():
    print(m.name + ' 对 ' + h.name + '\n造成伤害:')
    m.attack(h)
    print(h.name + ' 对 ' + m.name + ' 造成伤害:')
    h.attack(m)

if m.has_living():
    print(m.name + ' 胜利!')
    print('【游戏结束】')
elif h.has_living():
    print(h.name + ' 胜利~!')
    print('【游戏结束】')
else:
    print(m.name + ' 和 ' + h.name + '均阵亡!')
    print('【游戏结束】')
```
内存分析：
![游戏模拟内存分析.png](https://upload-images.jianshu.io/upload_images/17476267-c78a89b7ab917c5c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

运行结果：
```
【魔王】的初始血量：(保密 @-@)
【圣骑士】的初始血量：100
【魔王】 对 【圣骑士】
造成伤害:
8
【圣骑士】 剩余血量:
92

---------------------
【圣骑士】 对 【魔王】 造成伤害:
9
【魔王】 剩余血量:
91
         +++++
# 为简短篇幅，此处缩略部分
         +++++
---------------------
【圣骑士】 对 【魔王】 造成伤害:
7
【魔王】 剩余血量:
24

---------------------
【魔王】 对 【圣骑士】
造成伤害:
15
【圣骑士】 剩余血量:
0

---------------------
【圣骑士】 对 【魔王】 造成伤害:
15
【魔王】 剩余血量:
9

---------------------
【魔王】 胜利!
【游戏结束】
```
##### 第一次进阶：
通过继承Sprite类，来尝试开发一个新的功能：
- 给玩家（Hero）角色加一个终结技能(英雄有10%的几率触发终结技能)；
- 为了保证游戏的体验感（不能厚此薄彼ლ(′◉❥◉｀ლ)），给怪兽（Monster）增加血量、增加基础伤害;

```
这里主要写了两个继承Sprite类的子类

class Hero(Sprite):
    def bug_attack(self,monster):
        monster.minus_blood(monster.blood)

    def attack(self,monster):
        super(Hero, self).attack(monster)
        num = rn.randint(0,4)
        if num == 1:
            print(self.name + ' 使用技能：终结')
            print(h.name + '对' + m.name + '造成伤害：%d'%(monster.blood))
            self.bug_attack(monster)

class Monster(Sprite):
    def __init__(self, name):
        self.blood = 200
        self.power = 15
        self.name = name
```
输出实例：
```
【魔王】的初始血量：(保密 @-@)
【圣骑士】的初始血量：100
【魔王】 对 【圣骑士】造成伤害:
12
【圣骑士】 剩余血量:
88

---------------------
【圣骑士】 对 【魔王】 造成伤害:
8
【魔王】 剩余血量:
192

---------------------
【魔王】 对 【圣骑士】造成伤害:
14
【圣骑士】 剩余血量:
74

---------------------
【圣骑士】 对 【魔王】 造成伤害:
13
【魔王】 剩余血量:
179

---------------------
【圣骑士】 使用技能：终结
【圣骑士】对【魔王】造成伤害：179
【圣骑士】 胜利~!
【游戏结束】
```
