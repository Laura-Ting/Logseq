- Chapter1-2简介
  collapsed:: true
	- SLAM：Simulataneous Localization and Mapping 同时定位与地图构建
	- 传感器在没有先验环境特定信息的环境下，①于运动过程中建立环境的模型（地图，表示不唯一，空间信息也可以），②同时估计自己的运动（他的轨迹）
	- 两类传感器：
	  collapsed:: true
		- 环境之中
		- 携带在机器人身上
		- 建图难度不同：环境之中的比较容易（因为都对环境条件做出了假设），我们此处针对携带在机器人身上的（限制相比于前者小很多）
	- 在slam中传感器分类：
	  collapsed:: true
		- 激光型
		- 相机型
		  collapsed:: true
			- 便宜
			- 轻便
			- 信息丰富
			- 计算量很大
			- 需要一定的工作条件：光线要充足，不能被挡住，环境需要有一定的纹理
	- 相机的种类
	  collapsed:: true
		- 单目Monocular：更难一些，只有一个图像
		  collapsed:: true
			- 没有深度，通过移动相机产生深度
			- 运动起来的时候近处的东西移动的快，远处的东西移动的慢
		- 双目Stereo：除了图像之外还能知道像素点离我们的距离，是个关键信息
		  collapsed:: true
			- 通过视差计算深度
			- 左右眼的微小差异判断远近；近处的变化小，远处的变化大——推算距离，计算量很大
		- 深度RGBD：也是
		  collapsed:: true
			- 通过物理方法计算深度（有点类似于激光）
			- 结构光ToF，主动测量功耗大，测量值较准确，量程小易受干扰，可以回复三维结构
		- 其他：鱼眼，全景，Event Camera
	- 相机的本质
	  collapsed:: true
		- 以二维的方式记录了三维世界的信息
		- 丢掉的一个维度：距离
	- 共同点
	  collapsed:: true
		- 利用图像和场景的几何关系，计算相机运动和场景结构
		- 三维空间的运动和结构
		- 图像来自连续的视频
	- 视觉SLAM框架
	  collapsed:: true
		- 前端VO
		  collapsed:: true
			- 视觉里程计Visual Odometry
			  collapsed:: true
				- 相邻图像估计相机运动
				- 基本原理：通过相邻两张图像计算运动和结构
				- 不可避免的有漂移
				- 方法：特征点法，直接法
		- 后端：Optimization
		  collapsed:: true
			- 从带有噪声的数据中优化轨迹和地图状态估计问题
			- 前期以滤波器（扩展卡尔曼滤波器Extended Kalman Filter，EKF）为主，后期以图优化为主
		- 回环检测：Loop Closing
		  collapsed:: true
			- 检测机器人是否回到早先位置
			- 识别到达过的场景
			- 计算图像间的相似性
			- 词袋模型
		- 建图：Mapping
		  collapsed:: true
			- 用于导航，通讯，规划，可视化，交互
			- 度量地图vs拓普地图
			- 稀疏地图vs稠密地图
		- 示意图
		  collapsed:: true
			- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202401151825669.png)
	- SLAM问题的数学表述
		- 运动（位置）
		  collapsed:: true
			- 离散时间t=1,2,3,...k
			- 所在位置x1,x2,x3...,xk
			- 他们都要看成随机变量，服从概率分布
			- 从上一时刻运动到下一时刻的运动方程：xk=f(xk-1, uk, wk) uk是输入，wk是噪声
		- 观测（位置+路标）
		  collapsed:: true
			- 路标（三维空间点）：y1, y2, ... yn
			- 传感器在xk出探测到了yj
			- 观测方程：zk,j = h(xk, yj, vkj)，vkj是一个噪声项
		- 上述两个基本方程：运动方程和观测方程
			- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202401151847681.png)
			- 问题
				- xk代表相机位姿，在三维中是如何取描述的？向量or矩阵？
				- 如果观测是像素点，应该如何表述
				- 如果我知道输入u和观测z，希望推断自己的位置x和目标点所在位置y
- Chapter3三维空间刚体运动
	- 刚体运动的描述方式
	  collapsed:: true
		- 旋转矩阵
		- 变换矩阵
		- 四元数
		- 欧拉角
	- 点，向量，坐标系，旋转矩阵
	  collapsed:: true
		- 点
		  collapsed:: true
			- 点存在于三维空间之中
			- 点和点可以组成向量
			- 点本身由原点指向他的向量所描述
		- 向量
		  collapsed:: true
			- 带指向性的箭头
			- 可以进行加法，减法等运算
			- 向量可以由R3坐标表示
			  collapsed:: true
				- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202401181205582.png)
			- 坐标系是由三个正交的轴组成
			  collapsed:: true
				- 构成线性空间的一组基
				- 左手系和右手系
			- 向量的运算
			  collapsed:: true
				- 加减法，缩放
				- 内积
				  collapsed:: true
					- 模长乘以夹角的余弦值
					  collapsed:: true
						- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202401181209725.png)
				- 外积
				  collapsed:: true
					- 相当于模长相乘后乘上sin，即这两个向量组成的平行四边形的有效面积
					- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202401181229695.png)
					- 我们可以发现前面乘的矩阵是一个反对称矩阵，aT=-a
		- 坐标系
		  collapsed:: true
			- 坐标系之间如何变换，如何计算同一个向量在不同坐标系中的坐标
			  collapsed:: true
				- SLAM中会有一个固定世界坐标系，还会有一个移动机器人坐标系
				- 旋转+平移：a‘ = Ra + t
					- 平移使用向量
					- 旋转使用旋转矩阵
					  collapsed:: true
						- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202401181235505.png)
						- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202401181237413.png)
						- 其中R成为旋转矩阵，旋转矩阵满足如下性质
						  collapsed:: true
							- R是一个正交矩阵
							- R的行列式为+1
						- 满足上述性质的也可以成为旋转矩阵
						  collapsed:: true
							- 特殊正交群
							- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202401181242533.png)
						- 变换
						  collapsed:: true
							- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202401181242207.png)
				- 齐次坐标和变换矩阵
				  collapsed:: true
					- 之前使用两种运动分开叠加有一点不便，叠加起来过于复杂
					- 使用矩阵进行统一
					  collapsed:: true
						- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202401181249387.png)
					- 齐次坐标：
					  collapsed:: true
						- 用四个数表达三维向量
						- 也就是下面多加一行1，如果下面没有多加，那么就叫做非齐次坐标
					- 变换矩阵：
						- 平移和旋转放入同一个矩阵
						- 这种矩阵也叫特殊欧式群
						  collapsed:: true
							- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202401181252040.png)
						- 求逆也比较简便
						  collapsed:: true
							- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202401181253846.png)
			-
	- 三维旋转：
		- 方向为旋转轴，长度为转过的角度
		- 称为角轴or旋转向量 w=θn
		- 与旋转矩阵不同的是
		  collapsed:: true
			- 旋转矩阵：9个量，有正交约束和行列式约束
			- 角轴：3个量，没有约束，也就是李代数
			- 转换
			  collapsed:: true
				- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202401181700874.png){:height 254, :width 283}
		- 欧拉角
		  collapsed:: true
			- 将旋转分解为三次不同轴上的转动，以便理解
			- 例如按照Z-Y-X，偏航角yaw-俯仰角pitch-滚转角roll
			- 轴可以是定轴或者动轴，顺序不一样也存在着很多欧拉角
			- 万向锁
			  collapsed:: true
				- 欧拉角中的一个问题，ZYX顺序中，若Pitch为正负90度，则第三次旋转和第一次绕同一个轴，使得系统丢失了一个自由度――存在奇异性问题
				  collapsed:: true
					- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202401181708068.png){:height 301, :width 543}
			- SLAM中很少使用欧拉角表示姿态
		- 四元数
			- 一种扩展的复数（单位圆上的复数可以表达二维平面的旋转）
			- 由三个虚部，可以表达三维空间中的旋转：q=q0+q1i+q2j+q3k，也可以写成向量形式
			  collapsed:: true
				- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202401191127802.png)
			- 虚部之间的关系
			  collapsed:: true
				- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202401191128332.png)
			- 运算和性质
			  collapsed:: true
				- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202401191131134.png)
-