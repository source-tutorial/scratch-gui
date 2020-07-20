# Scratch-gui二次开发手册
### 前提
有React基础知识，已经搭建并成功运行scratch-gui，这里不普及如何安装，以免影响干货的质量；

## 正题

### 如何调整主题皮肤
#### HSLA知识点普及
> HSLA 颜色值得到以下浏览器的支持：IE9+、Firefox 3+、Chrome、Safari 以及 Opera 10+。

> HSLA 颜色值是 HSL 颜色值的扩展，带有一个 alpha 通道 - 它规定了对象的不透明度。

> H：Hue(色调)。0(或360)表示红色，120表示绿色，240表示蓝色，也可取其他数值来指定颜色。取值为：0 - 360

> S：Saturation(饱和度)。取值为：0.0% - 100.0%

> L：Lightness(亮度)。取值为：0.0% - 100.0%

> A：Alpha透明度。取值0~1之间。

#### 如何调整色调
色调配置colors.css文件中，按照注释尝试调整看看

**路径为**：{你源码clone的根目录}/scratch-gui/src/css/colors.css

```
/* 主色、次色、三级色*/
$ui-primary: hsla(35, 100%, 95%, 1); /* #E5F0FF */
$ui-secondary: hsla(0, 100%, 100%, 1); /* #E9F1FC */
$ui-tertiary: hsla(0, 100%, 100%, 1); /* #D9E3F2 */

$ui-modal-overlay: hsla(240, 100%, 65%, 0.9); /* 90% transparent version of motion-primary */

/* 白色系定义 */
$ui-white: hsla(0, 100%, 100%, 1); /* #FFFFFF */
$ui-white-dim: hsla(0, 100%, 100%, 0.75); /* 25% transparent version of ui-white */
$ui-white-transparent: hsla(0, 100%, 100%, 0.25); /* 25% transparent version of ui-white */
$ui-transparent: hsla(0, 100%, 100%, 0); /* 25% transparent version of ui-white */

$ui-black-transparent: hsla(35, 100%, 30%, 0.15); /* 15% transparent version of black */

/* 文本字体颜色定义 */
$text-primary: hsla(225, 15%, 40%, 1); /* #575E75 */
$text-primary-transparent: hsla(225, 15%, 40%, 0.75);

/*菜单栏颜色，按钮颜色*/
$motion-primary: hsla(35, 100%, 50%, 1); /* #4C97FF */
$motion-tertiary: hsla(35, 100%, 50%, 1); /* #3373CC */
$motion-transparent: hsla(35, 100%, 65%, 0.35); /* 35% transparent version of motion-primary */
$motion-light-transparent: hsla(35, 100%, 65%, 0.15); /* 15% transparent version of motion-primary */

/* 红色系定义 */
$red-primary: hsla(20, 100%, 55%, 1); /* #FF661A */
$red-tertiary: hsla(20, 100%, 45%, 1); /* #E64D00 */

/* 声音色系 */
$sound-primary: hsla(300, 53%, 60%, 1); /* #CF63CF */
$sound-tertiary: hsla(300, 48%, 50%, 1); /* #BD42BD */

/*  播放控制按钮色系 */
$control-primary: hsla(230, 100%, 55%, 1); /* #FFAB19 */

/* 分享按钮以及开发中功能提示的背景色 */
$data-primary: hsla(38, 100%, 55%, 1); /* #FF8C1A */

/* 改变按钮的背景色系 */
$pen-primary: hsla(30, 100%, 40%, 1); /* #0FBD8C */
$pen-transparent: hsla(35, 105%, 40%, 0.25); /* #0FBD8C */

/* 错误提示的色系 */
$error-primary: hsla(30, 100%, 55%, 1); /* #FF8C1A */
$error-light: hsla(30, 100%, 70%, 1); /* #FFB366 */
$error-transparent: hsla(30, 100%, 55%, 0.25); /* #FF8C1A */

/* 悬浮按钮的色系 */
$extensions-primary: hsla(35, 95%, 45%, 1); /* #0FBD8C */
$extensions-tertiary: hsla(35, 95%, 40%, 1); /* #0B8E69 */
$extensions-transparent: hsla(35, 95%, 50%, 0.35); /* 35% transparent version of extensions-primary */
$extensions-light: hsla(35, 57%, 85%, 1); /* opaque version of extensions-transparent, on white bg */

/* 拖动高亮色 */
$drop-highlight: hsla(35, 100%, 77%, 1); /* lighter than motion-primary */

```
##### 调整后的样子

![image](https://github.com/source-tutorial/scratch-gui/blob/dev/images/colors.png)
