# 2023年夏季《移动软件开发》实验报告



<center>姓名：黄乃煊  学号：21020007028</center>

| 姓名和学号？         |                                  |
| -------------------- | -------------------------------- |
| 本实验属于哪门课程？ | 中国海洋大学22夏《移动软件开发》 |
| 实验名称？           | 实验2：天气查询小程序            |
| 博客地址？           |                                  |
| Github仓库地址？     |                                  |

（备注：将实验报告发布在博客、代码公开至 github 是 **加分项**，不是必须做的）



## **一、实验目标**

 1、掌握服务器域名配置和临时服务器部署；2、掌握 wx.request 接口的用法。



## 二、实验步骤

### 1.前置工作

要想使用和风天气API，必须得注册一个账号[实时天气 for API | 和风天气开发服务 (qweather.com)](https://dev.qweather.com/docs/api/weather/weather-now/)

![image-20230823104737505](https://cdn.lmark.cc/img/image-20230823104737505.png)

注册好账号之后，就可以新建一个项目来获取key了，这里选择的是免费的额度

![image-20230823105127970](https://cdn.lmark.cc/img/image-20230823105127970.png)

![image-20230823105202624](https://cdn.lmark.cc/img/image-20230823105202624.png)

根据官网的API使用说明，这里使用`https://devapi.qweather.com/v7/weather/now?location=101010100&key=YOUR_KEY`这个接口来获取数据，

如果要在小程序里使用API接口，需要将该接口的域名添加到白名单，需要登录微信公众平台的小程序后台添加：

![image-20230823112123218](https://cdn.lmark.cc/img/image-20230823112123218.png)

添加`https://free-api.heweather.com`

![image-20230823112317634](https://cdn.lmark.cc/img/image-20230823112317634.png)



### 2.创建小程序

按照上一个实验的步骤创建项目：

![image-20230823155425090](https://cdn.lmark.cc/img/image-20230823155425090.png)

然后把老师提供的天气图标放入images文件夹中备用

![image-20230823160414967](https://cdn.lmark.cc/img/image-20230823160414967.png)

### 3.视图设计

导航栏默认黑底白字，所以要修改一下`app.json`中标题的背景色

![image-20230823160649893](https://cdn.lmark.cc/img/image-20230823160649893.png)

页面上主要包含4个区域，具体内容解释如下：

- 区域1：地区选择器，用户可以自行选择查询的省，市，区；
- 区域2：显示当前城市的温度和天气状态的文字说明；
- 区域3：显示当前城市的天气图标；
- 区域4：分多行显示其他天气信息，例如湿度、气压、能见度和风向等。

计划使用如下组件：

- 页面整体:< view >组件，并定义 class='container'
- 区域 1:< picker >组件;
- 区域 2:< text >组件;
- 区域 3:<image >组件;
- 区域 4:< view >组件,并定义 class='detail';
- 区域4 内单元行:4 个< view >组件,并定义 class=bar';
- 区城 4 内单元格:每行 3 个< iew >组件,并定义 class='box'

我们先开始进行区域1的设计：

使用picker写一个区域选择器

![image-20230823204416980](https://cdn.lmark.cc/img/image-20230823204416980.png)

​	然后添加单行显示天气：

![image-20230823204835966](https://cdn.lmark.cc/img/image-20230823204835966.png)



再添加天气的图标显示

![image-20230823205122222](https://cdn.lmark.cc/img/image-20230823205122222.png)

接下来写多行的天气信息，这里写6个天气的信息

![image-20230823210017520](https://cdn.lmark.cc/img/image-20230823210017520.png)

使用不同的类名区分，然后再写样式，整体效果如下：

![image-20230823210046484](https://cdn.lmark.cc/img/image-20230823210046484.png)

基本的外观写好后，接下来改写具体实现的逻辑了，首先我们需要给picker组件绑定一个函数来监听：

![image-20230823210705471](https://cdn.lmark.cc/img/image-20230823210705471.png)

然后使用region变量来代替城市名，具体函数如下：

![image-20230823210738435](https://cdn.lmark.cc/img/image-20230823210738435.png)

选择器改变后，修改region的值，可以切换到国内任意省市区

最后一个获取天气数据的函数的编写，这是比较复杂的，因为需要使用wx.request函数，这里使用get请求，data作为请求的参数，然后成功后实行一个函数打印结果![image-20230823211140970](https://cdn.lmark.cc/img/image-20230823211140970.png)

经过一点小修改，可以获取到天气信息

![image-20230823213137841](https://cdn.lmark.cc/img/image-20230823213137841.png)

然后添加一个设置函数

![image-20230823214135889](https://cdn.lmark.cc/img/image-20230823214135889.png)

再把页面上的变量全部替换成now里面的变量，这样就能自动刷新了

![image-20230823214213210](https://cdn.lmark.cc/img/image-20230823214213210.png)

## 三、程序运行结果

![image-20230823214220721](https://cdn.lmark.cc/img/image-20230823214220721.png)

![image-20230823214231767](https://cdn.lmark.cc/img/image-20230823214231767.png)

## 四、问题总结与体会

在程序运行过程中会遇到![image-20230823211545579](https://cdn.lmark.cc/img/image-20230823211545579.png)

地址访问不了的情况，这是因为API接口不支持中文的查询，需要转换成ID，这时候需要使用util.js来转换

导入js得手动填写相对路径

![image-20230823214257759](https://cdn.lmark.cc/img/image-20230823214257759.png)

同时由于接口的更新，我需要重新获取定义的变量，这也是比较麻烦的，但是成品做出来之后还是很有颜值的，对于小程序开发的喜爱提升了

