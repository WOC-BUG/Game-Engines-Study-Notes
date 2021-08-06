---
title: Unity3D - 基础手册
description: Unity 2020.3.14f1c1 | c#
---



### 快捷键

* 找到物体位置：选中物体，鼠标移到场景窗口，按F键
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



### 光照

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

  * 自传

    ```c#
    float angle = 30f * Time.deltaTime;
    transform.Rotate(0,angle,0);
    ```

  * 公转

    ```c#
    transform.parent.Rotate(0, -angle, 0);	// 公转就是让一个空节点做父节点自转，则子物体都会跟着转
    ```

    



### 遮挡

* `Occludee Static`：透明物体不能遮挡，以及小物件，都不可能阻挡其他的东西，应标记为Occludees，但不遮挡。这意味着它们将被视为能被其他物体遮挡，但不会被视为作为遮挡物自身，这将有助于减少计算量。



### 建模

* 模型的正面最好是与z轴的朝向一致，便于LookAt()等方法的使用。若不一致，可以通过建立父节点的方式，再调整该模型的Rotation。

* material：

  * Albedo 反照率，可设置贴图
  * Metallic 金属性，值越大越像金属
  * Smoothness 平滑度
  * Normal Map 法线贴图，用于表现物体凹凸不平的材质

  

### 动画

* 创建Animation：创建动画 -> 属性面板`Inspector`字样处调至`Debug`模式 -> 勾选`Legacy`标签 -> 调回`normal`模式 -> 挂载动画文件到物体上
* 制作移动动画：
  * 打开编辑窗口：`Window` -> `Animation` ->` Animation`
  * 选中要添加动画的物体，选中 `Add Property` -> `Transform` -> `Position`
  * 可使用滚轮缩放，按住滚轮可移动
  * 点击红色按钮，进入录制模式，然后进行编辑，编辑结束后取消录制模式
* Animation编辑窗口可以批量操作：多选、拖拽等进行多个移动/缩放
* Animation编辑窗口，左下角选中`Curves`进入曲线模式