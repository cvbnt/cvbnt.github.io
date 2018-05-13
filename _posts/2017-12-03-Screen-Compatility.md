
---

layout: post  

title: Screen Compatility

date: 2017-12-03 16:00:00 +0800 

categories: AS  

---

## 1、基本概念

PX：像素

屏幕尺寸：对角线长度，单位为inch(英尺)，1inch=2.54cm

DPI：密度值，或者叫每英寸像素值，例如有一款5英寸屏幕手机，分辨率为1920×1080，对角线像素值为
$$
\sqrt{(1920)²+(1080)²}
$$
约为2203，2205÷5约为440，DPI就为440。

DP：也可写作DIP，密度无关像素，虚拟像素单位，PX = (DP × DPI)/ 160 ，DP=(160×PX)/DPI

## 2、固定密度

对特定的分辨率，并非按照实际密度换算，Android采用固定密度换算

![normal](https://cvbnt.github.io/cvbnt.github.io/assets/images/Screen Compatility/normal.PNG)

简单转换

![transfer](https://cvbnt.github.io/cvbnt.github.io/assets/images/Screen Compatility/transfer.PNG)

注：当系统字号设置为普通时，SP和DP值等同

## 3、使用DP进行屏幕适配

1、安装插件

Settings->Plugins->Browse repositories->ScreenMatch

2、将screenMatchDP.bat和screenMatchDP.jar放入app\src\main中

[项目地址](https://github.com/mengzhinan/PhoneScreenMatch)

大部分手机DP值为360，以360值为基准，screenMatchDP.bat里面的代码为： 

```bash
java -jar %~dp0\screenMatchDP.jar 360 384 400 411 533 640 720 768 820  
pause  
```

默认第一个值为基准值，默认为360，后面的为要适配的其他DP值

3、app\src\main\res\values需要一份dimens.xml文件，内容如下：

```xml
<resources>  
    <!-- Default screen margins, per the Android Design guidelines. -->  
    <dimen name="activity_horizontal_margin">16dp</dimen>  
    <dimen name="activity_vertical_margin">16dp</dimen>  
      
    <dimen name="dp_m_60">-60dp</dimen>  
    <dimen name="dp_m_30">-30dp</dimen>  
    <dimen name="dp_m_20">-20dp</dimen>  
    <dimen name="dp_m_10">-10dp</dimen>  
    <dimen name="dp_m_5">-5dp</dimen>  
    <dimen name="dp_0.1">0.1dp</dimen>  
    <dimen name="dp_0.5">0.5dp</dimen>  
    <dimen name="dp_1">1dp</dimen>  
    <dimen name="dp_2">2dp</dimen>  
    <dimen name="dp_2.5">2.5dp</dimen>  
    <dimen name="dp_3">3dp</dimen>  
    ...........  
    <dimen name="dp_370">370dp</dimen>  
    <dimen name="dp_402">402dp</dimen>  
    <dimen name="dp_410">410dp</dimen>  
    <dimen name="dp_422">422dp</dimen>  
    <dimen name="dp_472">472dp</dimen>  
    <dimen name="dp_500">500dp</dimen>  
    <dimen name="dp_600">600dp</dimen>  
    <dimen name="dp_640">640dp</dimen>  
  
    <dimen name="sp_6">6sp</dimen>  
    <dimen name="sp_7">7sp</dimen>  
    <dimen name="sp_8">8sp</dimen>  
    <dimen name="sp_9">9sp</dimen>  
    <dimen name="sp_10">10sp</dimen>  
    <dimen name="sp_11">11sp</dimen>  
    ......  
    <dimen name="sp_19">19sp</dimen>  
    <dimen name="sp_20">20sp</dimen>  
    <dimen name="sp_21">21sp</dimen>  
    <dimen name="sp_22">22sp</dimen>  
    <dimen name="sp_24">24sp</dimen>  
    <dimen name="sp_28">28sp</dimen>  
    <dimen name="sp_38">38sp</dimen>  
    <dimen name="sp_40">40sp</dimen>  
    <dimen name="sp_41">41sp</dimen>  
    <dimen name="sp_48">48sp</dimen>  
</resources>  
```

4、双击执行screenMatchDP.bat

## 使用PX进行屏幕适配

1、安装插件

Settings->Plugins->Browse repositories->ScreenMatch

2、将screenMatchPX.bat和screenMatchPX.jar放入app\src\main中

[项目地址](https://github.com/mengzhinan/PhoneScreenMatch)

基准分辨率为1280×720

默认的适配屏幕为

"320,480;480,800;480,854;540,888;600,1024;720,1184;720,1196;720,1280;768,1024;768,1280;800,1280;1080,1812;1080,1920;1440,2560;";  

增加适配分辨率

```bash
java -jar xxx.jar 基准width 基准height 待适配w,待适配h；待适配w,待适配h；待适配w,待适配h；  
```

更改基准分辨率

```bash
java -jar xxx.jar 基准width 基准height  
```

其他写法

```bash
java -jar xxx.jar 待适配w,待适配h；待适配w,待适配h；待适配w,待适配h； 
```

运行bat文件，values目录下自动生成所有分辨率的dimens.xml文件，用法：任何控件宽度设置为@dimens_x/x720，必定占据所有宽度

若设备的屏幕像素参数为 1283.45 x 724.89，那就取 1283x724，不要四舍五入。

3、双击执行screenMatchPX.bat



**Markdown**
*Awesome*