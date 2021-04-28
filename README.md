# RAEDME

修改后的PVZ程序

添加了以下功能

* 自动收集阳光
* 添加直接植物种植接口
* 添加了自定义的属性

目前的demo中设定如果阳光大于等于100则自动在第3列自上而下种植豌豆

通过constants中的auto实现开启自动模式

## requirements

* numpy

## 接口说明

游戏的运行都在tool.py文件定义的Control类中实现，游戏的主函数位于Contol类的main函数中

```python
def main(self):
        while not self.done:
            self.event_loop()#鼠标事件
            self.update() #游戏更新
            pg.display.update()#刷新显示
            self.clock.tick(self.fps)#计时器
            self.my_update()#我们的属性更新
            self.ai_test()#暂定的AI控制接口
        print('game over')
```

### 基础属性

规划所需要的的属性存储在类属性

* self.sun_value
* self.has_zombie 
* self.zombie_state_all 
* self.has_bullet 
* self.bullet_state_all  

具体说明如下

#### 植物相关属性

* **plant_pos**  ：np.array(5,9)
* **plant_health**：np.array(5,9)
* **plant_frozen**：字典，键值为植物名称，值为是否能种植

```python
class plant_state_map():
    def __init__(self,plant_pos,plant_health):
        self.plant_pos = plant_pos
        self.plant_health = plant_health
        self.plant_frozen = {}
        # 植物信息(class)
            #成员变量
            # 地图上的植物分布：game.plant_state_all.plant_pos
            #               1、数据类型 np.array(5,9)
            #               2、植物种类：0表示没植物，1表示向日葵，2表示豌豆射手
            # 地图上的植物血量：game.plant_state_all.plant_health
            #               1、数据类型 np.array(5,9) 
            #状态栏中植物冷却情况
            #				1、存储为dict，键名为constant文件中相应的文件名
            #				值为bool量
            #				向日葵：c.SUNFLOWER
            #				豌豆射手：c.PEASHOOTER 
：
```

#### 僵尸相关属性

```python
class zombie_state_single():
    def __init__(self,name,x,y,health,state):
        self.name = name
        self.x = x
        self.y = y
        self.health = health
        self.state = state
```



#### 子弹相关属性

```python
class bullet_state_single():
    def __init__(self, name, x, y, state):
        self.name = name
        self.x = x
        self.y = y
        self.state = state
```



#### 属性更新

```python
def my_update(self):
        if self.state_name == c.LEVEL:
            if self.state.state == c.PLAY:
                self.sun_value = self.state.sun_value

                
                self.plant_state_all = self.state.plant_state_all

                # 是否有僵尸（0无，1有）
                self.has_zombie = self.state.has_zombie

                # 僵尸信息(list)，其中的每一个元素为一个class
                # eg:
                #     僵尸种类：self.zombie_state_all[0].name
                #     僵尸坐标x(像素表示）：self.zombie_state_all[0].x
                #     僵尸坐标y(0,1,2,3,4,5表示）：self.zombie_state_all[0].y
                #     僵尸血量：self.zombie_state_all[0].health
                #     僵尸状态：self.zombie_state_all[0].state（walk：正常行走；attack：被攻击）
                # 无僵尸则为[]
                self.zombie_state_all = self.state.zombie_state_all

                # 是否有子弹（0无，1有）
                self.has_bullet = self.state.has_bullet

                # 子弹信息(list)，其中的每一个元素为一个class
                # eg:
                #     子弹种类：self.bullet_state_all[0].name
                #     子弹坐标x(像素表示）：self.bullet_state_all[0].x
                #     子弹坐标y(像素表示）：self.bullet_state_all[0].y
                #     子弹状态：self.bullet_state_all[0].state（fly：正常飞行；explode：击中目标）
                # 无子弹则为[]
                self.bullet_state_all = self.state.bullet_state_all
```

​    

##  种植植物接口

tool.state.my_addPlant(map_x,map_y,name)

* name为植物名称，在constants.py文件中可以查找

* map_x，map_y为地图5*9的横纵坐标

里面加的越界检查有一点bug，注意不要超界给的坐标



