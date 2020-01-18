【写在前面】：本篇文章呢，是使用**pygame 通过面向对象的方式**实现的“飞机大战”的**完整程序**的串讲。

【特点介绍】
- 设置游戏背景；

- 设置游戏背景音乐；

- 设置了游戏名称“外星人入侵”；

- 设置了积分装置（左上角）；

- 设置了玩家的生命值（右上角）；

- 设置了两种道具（powerup）和（liveup）；
- 设置了我方战机三种攻击模式（吃了道具后，模型转变（持续时间：5秒））；
- 设置了敌机的随机产生，以及敌机子弹的随机发射；
- 我方战机死亡后，游戏界面转变；
- 我方战机死亡后，可选择【重新游戏】和【游戏结束】
- ......

---
【是否拭目以待了呢？】
先来看一下游戏的运行结果吧！
![游戏开始](https://upload-images.jianshu.io/upload_images/17476267-bc00a6284a012225.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
---
火力升级【power up】
![【power up】](https://upload-images.jianshu.io/upload_images/17476267-ff9991a7b5ef57f3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

火力升级【power up（MAX）】
![【power up（MAX）】](https://upload-images.jianshu.io/upload_images/17476267-e41c2b843455384c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
【死亡后界面跳转】
![死亡后界面跳转-图例](https://upload-images.jianshu.io/upload_images/17476267-dec9fb3278e687bd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
看着酱紫的界面，是否有玩一局的冲动呢？
---

再一次游戏【once again】

![再一次游戏-图例](https://upload-images.jianshu.io/upload_images/17476267-875af00ee3c07e4a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

---
下面进入正题

##### 先提一下：什么是游戏循环?
每个游戏的核心都是一个循环，将其称为“游戏循环”。这个循环一直在不断运行，一遍又一遍地完成游戏工作所需的所有事情。每次循环显示一次游戏当前画面，称为帧。

---

【本文主要有以下10个模块】
- App.py文件

- hero.py文件
- Npc.py文件
- bullet.py文件
- prop.py文件
- hit.py文件
- information.py文件
- music.py文件
- setting.py文件
- setup.py文件

---
##### 关于App.py文件
【作用如下】
- 启动pygame；

- 确定游戏界面；
-  设置帧率
- 设置游戏名称；
- 设置游戏背景；
- 设置游戏背景音乐；
- 【调用各个模块来丰富游戏功能】
- 游戏关闭；
```
# @Author  : Zurich.Alcazar
# @Email   : 1178824808@qq.com
# @File    : App.py
# @Software: PyCharm

import pygame
import sys
import hero
import os.path as path
import setting
from information import DataBus
from Npc import EnermyFactory
from Npc import Button
from prop import PropFactory
import hit
# import music

class App:

    def start(self):
        pygame.init()    # 启动pygame
        pygame.mixer.init()
        # 确定游戏界面
        screen = pygame.display.set_mode((400, 600))

        # 设置帧率
        clock = pygame.time.Clock()
        bus = DataBus()
        # 设置游戏名称
        pygame.display.set_caption("外星人入侵")
        # 设置游戏背景
        # bg_img = pygame.image.load(path.join(setting.img_folder, "background.png")).convert()
        bg_img = pygame.image.load(path.join(setting.img_folder, "789.jpg")).convert()
        # bg_img = pygame.image.load(path.join(setting.img_folder, "888.png")).convert()
        screen.blit(bg_img,(0,0))

        # 设置游戏背景音乐
        bgm = pygame.mixer.Sound(path.join(setting.snd_folder,'game_music.ogg'))
        bgm.set_volume(0.5)
        bgm.play(loops = -1)
        # 调用

        ef = EnermyFactory()
        pp = PropFactory()
        transColor = pygame.Color(0, 0, 0)
        player_img = pygame.image.load(path.join(setting.img_folder, "me1.png")).convert()

        player_mini_img = pygame.transform.scale(player_img, (25, 30))
        player_mini_img.set_colorkey(transColor)

        game_font = pygame.font.SysFont('arial', 24, True)


        # 让渲染结果一次显示
        pygame.display.flip()

        while True:
            screen.blit(bg_img, (0, 0))
            clock.tick(30)
            if not bus.is_game_over:
                # 制造敌机
                ef.generate_enermy()
                # 制造道具
                pp.generate_bomb_prop()
                pp.generate_bullet_prop()

                # 更新坐标（核心）
                bus.all_sprites.update()
                # 映射到屏幕
                bus.all_sprites.draw(screen)
                # 碰撞检测: 玩家子弹和敌机
                bus.game_helper.collision()
                screen.blit(game_font.render(u'%d' % bus.score, True, [255, 0, 0]), [20, 20])
                hero.Hero.draw_lives(screen, 300, 20, 3, player_mini_img)

                # 让渲染结果一次显示
                pygame.display.flip()

            if bus.is_game_over:
                bg_img = pygame.image.load(path.join(setting.img_folder, "888.png")).convert()
                screen.blit(bg_img, (0, 0))
                bus.game_helper.draw_game_over_view()

            bus.all_sprites.draw(screen)
            pygame.display.flip()


            for event in pygame.event.get():
                if event.type == pygame.QUIT:  # 游戏关闭
                    sys.exit()

App().start()
```
---
#####  关于hero.py文件
【作用】：定义英雄的各种功能（移动、发射子弹等等）
- 定义初始位置；

- 定义子弹发射音效；
- 弹延时（效果）；
- 我方战机生命值；
- 是否触碰游戏边界【检查】；
- 等级提升，火力升级【powerup】；
- 战机隐藏；
- 我方战机爆炸（此功能尚未实现）；

```
# @Author  : Zurich.Alcazar
# @Email   : 1178824808@qq.com
# @File    : hero.py
# @Software: PyCharm
import pygame
from pygame.sprite import Sprite
import os.path as path
import setting
from bullet import HeroBullet
from bullet import HeroBullet2
import information as info

class Hero(Sprite):
    def __init__(self):
        super().__init__()

        image = pygame.image.load(path.join(setting.img_folder,"me1.png"))
        self.image = pygame.transform.scale(image,(51,60))
        # 定义初始位置
        self.rect = self.image.get_rect()
        self.rect.center = (setting.SCREEN_WIDTH - 200,setting.SCREEN_HEIGHT - 80)
        self.bus = info.DataBus()
        # 定义子弹发射音效
        self.shoot_sud = pygame.mixer.Sound(path.join(setting.snd_folder,"bullet.wav"))
        self.shoot_sud.set_volume(0.8)
        # self.clock = pygame.time.Clock()
        # 子弹延时（效果）
        self.shoot_delay = 500
        self.last_shoot_time = pygame.time.get_ticks()
        self.power = 1
        self.POWERUP_TIME = 5000
        self.power_time = pygame.time.get_ticks()
        # 我方战机生命值
        self.lives = 3
        self.hidden = False
        self.hide_timer = pygame.time.get_ticks()

        player_img = pygame.image.load(path.join(setting.img_folder, "me1.png")).convert()
        player_mini_img = pygame.transform.scale(player_img, (25, 30))
        player_mini_img.set_colorkey()



    def update(self, *args):
        speed = 10
        keystate = pygame.key.get_pressed()
        if (keystate[pygame.K_LEFT] or keystate[pygame.K_a]):
            self.rect.x -= speed
        if keystate[pygame.K_RIGHT] or keystate[pygame.K_d]:
            self.rect.x += speed
        if keystate[pygame.K_UP] or keystate[pygame.K_w]:
            self.rect.y -= speed
        if keystate[pygame.K_DOWN] or keystate[pygame.K_s]:
            self.rect.y += speed
        # if keystate[pygame.K_SPACE]:  # 空格键 射击
        if self.power == 1:
            self.shoot()
        if self.power == 2:
            self.powershoot()
        if self.power >= 3:
            self.superpower()
        # 战机复活影藏
        if self.hidden and pygame.time.get_ticks() - self.hide_timer > 1000:
            self.hidden = False
            self.rect.centerx = setting.SCREEN_WIDTH / 2
            self.rect.bottom = setting.SCREEN_HEIGHT - 80


        # 边界检查
        if self.rect.right > setting.SCREEN_WIDTH:
            self.rect.right = setting.SCREEN_WIDTH
        if self.rect.left < 0:
            self.rect.left = 0
        if self.rect.top < 0:
            self.rect.top = 0
        if self.rect.bottom > setting.SCREEN_HEIGHT:
            self.rect.bottom = setting.SCREEN_HEIGHT

        if self.power >= 2 and pygame.time.get_ticks() - self.power_time > self.POWERUP_TIME:
            self.power -= 1
            self.power_time = pygame.time.get_ticks()

    # 生命值减少
    def Lives(self):
        self.lives -= 1

    # 等级提升
    def powerup(self):
        self.power += 1
        self.power_time = pygame.time.get_ticks()

    # 战机隐藏
    def hide(self):
        # hide the player temporarily

        self.hidden = True
        self.hide_timer = pygame.time.get_ticks()
        self.rect.center = (setting.SCREEN_WIDTH/ 2, setting.SCREEN_HEIGHT + 200)



    def shoot(self):
        now = pygame.time.get_ticks()
        if now - self.last_shoot_time > self.shoot_delay:
            self.last_shoot_time = now

            bullet = HeroBullet(self.rect.centerx, self.rect.top)
            self.shoot_sud.play()
            self.bus.add_sprite(bullet)
            self.bus.add_hero_bullet(bullet)

    # 火力升级
    def powershoot(self):
        now = pygame.time.get_ticks()
        if now - self.last_shoot_time > self.shoot_delay:
            self.last_shoot_time = now

            bullet1 = HeroBullet(self.rect.left + 15, self.rect.top)
            bullet2 = HeroBullet(self.rect.right - 15, self.rect.top)
            bullet3 = HeroBullet(self.rect.centerx, self.rect.top - 5)

            # 发射子弹时的音效
            self.shoot_sud.play()

            self.bus.add_sprite(bullet1)
            self.bus.add_sprite(bullet2)
            self.bus.add_sprite(bullet3)

            self.bus.add_hero_bullet(bullet1)
            self.bus.add_hero_bullet(bullet2)
            self.bus.add_hero_bullet(bullet3)

    def superpower(self):
        now = pygame.time.get_ticks()
        if now - self.last_shoot_time > 50:
            self.last_shoot_time = now

            bullet3 = HeroBullet2(self.rect.centerx - 32, self.rect.top)
            self.shoot_sud.play()
            self.bus.add_sprite(bullet3)
            self.bus.add_hero_bullet(bullet3)

    def draw_lives(surf, x, y, lives, img):
        for i in range(lives):
            img_rect = img.get_rect()
            img_rect.x = x + 30 * i
            img_rect.y = y
            surf.blit(img, img_rect)


class HeroLive(Sprite):
    """
    我方战机爆炸
    """
    def __init__(self, name, num, center):
        super().__init__()
        self.animation = []

```
【解释一下】：只要是与我方战机挂钩的功能，都可在此模块添加。

---
上面写完了有关我方战机的各种功能，下面就该【邪恶角色】外星人出场了..
#####  关于Npc.py文件

【作用】：随机生成敌机。

【小技巧介绍】：

```
class EnermyFactory:
```
【敌机工厂类】：来实现敌机的随机产生

```
class Explosion(Sprite):
```
【爆炸效果类】：实现敌机与子弹碰撞后的爆炸效果。
【此代码块如下】
```
# @Author  : Zurich.Alcazar
# @Email   : 1178824808@qq.com
# @File    : Npc.py
# @Software: PyCharm

import random
import pygame

from pygame.sprite import Sprite
import os.path as path

import setting
import information as info
from bullet import NpcBullet

class commonEnermy(Sprite):
    def __init__(self,x,y):
        super().__init__()
        image = pygame.image.load(path.join(setting.img_folder,"enemy1.png"))
        self.image = pygame.transform.scale(image,(40,30))
        self.bus = info.DataBus()
        self.rect = self.image.get_rect()
        self.rect.centerx = x
        self.rect.centery = y
        self.shoot_time = pygame.time.get_ticks()
        self.delay_shoot = random.randint(500,3500)

    def update(self, *args):
        speed = 4
        self.rect.y += speed
        # 可设计其他轨迹
        self.shoot()
        if self.rect.y > setting.SCREEN_HEIGHT:
            self.kill()

    def shoot(self):
        now =  pygame.time.get_ticks()
        if now - self.shoot_time > self.delay_shoot:
            self.delay_shoot = random.randint(500, 3500)
            self.shoot_time = now
            bullet = NpcBullet(self.rect.centerx,self.rect.bottom)
            self.bus.add_sprite(bullet)
            self.bus.add_npc_bullet(bullet)

class EnermyFactory:
    def __init__(self):
        self.bus = info.DataBus()
        # 普通敌军生成时间
        self.last_common_time = pygame.time.get_ticks()

    def generate_comon_enermy(self):
        x = random.randint(26,setting.SCREEN_WIDTH - 26)
        y = random.randint(-100,80)
        common_enermy = commonEnermy(x,y)
        self.bus.add_sprite(common_enermy)
        self.bus.add_enermy(common_enermy)

    def generate_enermy(self):
        now = pygame.time.get_ticks()
        if now - self.last_common_time > 800:
            self.generate_comon_enermy()
            self.last_common_time = now

class Explosion(Sprite):
    animations = dict()
    def __init__(self, who, center):
        super().__init__()
        self.aninmation = []
        self.load()

        if who == "enermy":
            self.animation = Explosion.animations['enemy_main']
        self.frame = 0
        self.image = self.animation[self.frame]
        self.rect = self.image.get_rect()
        self.rect.center = center
        # 上一帧图片的显示时间
        self.last_frame_time = pygame.time.get_ticks()

    def load(self):
        # 加载敌机爆炸资源
        if Explosion.animations.get("enemy_main") is None:
            enemy_explosion_res = []
            for i in range(0, 4):
                filename = "enemy1_down{}.png".format(i+1)
                img = pygame.image.load(path.join(setting.img_folder,filename))
                enemy_explosion_res.append(img)
            Explosion.animations["enemy_main"] = enemy_explosion_res

    def update(self, *args):
        now = pygame.time.get_ticks()
        if now - self.last_frame_time > 100:
            if self.frame == len(self.animation) - 1:
                self.kill()
            else:
                self.frame += 1
                self.image = self.animation[self.frame]

class HeroExplosion(Sprite):
    animations = dict()
    def __init__(self, who, center):
        super().__init__()
        self.aninmation = []
        self.load()

        if who == "hero":
            self.animation = Explosion.animations['enemy_main']
        self.frame = 0
        self.image = self.animation[self.frame]
        self.rect = self.image.get_rect()
        self.rect.center = center
        # 上一帧图片的显示时间
        self.last_frame_time = pygame.time.get_ticks()

    def load(self):
        # 加载爆炸资源
        if Explosion.animations.get("enemy_main") is None:
            enemy_explosion_res = []
            for i in range(0, 4):
                filename = "me_destroy_{}.png".format(i+1)
                img = pygame.image.load(path.join(setting.img_folder,filename))
                enemy_explosion_res.append(img)
            Explosion.animations["enemy_main"] = enemy_explosion_res

    def update(self, *args):
        now = pygame.time.get_ticks()
        if now - self.last_frame_time > 100:
            if self.frame == len(self.animation) - 1:
                self.kill()
            else:
                self.frame += 1
                self.image = self.animation[self.frame]


class Button(Sprite):
    def __init__(self,img_name,offset_y):
        super(Button, self).__init__()
        self.image = pygame.image.load(path.join(setting.img_folder,img_name))
        self.rect = self.image.get_rect()
        self.rect.center = setting.SCREEN_WIDTH / 2, (setting.SCREEN_HEIGHT/2) + offset_y

    @staticmethod
    def check_click(button):
        pressed = pygame.mouse.get_pressed()
        print(pressed)
        if pressed == (1, 0, 0):
            print("左键点击")
            pos = pygame.mouse.get_pos()
            button_x = pos[0]
            button_y = pos[1]

            if button.rect.left < button_x < button.rect.right and button.rect.top < button_y < button.rect.bottom:
                return True
            return False
```
---
那么敌军和我方英雄都存在了，接下来，我们介绍一下关于【子弹发射的问题】
#####bullet.py
【作用】：导入子弹、定义子弹初始位置、以及移动轨迹。

【代码块】
```
# @Author  : Zurich.Alcazar
# @Email   : 1178824808@qq.com
# @File    : bullet.py
# @Software: PyCharm

import pygame
import os.path as path
import setting
from pygame.sprite import Sprite

class HeroBullet(Sprite):
    def __init__(self,x,y):
        super(HeroBullet, self).__init__()
        # 导入子弹
        self.image = pygame.image.load(path.join(setting.img_folder,"bullet2.png"))
        # 定义子弹初始位置
        self.rect = self.image.get_rect()
        self.rect.centerx = x
        self.rect.bottom = y

    def update(self, *args):
        self.rect.y -= 10
        if self.rect.bottom < 0:
            self.kill()

class HeroBullet2(Sprite):
    def __init__(self,x,y):

        super(HeroBullet2, self).__init__()
        # 导入子弹
        self.image = pygame.image.load(path.join(setting.img_folder,"bullet2.png"))
        # 定义子弹初始位置
        self.rect = self.image.get_rect()
        # self.rect.center = center
        self.rect.centerx = x
        self.rect.bottom = y
        self.speedx = 0
        self.d = 6

    def update(self, *args):

        self.rect.y -= 4
        if self.speedx < -12:
            self.d = 3
        if self.speedx > 12:
            self.d = -3
        self.speedx += self.d
        self.rect.x += self.speedx

        if self.rect.bottom < 0:
            self.kill()

class NpcBullet(Sprite):
    def __init__(self,x,y):
        super(NpcBullet, self).__init__()
        # 导入敌机子弹
        image = pygame.image.load(path.join(setting.img_folder,"bullet1.png"))
        self.image = pygame.transform.scale(image, (4, 8))
        self.rect = self.image.get_rect()
        self.rect.centerx = x
        self.rect.bottom = y

    def update(self, *args):
        self.rect.y += 7
        if self.rect.centery < 0:
            self.kill()
```
【解释】：关于子弹这一块，其实是最简单的啦。子弹能发射了，那接下来就可以加载以下道具了。

---
#####关于prop.py文件
【分类】

- 火力提升道具 
>class Bulletprop(Sprite):
- 加血道具
>class Bombprop(Sprite):

【小技巧】

```
class PropFactory:
```
通过调用【道具工厂类】来实现道具的生成。
【代码】
```
import random
import pygame
from pygame.sprite import Sprite
import os.path as path
import setting
import hit
import information as info
import math




class Bombprop(Sprite):
    # 定义加血道具
    def __init__(self,x,y):
        super().__init__()
        image = pygame.image.load(path.join(setting.img_folder,"bomb_supply.png"))
        self.image = pygame.transform.scale(image,(20,35))

        self.rect = self.image.get_rect()
        self.rect.centerx = x
        self.rect.centery = y

    def update(self, *args):
        speed = 2
        self.rect.y += speed
        if self.rect.y > setting.SCREEN_HEIGHT:
            self.kill()


class Bulletprop(Sprite):
    # 定义加火力道具
    def __init__(self,x,y):
        super(Bulletprop, self).__init__()
        prop_image = pygame.image.load(path.join(setting.img_folder,"bullet_supply.png"))
        self.image = pygame.transform.scale(prop_image, (20, 35))
        self.rect = self.image.get_rect()
        self.rect.centerx = x
        self.rect.centery = y

    def update(self, *args):
        speed = 2
        self.rect.y += speed
        if self.rect.y > setting.SCREEN_HEIGHT:
            self.kill()

class PropFactory:
    def __init__(self):
        self.bus = info.DataBus()
        # 普通敌军生成时间
        self.prop_time = pygame.time.get_ticks()
        self.bullet_time = pygame.time.get_ticks() - 500

    def generate_comon_prop(self):
        # 定义加血道具位置
        x = random.randint(26, setting.SCREEN_WIDTH - 26)
        y = random.randint(26, 180)
        bomb_supply = Bombprop(x, y)
        self.bus.add_sprite(bomb_supply)
        self.bus.add_bomb_prop(bomb_supply)

    def bullet_prop(self):
        # 定义火力道具位置
        x = random.randint(26, setting.SCREEN_WIDTH - 26)
        y = random.randint(26, 180)

        bullet_supply = Bulletprop(x, y)
        self.bus.add_sprite(bullet_supply)
        self.bus.add_bullet_prop(bullet_supply)

    # 加血道具出现时间间隔
    def generate_bomb_prop(self):
        now = pygame.time.get_ticks()
        if now - self.prop_time > 2000 :
            self.generate_comon_prop()
            self.prop_time  = now

    def generate_bullet_prop(self):
        it = pygame.time.get_ticks()
        if it - self.bullet_time > 3000:
            self.bullet_prop()
            self.bullet_time = it

```
【注意事项】：在设置道具产生时，会很容易出现满屏的道具问题，如果自己在编程中有遇到，(请自行百度解决，或者参考本篇文章)。

---
【插播小广告，嘻嘻】
【关于爆炸效果】：其实游戏设置中的爆炸效果，是由几张图片在极短的时间内闪过产生的视觉效果。

下面来讲一讲碰撞检测。

---

#### 关于hit.py文件

【碰撞检测分类】
- 子弹和敌军碰撞检测；

- 敌机与战机碰撞检测；
- 敌机子弹和战机碰撞检测；
- 战机和火力道具碰撞检测；
- 战机和加血道具碰撞检测；

【代码】：此块代码中有许多被 住掉的（#...）如果有兴趣的话，可以尝试尝试。
```
import pygame
import sys
import setting
import os.path as path
import hero
from pygame.sprite import Sprite
# from Npc import EnermyFactory
import information as info
from Npc import Explosion
# from Npc import HeroExplosion
from Npc import Button


class Hit(Sprite):
    """
    碰撞类
    """
    def __init__(self):
        super(Hit, self).__init__()
        self.score = 0
        self.power = 1
        self.die_music = pygame.mixer.Sound(path.join(setting.snd_folder, "enemy1_down.wav"))
        self.get_bullet = pygame.mixer.Sound(path.join(setting.snd_folder,"get_bullet.wav"))
        self.bus = info.DataBus()


class GameHelper:
    def __init__(self):
        self.bus = info.DataBus()

    def collision(self):
        """
        全局碰撞检测
        :return:
        """
        # 子弹和敌军碰撞检测
        hits = pygame.sprite.groupcollide(self.bus.hero_bullets, self.bus.enermys, True, True)
        if hits:
            self.handle_collision(hits)

        #  敌机与战机碰撞
        enermys = self.bus.enermys.sprites()
        enermys_hit = pygame.sprite.spritecollide(self.bus.hero, self.bus.enermys, True, pygame.sprite.collide_circle_ratio(0.7))
        # for e in enermys:
        #     pygame.sprite.collide_circle_ratio(0.7)
        #     if pygame.sprite.collide_circle(self.bus.hero, e):

        if enermys_hit:
            # for b, e in hits.items():
            #     for eson in e:
            #         explosion = HeroExplosion('hero', eson.rect.center)
            #         self.bus.add_sprite(explosion)
            # hero.Hero.hide(self.bus.hero)
            # hero.Hero.Lives(self.bus.hero)
            # hero.Hero.shield = 100
                self.bus.is_game_over =True
                print("Game Over...")

        # 敌机子弹和战机碰撞
        enermys_bullets = self.bus.enermy_bullets.sprites()
        for i in enermys_bullets:
            if pygame.sprite.collide_rect(self.bus.hero, i):
                # # 敌人爆炸动画
                # for b, e in hits.items():
                #     for eson in e:
                #         explosion = HeroExplosion('hero', eson.rect.center)
                #         self.bus.add_sprite(explosion)
                # hero.Hero.hide(self.bus.hero)
                # hero.Hero.Lives(self.bus.hero)
                # hero.Hero.shield = 100
                self.bus.is_game_over = True
                print("Game Over...")


        # 战机和火力道具碰撞
        bullet_prop = self.bus.bullet_props.sprites()
        bullet_hit = pygame.sprite.spritecollide(self.bus.hero, self.bus.bullet_props,True, pygame.sprite.collide_circle_ratio(0.7))
        # for k in bullet_prop:
        #     pygame.sprite.collide_circle_ratio(0.7)
        #     if pygame.sprite.collide_circle(self.bus.hero, k):
        #         hero.Hero.powerup(self.bus.hero)
        if bullet_hit:
            hero.Hero.powerup(self.bus.hero)


        # 战机和加血道具碰撞
        bomb_prop = self.bus.bomb_props.sprites()
        bomb_hit = pygame.sprite.spritecollide(self.bus.hero, self.bus.bomb_props, pygame.sprite.collide_circle_ratio(0.8))
        # if bomb_hit:
        #     hero.Hero.(self.bus.hero)

    def handle_collision(self, hits):
        # 加分
        self.bus.add_score(10 * len(hits))
        # 敌人死亡声音
        self.bus.m.play_die_music()

        # 敌人爆炸动画
        for b, e in hits.items():
            for eson in e:
                explosion = Explosion('enermy', eson.rect.center)
                self.bus.add_sprite(explosion)

    def draw_game_over_view(self):
        self.bus.all_sprites.empty()

        game_over_button = Button("gameover.png", -40)
        begin_button = Button("again.png",40)
        self.bus.all_sprites.add(game_over_button)
        self.bus.all_sprites.add(begin_button)

        if Button.check_click(game_over_button):
            sys.exit()

        if Button.check_click(begin_button):
            self.bus.reset()
            self.bus.is_game_over = False

```
#### 关于information.py文件
这个文件特别重要。
【作用】
- 初始化英雄（...）；

- 将游戏中的各个元素（敌机、子弹、道具等）分门别类的“存”起来；

【代码】

 ```
# @Author  : Zurich.Alcazar
# @Email   : 1178824808@qq.com
# @File    : information.py
# @Software: PyCharm
import pygame
import hero
import music
import hit
import setting
# from Npc import Explosion

class DataBus:
    instance = None


    def __new__(cls, *args, **kwargs):
        if DataBus.instance is None:
            DataBus.instance = super().__new__(cls)
            DataBus.instance.reset()


        return DataBus.instance

    def reset(self):

        self.score = 0
        self.is_game_over = False

        self.all_sprites = pygame.sprite.Group()
        self.hero_bullets = pygame.sprite.Group()

        self.enermy_bullets = pygame.sprite.Group()
        self.heros = pygame.sprite.Group()
        self.enermys = pygame.sprite.Group()
        self.bomb_props = pygame.sprite.Group()
        self.bullet_props = pygame.sprite.Group()
        self.m = music.Music()
        self.game_helper = hit.GameHelper()
        # self.game_over_button = Npc.Button("gameover.png")
        # self.game_over_button_clicked = False

        # 初始化英雄
        self.__init_hero()

    def __init_hero(self):
        """
        初始化英雄
        :return:
        """
        self.hero = hero.Hero()
        self.add_sprite(self.hero)

    def add_sprite(self,sprite):
        self.all_sprites.add(sprite)

    def remove_sprite(self,sprite):
        self.all_sprites.remove(sprite)

    def add_hero_bullet(self,hero_bullet):
        self.hero_bullets.add(hero_bullet)

    def add_npc_bullet(self,npc_bullet):
        self.enermy_bullets.add(npc_bullet)

    def add_hero(self,hero):
        self.heros.add(hero)

    def add_enermy(self,enermy):
        self.enermys.add(enermy)

    def add_bomb_prop(self,prop):
        self.bomb_props.add(prop)

    def add_bullet_prop(self,prop):
        self.bullet_props.add(prop)

    def add_score(self,score):
        self.score += score
        num_score = self.score

        print("当前得分：%d" % self.score)


    def draw_text(self, screen):
        font1 = pygame.font.Font(16, True)
        screen.blit(font1.render(u'当前得分：%d' % self.score, True,'red'), [20, 20])

```
【解释一下】：这个文件就是拿来存储数据的。

---
####关于music.py文件
【作用】：配置音乐

【代码】：
```
# @Author  : Zurich.Alcazar
# @Email   : 1178824808@qq.com
# @File    : music.py
# @Software: PyCharm
import pygame
import sys
import setting
import os.path as path


class Music:

    singleton = None
    def __new__(cls, *args, **kwargs):
        if Music.singleton is None:
            Music.singleton = super().__new__(cls, *args, **kwargs)
            Music.singleton.initial()

        return Music.singleton

    def initial(self):
        self.die_music = pygame.mixer.Sound(path.join(setting.snd_folder, "enemy1_down.wav"))
        self.bgm = pygame.mixer.Sound(path.join(setting.snd_folder, 'game_music.ogg'))
        self.shoot_sud = pygame.mixer.Sound(path.join(setting.snd_folder, "bullet.wav"))
        self.enermy_die_music = pygame.mixer.Sound(path.join(setting.snd_folder, "enemy1_down.wav"))

        self.set_config()

    def set_config(self):
        """
        配置音乐
        :return:
        """
        self.bgm.set_volume(0.6)

    def play_die_music(self):
        self.die_music.play()

    def play_bgm(self):
        self.bgm.play(loops=-1)

    def play_shoot_sud(self):
        self.shoot_sud.play()

    def play_enmery_die_music(self):
        self.enermy_die_music.play()

```
---
#### 关于setting.py文件

【作用】：设置工程目录，并且设置游戏的边界。

```
import os.path as path
import pygame

SCREEN_WIDTH = 400
SCREEN_HEIGHT = 600

# 工程目录
print(__file__)
project_folder = path.dirname(__file__)

img_folder = path.join(project_folder,"plane/images")
snd_folder = path.join(project_folder,"plane/sound")
print(img_folder)

```
---
### 总结一下
本篇文章主要是理一下使用pygame面向对象编程的思路，说一下本文中程序的缺点吧，此程序其实部分功能没有实现，例如：关于加血道具那一块、以及我方战机的爆炸效果等。不把这个做的尽善尽美，也是为了看这篇文章的你能够有自己的【Idea】。能够学着自己去实现一些小小的功能。

