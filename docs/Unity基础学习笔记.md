---
title: Unity3D - 基础手册
description: Unity 2020.3.14f1c1 | c#
---



### 快捷键

* 找到物体位置：选中物体，鼠标移到场景窗口，按F键
* 缩放：按住 `Alt（/Option)`，鼠标右键拖拽
* 旋转：按住 `Alt（/Option)`，鼠标左键拖拽
* Y轴归位：按下`shift`+点击方向轴中间的小方块
* 克隆物体：
  	1. `Ctrl(/command) + D` 
   	2. 右键物体 -> Duplicate
  
  

### 摄像机

* 摄像机到我当前观察的位置：选中摄像机，`Game Object` -> `Align With View`



### 光照

* 自动生成光照：`window` -> `Rendering` -> `Lighting` -> `Scene` -> `Generate Lighting`  ，生成后，游戏运行时自动加载这份光照数据
* 光照大小：`window` -> `Rendering` -> `Lighting` -> `Environment` -> `Intensity Multiple` 调小
* 天空盒：`window` -> `Rendering` -> `Lighting` -> `Environment` -> `Skybox Matrrial`
  * 一般着色器可以选择`Skybox/6 Sided`，六张图分别表示上下前后左右，大小一般为`2048*2048`或 ` 4096*4096`
  * 自制方法：
    1. 准备6张图片
    2. 将6张图片的`Wrap Mode`全部设置为`Clamp`，使图片无缝拼贴
    3. 新建`material`，并将`shader` 改为`Skybox/6 Sided`
    4. 将图片拽入对应位置（可将材质的属性面板右上角先锁定，再切换路径找图片）
    5. 光照设置中设置天空盒



### 编辑器

* Cube和Cube顶点对齐：`Shift+V` -> 选中一个顶点，拖拽到另一个要对其的位置上，会自动吸附对齐

* Plane和Cube顶点对齐：选中`Plane` -> `Scene`左上角切换为`Shaded`或者`Shaded WireFrame `显示出的每一个格子顶点都是可以对齐的（另外补充，`Wireframe` 表示网格显示）

* 多个物体旋转：
  1. 选中多个物体，左上方切换为 `Pivot` 表示多个物体按照各自中心旋转
  2. 选中多个物体，左上方切换为 `Center` 表示多个物体按照统一轴旋转



### 运动

* 设置鼠标拖拽时的单位步长：`Edit` -> `Grid and Snap` -> `Move`

* LookAt朝向物体的用法：

  ```c#
  // this.gameObject 朝向 obj
  Vector3 pos = obj.transform.position;
  pos.y = 0;	// 应当朝向obj在地面上的投影
  this.transform.LookAt(pos);     // this的z轴指向目标在地面的投影
  ```

* 自转与公转：

    * 自转

      ```c#
      float angle = 30f * Time.deltaTime;
      transform.Rotate(0,angle,0);
      ```

    * 公转

      ```c#
      transform.parent.Rotate(0, -angle, 0);	// 公转就是让一个空节点做父节点自转，则子物体都会跟着转
      ```

    

### 遮挡

* `Occludee Static`：透明物体不能遮挡，以及小物件，都不可能阻挡其他的东西，应标记为`Occludees`，但不遮挡。这意味着它们将被视为能被其他物体遮挡，但不会被视为作为遮挡物自身，这将有助于减少计算量。



### 建模

* 模型的正面最好是与z轴的朝向一致，便于LookAt()等方法的使用。若不一致，可以通过建立父节点的方式，再调整该模型的Rotation。

* material：

    * Albedo 反照率，可设置贴图
    * Metallic 金属性，值越大越像金属
    * Smoothness 平滑度
    * Normal Map 法线贴图，用于表现物体凹凸不平的材质

  

### 动画

#### 1. Animation

* 创建Animation：创建动画 -> 属性面板`Inspector`字样处调至`Debug`模式 -> 勾选`Legacy`标签 -> 调回`normal`模式 -> 挂载动画文件到物体上

* 制作移动动画：
    * 打开编辑窗口：`Window` -> `Animation` ->` Animation`
    * 选中要添加动画的物体，选中 `Add Property` -> `Transform` -> `Position`
    * 可使用滚轮缩放，按住滚轮可移动
    * 点击红色按钮，进入录制模式，然后进行编辑，编辑结束后取消录制模式
    
* Animation编辑窗口可以批量操作：多选、拖拽等进行多个移动/缩放

* Animation编辑窗口，左下角选中`Curves`进入曲线模式
    * 按`F`显示曲线全貌
    * 滚轮缩放视图；`Ctrl` + 滚轮 ，仅缩放横轴；`shift` + 滚轮，仅缩放纵轴
    * 按住鼠标滚轮，可平移视图
    * 在线上双击，即可得到关键帧，右键`delete`即可删除关键帧
    * 将曲线改为直线的方法（即改为匀速运动）：右键关键帧 -> `Both Tangents` -> `Linear` (其中，Free是可编辑曲线，`Linear`是直线，`Contstant`是恒定值)
    
* 动画事件：
    * 在动画进行到某个关键帧时，执行脚本中的方法：在红色时间轴下方的灰色区域右键 -> `add Animation Event` -> 在右边属性栏中指定方法（方法中可传入的参数类型为：`int`、`float`、`string`、`GameObject`）
    
* 脚本中控制API播放动画：添加Animation组件 -> 调整动画数量，取消自动播放 -> 将Animator拖拽进属性 -> 修改脚本

    ```c#
    Animation anim = GetComponent<Animation>();
    anim.Play("...");
    anim.Stop();
    ```



#### 2. 动画状态机

* 创建状态：右键创建`Animation Controller` -> 双击打开 -> 在`controller`面板内右键创建状态

* 设置默认状态：选中状态 -> 右键 -> `Set as Layer Default State `

* 创建过渡：选中状态 -> 右键 -> `Make Transition` -> 选中另一个状态

* 绑定动作：

  * 添加一个动画
  * 选中某个状态，将动画拖拽到它的`Motion`参数中
  * 选中物体，编辑动画，类型不能为`Legacy`

* 添加状态参数：左侧`Parameters`中创建参数 -> 选中过渡线 -> 在右侧`Inspector`中取消勾选`Has Exit Time`（该参数用于设置自动退出的时机） -> 在`Conditions`属性中添加条件

* `Has Exit Time`:

  * `Exit Time`：多少轮/秒时退出
  * `Fixed Duration`：勾选——`秒`；不勾选——`轮`

* 状态机API：

  ```c#
  Animation animator = GetComponent<Animator>()
  animator.SetBool("dancing", Input.GetKey(KeyCode.V));	// 按V键dancing
  animator.SetBool("jump", Input.GetKey(KeyCode.Space));	// 按空格jump
  animator.SetIntger(...);
  animator.setFloat(...);
  ```

  