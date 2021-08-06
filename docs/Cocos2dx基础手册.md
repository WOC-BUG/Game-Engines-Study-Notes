---
title: Cocos2d-x - 基础手册
description: Cocos Creator 2.4.5 | typescript ｜ 王浩宇老师整理
---



## 摄像机

客户端看到的画面，类似viewport,，摄像机是cocos creator里的一个组件，每个场景中至少需要一个摄像机，也可以创建多个,创建场景时会自动生成一个摄像机，作为主摄。



## 场景

场景代表的当前的游戏世界，引擎一次性只会加载一个场景，如果需要切换场景，默认会将场景中所有的节点和其他实例销毁

* 场景的加载和切换使用 cc.director.loadScene(`newScene`)
* 如果需要换场，让场景切换时不被自动销毁，常驻内存，需要一个组件控制所有场景的加载并把它标记为常驻节点，使用cc.game.addPersistRootNode(myNode)
* 取消节点的常驻属性 cc.game.removePersistRootNode(myNode)
* 场景的预加载　cc.director.preloadScene



## 预制体

预先配置好的游戏对象，常见为动态加入，多次调用对象，生成方法，在场景中编辑好节点，从层级选择器拖到资源选择器自动生成预制体，在属性检查器中可以选择自动同步修改，双击资源选择器的预制体可以对节点进行进一步修改



## node

* Cocos create以组件式开发为核心，节点是承载组件的实体，节点提供了位置、旋转、缩放、尺寸、颜色等信息，复杂的组件有复杂的节点树，拖动父级节点的位置可以将子节点也同时移动，一个节点上可以添加多个组件，形成节点树，但是一个节点只能添加一个渲染组件

* 脚本中动态创建节点，new cc.Node()
* 克隆节点 cc.instantiate
* 创建预制节点（和克隆节点类似），设置预制体，在properties中声明，type设置为cc.Prefab，使用cc.instantiate生成
* 销毁节点 node.destroy()，并不会立即销毁，而是当帧逻辑更新结束后，统一执行，可以用cc.isValid来判断



## 事件机制

事件处理是在cc.Node中完成，对于组件可以通过this.node用来注册和监听事件

* 监听事件通过this.node.on()函数来注册（第三个参数绑定调用者）

* off()关闭监听（参数和on一样）

* 发射事件 emit(第二个参数传递事件参数）

* 派送事件，dispatcheEvent(可以做事件传递)，采用冒泡派送，使用event.stopPropagation()来中断事件传递

* 系统事件

> 鼠标（节点系统事件）
>> cc.Node.EventType.MOUSE_DOWN
>>
>> cc.Node.EventType.MOUSE_ENTER
>
>触摸（节点系统事件）
>
>> 支持节点树的事件冒泡
>
>键盘（全局系统事件）
>
>> 通过cc.systemEvent.on(type,callback,target)注册
>>
>> type类型
>> >
>> >cc.SystemEventType.KEY_DOWN
>> >
>> >cc.SystemEventType.KEY_UP
>> >
>> >cc.SystemEventType.DEVICEMOTION
>
>重力（全局系统事件）



## 生命周期

> onLoad(节点首次激活时触发)
>
> start(初始化一些中间状态的数据，这些数据可能会在update时改变)
>
> update(每一帧渲染前更新物体的行为，状态和方位，会在动画更新前执行)
>
> lateUpdate(在动效更新之后进行一些额外操作，希望所有组件的update都执行完后操作)
>
> 当组件的enable属性从false变为true，或所在节点的active属性从false变为true,会激活onEnable回调，若节点第一次被创建且enabled为true,会在onLoad后，start之前被调用
>
> 当组件的enabled属性从true变为false时，或者所在节点的active属性从true变为false,会激活onDisable回调
>
> 当组件或者所在节点调用了destroy()，则会调用onDestroy回调，并在当帧结束时统一回收组件

### 一个完整的生命周期

> onLoad
>
> onEnable
>
> start
>
> update
>
> lateUpdate
>
> onDisable
>
> onDestroy



## 加载机制

* 脚本的加载机制

>COCOS2D引擎
>
>插件脚本（有多个按项目中路径字母顺序依次加载）
>
>普通脚本（打包后只有一个文件，内部按require的依赖顺序依次初始化）

## 渲染机制

根据节点的node._renderFlag状态进入到 transform=>render分支，而不需要再进行多余的状态判断

> Node
>
> Transfrom
>
> Render
>
> Render&transfrom
>
> Render&transfrom&children



## 图集

多张图片合成一个集合，合成图集会减少游戏包体和内存占用，如果多个精灵节点使用同一张图集，会大大减少cpu的运算时间，提高运行效率



## drawcall

 cpu调用图形接口，命令gpu进行图形渲染，为了优化需要减少drawcall的次数，如合批（批处理）方法，合并碎图成图集，同一图集按照顺序摆放节点，中间不能插入其他图集的节点

* 优化注意

  默认字体的每个label都会产生，需要制作位图字体
* 修改图片默认颜色会增加
* 图片类型为九宫格会增加
* 修改图片默认的透明度会增加
* 默认的纯色图片会增加



## 帧频（fps)

Fps：画面每秒传输帧数，帧数越多，越流畅，cocos中获得fps的几个val方法：cc.director.getAnimationInterval(),cc.director.getSecondsPerFrame()

* 设置刷新帧频 cc.game.setFrameaRate()



## 坐标系

cocos采用笛卡尔坐标系

* 世界坐标系

> 绝对坐标系，表示游戏场景

* 本地坐标系

> 相对坐标系，与节点相关联的坐标系，每个节点都有独立的坐标系，修改节点的位置属性是采用本地坐标系



## 锚点

决定了节点以自身约束框中的哪一个点作为整个节点的位置，由anchorX和anchorY两个值来表示，通过节点尺寸计算锚点位置的乘数因子，范围都是0-1之间，（0，0）时，在左下角

