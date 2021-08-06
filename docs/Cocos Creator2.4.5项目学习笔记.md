---
title: Cocos2d-x - 项目学习笔记
description: Cocos Creator 2.4.5 | typescript
---



### 一、sync-demo

先从loading UI看起

主要查看teacherPanel和studentPanel

ConstValue中的变量需要注意：IS_EDITIONS在调试时设置为false，发布时设置为true

CoursewareKey是标识自己项目的唯一码，在网上随机生成24位

* 发布时，需要单独选择发布的是教师版还是学生版
* IS_TEACHER在发布时需要根据教师版还是学生版本进行修改
* 观察者模式在哪个文件里来着？好像是xxxxManager吧？



### 二、jar-cocos

frameControl是js文件中的，可以直接通过window.frameControl调用

IS_DIRTIONS需要修改？

疑问：

* `showUI`和`showUI_Simplify`的区别？
*  隐藏`LoadingUI`和删除`LoadingUI`的区别？为啥要把删除改为隐藏？



### 三、RaceGame

* 代码命名规范的重要性
* 尽量通过creater拖拽的方式对节点进行复制，少用cc.find()或node.findChildByName()方式，因为层级关系容易改变，节点名称也容易改变。



### 四、视频学习

* 音频的加载模式必须是dom audio，有的用户设备太老了无法兼容

* `===`判断**值**和**类型**相不相同

* ( ) => {}

* `any`和`unknown`
  * any可以是任意类型，一般适用于：1. 读取未知类型的输入 2. 任意值转换为字符串
  * unknown比any有一些限制，在使用前必须对其进行类型限定
  
* `@param`

  是jsdoc之类的文档语法，可以自动生成相应文档。在一些工具中也会自动提示。它一般用于函数声明注释。

  示例: 注释变量名 、 变量类型 和 变量说明

  ```js
  /**
   * @param {string} somebody Somebody's name.
   */
  function sayHello(somebody) {
      alert('Hello ' + somebody);
  }
  ```

* `JSON.parse`将从服务器获取的数据转换为JS对象
  
* 随机字符串的生成
  
  * 长度24位
  
* `eval(string)`会将string当作JS代码执行。

* 存取器的用法`get `、`set`

* `public static readonly Test='Test'`，其中的`readonly`表示在 **属性检查器** 中只读

* `function.bnid(xxx)`就是让function中的`this`都绑定为`xxx`

* `declare`是用于定义`js`中传入数据的类型



### 五、《一刻千金》项目

* `parseInt`字符串转`number`
* `isNaN(x)`检测x是否非数字值
* `JSON.parse`将json串赋值给对象
* `courseware_content`中是需要用到的游戏数据
* `slice(start,end)`将字符串/数组中`start`到`end`到部分截取出来
* 对象中的扩展运算符`(...)`用于取出参数对象中的所有可遍历属性，拷贝到当前对象之中
  * 如：`let lists=[...items]`		// 妙啊，就相当于循环便遍历数组，将`items`数组赋值给`lists`数组
* `()=>{callback&&callback();}`      // 如果callback存在，就执行callback()
* `A.trigger(B)`触发`A`的事件`B`



### 六、《哪边多哪边少》项目

#### GameData

* 【1】是否开启星级评判、是否朗读题干、是否允许重玩
* 【2】课件等级，is_active

#### GameDataCtrl 同步的全量数据:

* 【1】当前关卡编号、操作编号、操作对象编号、操作对象位置
* 【2】关卡编号、关卡数据、游戏是否结束

#### TemplateGonfig 通用配置表：

* 【1】UI枚举类型；
* 【2】可被打断的音效；
* 【3】各种音效、动态加载资源spine、各种反馈时间、总关卡数、可重玩次数

#### GameTitle

* 【1】是否自动播放、是否有背景、音效名[ ]、文本

#### SyncManager

* sendSyncData( )、sendSyncClick( )等是自己发送消息给所有成员（包括自己）
* syncCallBack( ) 是接受消息的机制，既可以接收到其他人发来的消息，也可以接受到自己的消息



#### 其他

* 删除资源`cc.assetManager.releaseAsset(asset);`
* 加载网络纹理：`cc.assetManager.loadRemote(path, cc.Texture2D, function());`
* `export` 语句用于从模块中导出实时绑定的函数、对象或原始值，以便其他程序可以通过 [`import`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/import) 语句使用它们
* listenerManager 是观察者模式的实现
* function.appley(o)  将function中用到的this改为对象o



#### 疑问

- [x] 为什么要监听其他用户发来的点击事件？他们点击不点击和我有什么关系呢？

  因为游戏同一时刻只能一个学生操作，这个学生的操作要同步到其他所有人的屏幕上

- [x] 只给给类的参数赋值，和使用单例赋值的区别是什么？

  比如【GameDataCtrl.playIndex=1 】和 【GameData.getInstance().passdata.has_star=0】

  答案：如果一个方法和他所在类的实例对象无关，那么它就应该是静态的，否则就应该是非静态。因此像工具类，一般都是静态的。

- [ ] 游戏开始上报的这些数据需要改吗？是固定的吗？

  ​		GameMsg.get_is_sync();

  ​        GameMsg.get_role();

  ​        GameMsg.getInstance().gameStart();



### 	七、海量老师项目

1. 将可以被拖拽的所有节点放在一个空节点下，然后将拖拽脚本放在外部空节点上，这样可以单独拖拽其所有子节点。

   当要获取拖拽对象时，在脚本下用一个回调函数传入参数e，可获得当前节点e.target。

   若要实现拖拽后归位的效果：就在回调函数中设置其父节点`node.parent = node.initParent`和当前位置`node.position = cc.v3(node.initPos.x,node.initPos.y)`，以防拖拽后复位。

2. 在creater设置回调函数的方法是：`@property({ type: [cc.Component.EventHandler] })`

   触发事件的方法是：`event.emit(data);`

   详见 `DragGroupSync.ts`

