## 目标
在空旷环境下实现点到点的路径规划
### 约束
+ **最小拐弯半径** 由于车辆物理结构的设计，必然对应着一个最小拐弯半径，如果路径的拐弯半径小于车辆的最小拐弯半径，则车辆无法实现此路径
+ **姿态** 车辆存在起点和终点，两个点都有对应的车的姿态，要使车辆满足姿态的要求，则路径的起始位置必须与车辆的起始角度相切，
路径的终点位置必须与车辆的终止角度相切
+ **其余物理因素不考虑** 为了不打击积极性，目前尽量不考虑太多细节
### 已知
+ **A*算法**  最简单的A*算法，在二维空间中可以找到一条最短路径，但是不光滑（由折线组成）
+ **混合A*算法** 高维度的A*算法，算是在简单的A*算法上面的推广，可以实现较光滑（连续）的路径
+ **多项式生成** 在一定的约束下，可以解除最小的jrek
+ **样条函数** 给定几个点，可以生成一条经过几个点的连续的曲线
### 分析
>  + A*算法生成的曲线不能用，放弃
> + 混合A*算法生成的曲线计算量比较大，但是能比较好的满足姿态要求
>  + 多项式生成，不是很熟悉，但是能满足最小化jrek的要求
> +  样条函数，相对的jrek也是比较小，计算量小，在短距离下不一定能够满足**约束**<br>
**由于目标空旷，约束相对较小，最终采用样条函数+混合A*算法的解决方案**
### 最终方案（配图看）
1. 起点和终点连线（两点间距离大于2R）
2. 将起点姿态通过圆圈（倒车或者开车）矫正到与连线平行
3. 同理将终点也矫正到与连线平行（转换到平行是为了确保样条函数能够符合约束）
4. 将矫正后的点投影到连线上，如果两点距离大于2R，则用样条函数将矫正后的位置拟合;
否则则只矫正起点，剩下的路径直接用混合A*算法
5. 如果起点和终点距离小于等于2R，直接使用混合A*算法<br/>
![示意图](https://github.com/LittleDang/fireFly-test/blob/master/myPathPlanner/myDrawPath2.png "示意图")
![所有路径](https://github.com/LittleDang/fireFly-test/blob/master/myPathPlanner/myDrawPath3.png "所有路径")
![最佳路径](https://github.com/LittleDang/fireFly-test/blob/master/myPathPlanner/myDrawPath.png "最佳路径")
### 调试工具
> ros+rviz</br>
![可视化](https://github.com/LittleDang/fireFly-test/blob/master/myPathPlanner/path.png "可视化")
### 结果
> 短距离</br>

![短距离](https://github.com/LittleDang/fireFly-test/blob/master/myPathPlanner/%E6%83%85%E6%B3%813.png "短距离")

> 中距离</br>

![中距离](https://github.com/LittleDang/fireFly-test/blob/master/myPathPlanner/%E6%83%85%E6%B3%812.png "中距离")

> 长距离</br>

![长距离](https://github.com/LittleDang/fireFly-test/blob/master/myPathPlanner/%E6%83%85%E6%B3%811.png "长距离")
