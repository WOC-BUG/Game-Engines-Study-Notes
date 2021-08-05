# Unity笔记

快捷键

* 找到物体位置：选中物体，鼠标移到场景窗口，按F键
* Y轴归位：按下`shift`+点击方向轴中间的小方块
* 克隆物体：
  	1. `Ctrl(/command) + D` 
   	2. 右键物体 -> Duplicate
* 



摄像机

* 摄像机到我当前观察的位置：选中摄像机，`Game Object` -> `Align With View`



光照

* 自动生成光照：`window` -> `Rendering` -> `Lighting` -> `Scene` -> `Generate Lighting`  ，生成后，游戏运行时自动加载这份光照数据
* 光照大小：`window` -> `Rendering` -> `Lighting` -> `Environment` -> `Intensity Multiple` 调小



**对齐**：

* Cube和Cube顶点对齐：`Shift+V` -> 选中一个顶点，拖拽到另一个要对其的位置上，会自动吸附对齐

* Plane和Cube顶点对齐：选中`Plane` -> `Scene`左上角切换为`Shaded`或者`Shaded WireFrame `显示出的每一个格子顶点都是可以对齐的（另外补充，`Wireframe` 表示网格显示）

* 多个物体旋转：
  1. 选中多个物体，左上方切换为 `Pivot` 表示多个物体按照各自中心旋转
  2. 选中多个物体，左上方切换为 `Center` 表示多个物体按照统一轴旋转



移动步长

* 设置拖拽时的单位步长：`Edit` -> `Grid and Snap` -> `Move`