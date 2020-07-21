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

![改造后的界面](https://github.com/source-tutorial/scratch-gui/blob/dev/images/colors.png)

### 理解状态流程图
> 理解官方给出的Project状态图尤为重要，只有理解了这张图，你才能游刃有余的完成二次开发

![状态流转图](https://github.com/LLK/scratch-gui/raw/develop/docs/project_state_diagram.svg)

#### 例子
![状态修改例子](https://github.com/LLK/scratch-gui/raw/develop/docs/project_state_example.png)

> 1. 当应用程序第一次挂载时，项目状态为NOT_LOADED。
> 2. SET_PROJECT_ID redux操作被分派(从src/lib/project-fetche -hoc.jsx)， projectId设置为123456。这将状态从NOT_LOADED转换到FETCHING_WITH_ID。
> 3. FETCHING_WITH_ID状态。在src / lib / project-fetcher-hoc。jsx, projectId值123456用于从服务器请求该项目的数据。
> 4. 当服务器响应数据时，src/lib/project-fetcher-hoc。jsx通过projectData集分派DONE_FETCHING_WITH_ID动作。这个动作将状态从FETCHING_WITH_ID转换到LOADING_VM_WITH_ID。
> 5. LOADING_VM_WITH_ID状态。在src / lib / vm-manager-hoc。在jsx中，我们将projectData加载到Scratch的虚拟机(“vm”)中。
> 6. 加载完成后，使用src/lib/vm-manager-hoc。jsx分派DONE_LOADING_VM_WITH_ID操作。这将状态从LOADING_VM_WITH_ID转换为SHOWING_WITH_ID
> 7. SHOWING_WITH_ID状态。现在项目出现正常，是可玩和编辑。

### 通过传入参数projectId来加载远程project
#### 如何传入动态projectId参数
Scratch-gui实现了一个高阶组件，什么是高阶组件，请自行搜索补课噢。
这个高阶组件叫HashParserHOC，在src/lib/hash-parser-hoc.jsx下，HashParserHOC组件的作用是监听hashchange事件

```
        componentDidMount () {
            window.addEventListener('hashchange', this.handleHashChange);
        }
```
通过修改访问地址，携带hash参数即可实现动态传入projectId的功能，例如：http://domain/#projectId=0F381E0267E5D2309F10E8517BF5E50F。我修改了scratch-gui projectId，感觉这里ProjectId用Md5值更合适，不过源码本身也是支持字符串的，projectId的类型定义是这样的

```
reduxProjectId: PropTypes.oneOfType([PropTypes.string, PropTypes.number]),
```
再来看看handleHashChange如何处理动态projectId参数，采用正则匹配projectId，未匹配到则取默认‘0’。
```
        handleHashChange () {
            const hashMatch = window.location.hash.match(/[#]projectId=([^#]\S+)/);
            const hashProjectId = hashMatch === null ? defaultProjectId : hashMatch[1];
            this.props.setProjectId(hashProjectId.toString());
        }
```
通过this.props.setProjectId方法，通过redux进行状态更改，这里科普一下什么是redux。

>Redux基础知识  
1. 三个基本原则  
- 整个应用只有唯一一个可信数据源，也就是只有一个 Store  
- State 只能通过触发 Action 来更改  
- State 的更改必须写成纯函数，也就是每次更改总是返回一个新的 State，在 Redux 里这种函数称为 Reducer

2. Actions  

Action 很简单，就是一个单纯的包含 { type, payload } 的对象，type 是一个常量用来标示动作类型，payload 是这个动作携带的数据。Action 需要通过 store.dispatch() 方法来发送。

    比如一个最简单的 action：
    
```
{
  type: 'ADD_TODO',
  text: 'Build my first Redux app'
}
```

一般来说，会使用函数（Action Creators）来生成 action，这样会有更大的灵活性，Action Creators 是一个 pure function，它最后会返回一个 action 对象：
    
```
function addTodo(text) {
  return {
    type: 'ADD_TODO',
    text
  }
}
```
所以现在要触发一个动作只要调用 dispatch: dispatch(addTodo(text))

3. Reducers

Reducer 用来处理 Action 触发的对状态树的更改。

所以一个 reducer 函数会接受 oldState 和 action 两个参数，返回一个新的 state：(oldState, action) => newState。一个简单的 reducer 可能类似这样：

```
const initialState = {
  a: 'a',
  b: 'b'
};

function someApp(state = initialState, action) {
  switch (action.type) {
    case 'CHANGE_A':
      return { ...state, a: 'Modified a' };
    case 'CHANGE_B':
      return { ...state, b: action.payload };
    default:
      return state
  }
}
```
4. Store  

现在有了 Action 和 Reducer，Store 的作用就是连接这两者，Store 的作用有这么几个：  
- Hold 住整个应用的 State 状态树  
- 提供一个 getState() 方法获取 State  
- 提供一个 dispatch() 方法发送 action 更改 State  
- 提供一个 subscribe() 方法注册回调函数监听 State 的更改  
- 创建一个 Store 很容易，将 root reducer 函数传递给 createStore 方法即可：

```
import { createStore } from 'redux';
import someApp from './reducers';
let store = createStore(someApp);
```
现在我们就拿到了 store.dispatch，可以用来分发 action 了：

```
let unsubscribe = store.subscribe(() => console.log(store.getState()));

// Dispatch
store.dispatch({ type: 'CHANGE_A' });
store.dispatch({ type: 'CHANGE_B', payload: 'Modified b' });

// Stop listening to state updates
unsubscribe();
```





====作者努力更新中==,请Star关注动态哦==
