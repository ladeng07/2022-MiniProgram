# 2023年夏季《移动软件开发》实验报告



<center>姓名：黄乃煊  学号：21020007028</center>

| 姓名和学号？         |                                                              |
| -------------------- | ------------------------------------------------------------ |
| 本实验属于哪门课程？ | 中国海洋大学23夏《移动软件开发》                             |
| 实验名称？           | 实验5：高校新闻网                                            |
| 博客地址？           |                                                              |
| Github仓库地址？     | [ladeng07/2022-MiniProgram (github.com)](https://github.com/ladeng07/2022-MiniProgram) |

（备注：将实验报告发布在博客、代码公开至 github 是 **加分项**，不是必须做的）



## **一、实验目标**

1、综合所学知识创建完整的前端新闻小程序项目；能够在开发过程中熟练掌握真机预览、调试等操作。

## 二、实验步骤

### 新建项目

还是先从经典的模板项目开始修改；增加images，utils文件夹，同时添加page/game/game目录

![image-20230905160946764](https://cdn.lmark.cc/img/image-20230905160946764.png)

### 导航栏设计

使用珊瑚红底色+白色字体，得到的效果

![image-20230905161138645](https://cdn.lmark.cc/img/image-20230905161138645.png)



### 页面设计

#### 公共样式设计

为公共的container和title写样式，使用flex布局，定一定高和宽：

![image-20230905161600069](https://cdn.lmark.cc/img/image-20230905161600069.png)

#### 首页设计

首页包括关卡设计和标题，这里我们使用view组件

![image-20230905161944709](https://cdn.lmark.cc/img/image-20230905161944709.png)

接下来调整样式：

将图片宽度设定到50%，并且使用flex布局，同时图片四周设定一个20rpx的边框

![image-20230905162046651](https://cdn.lmark.cc/img/image-20230905162046651.png)

可以看到，此时测试的样式是很正常的，接下来进行游戏页的设计



#### 游戏页面设计

游戏页面需要用户点击首页的关卡列表,然后在新窗口中打开该页面。游戏页面包括游戏关卡标题、游戏画面、方向键和“重新开始”按钮。可能会新用到的组件有

- `<canvas>`画布，来显示游戏图片
- `<button>`来显示四个按钮，控制角色移动

由于没有设计跳转，这里我们手动添加编译模式

![image-20230905162516466](https://cdn.lmark.cc/img/image-20230905162516466.png)



先设计基本的布局：这里我们使用view嵌套布局，和button一起使用

![image-20230905162649194](https://cdn.lmark.cc/img/image-20230905162649194.png)

再来修改样式：

![image-20230905163139557](https://cdn.lmark.cc/img/image-20230905163139557.png)



现在按钮看起来有一点点奇怪，这大抵是渲染引擎的问题，只需要把app.json中的**"style": "v2",**删除即可

![image-20230905163229656](https://cdn.lmark.cc/img/image-20230905163229656.png)



至此，所有页面设计已经基本完成，接下来进行逻辑的设计。



### 逻辑设计

首先我们先给data.js添加游戏地图数据：

![image-20230905163422363](https://cdn.lmark.cc/img/image-20230905163422363.png)

然后可以开始首页逻辑设计



#### 首页逻辑设计

首页需要完成两份功能，一是展示关卡列表，二是点击图片能跳转到关卡界面

首先需要在首页的js文件中添加一些数据。

![image-20230905163701335](https://cdn.lmark.cc/img/image-20230905163701335.png)

然后在wxml文件中使用wx：for进行循环遍历展示：

![image-20230905163745414](https://cdn.lmark.cc/img/image-20230905163745414.png)

展示效果基本完成。接下来做跳转功能。

需要绑定一个chooseLevel函数来使用

![image-20230905170130405](https://cdn.lmark.cc/img/image-20230905170130405.png)

可以看到现在能正常跳转了。接下来写详情页的逻辑，在onload函数中监听level变量的变化，并设置变量。


![image-20230905170500883](https://cdn.lmark.cc/img/image-20230905170500883.png)

#### 游戏具体逻辑的实现

首先，现在game.js中初始一些信息：

```
var map = [
  [0, 0, 0, 0, 0, 0, 0, 0],
  [0, 0, 0, 0, 0, 0, 0, 0],
  [0, 0, 0, 0, 0, 0, 0, 0],
  [0, 0, 0, 0, 0, 0, 0, 0],
  [0, 0, 0, 0, 0, 0, 0, 0],
  [0, 0, 0, 0, 0, 0, 0, 0],
  [0, 0, 0, 0, 0, 0, 0, 0],
  [0, 0, 0, 0, 0, 0, 0, 0]
];
var box = [
  [0, 0, 0, 0, 0, 0, 0, 0],
  [0, 0, 0, 0, 0, 0, 0, 0],
  [0, 0, 0, 0, 0, 0, 0, 0],
  [0, 0, 0, 0, 0, 0, 0, 0],
  [0, 0, 0, 0, 0, 0, 0, 0],
  [0, 0, 0, 0, 0, 0, 0, 0],
  [0, 0, 0, 0, 0, 0, 0, 0],
  [0, 0, 0, 0, 0, 0, 0, 0]
];
//方块宽度
var w = 40;
//初始化游戏主角的行与列
var row = 0;
var col = 0;
```

初始化地图函数：

![image-20230905203958459](https://cdn.lmark.cc/img/image-20230905203958459.png)

这个函数首先从公共丽数文件 data.js 中读取对应关卡的游戏地图数据,然后使用双重for 循环对每一块地图数据进行解析,并更新当前游戏的初始地图数据、箱子数据以及游戏主角(小鸟)的所在位置。

绘制地图函数：

![image-20230905204439845](https://cdn.lmark.cc/img/image-20230905204439845.png)

这样游戏第一步运行所需的函数就完成了，接下来只需要在onload函数中调用这两个函数即可

![image-20230905204627444](https://cdn.lmark.cc/img/image-20230905204627444.png)

最后呈现的效果如下：

![image-20230905205059583](https://cdn.lmark.cc/img/image-20230905205059583.png)

接下来实现方向键的逻辑，四个方向键逻辑差不多，主要判断数组中对应方向的下一格是否为石头，如果是石头则不走；如果是箱子，且箱子的对应方向没有石头，则可以移动；否则不能移动。

截选一个向右移动函数：

![image-20230905205431761](https://cdn.lmark.cc/img/image-20230905205431761.png)

效果演示：

![image-20230905205500040](https://cdn.lmark.cc/img/image-20230905205500040.png)

![image-20230905205511480](https://cdn.lmark.cc/img/image-20230905205511480.png)

游戏能正常跑起来，接下来就是判断游戏是否胜利，需要写一个iswin函数，不停遍历整个8x8数组，以至于所有箱子都到正确的位置即可

![image-20230905205653780](https://cdn.lmark.cc/img/image-20230905205653780.png)

然后写一个检查胜利的函数，使用iswin函数，进行胜利的提示，同时需要在四个方向移动的函数里添加胜利检查函数。

![image-20230905205947733](https://cdn.lmark.cc/img/image-20230905205947733.png)

最后一步是添加重置游戏的逻辑，因为推箱子游戏很容死局，所以要这个

![image-20230905233105743](https://cdn.lmark.cc/img/image-20230905233105743.png)



## 三、程序运行结果

选关界面

![image-20230905233459300](https://cdn.lmark.cc/img/image-20230905233459300.png)

角色移动

![image-20230905233516282](https://cdn.lmark.cc/img/image-20230905233516282.png)



游戏胜利

![image-20230905233441462](https://cdn.lmark.cc/img/image-20230905233441462.png)

## 四、问题总结与体会

这门课程要求综合运用前端开发的各种知识，包括HTML、CSS、JavaScript等。通过实际项目的开发，我更深入地理解了这些知识点的应用；开发小程序时，我更加关注用户体验。良好的界面设计和流畅的交互能够提高用户的满意度，这是一个重要的考虑因素。总的来说，这门课程让我对移动应用开发有了更深入的了解，提高了我的前端开发技能和团队协作能力。通过不断练习和实践，我相信我可以进一步提升自己在移动应用开发领域的能力。