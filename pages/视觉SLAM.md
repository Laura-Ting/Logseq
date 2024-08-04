- Chapter1-2简介
  collapsed:: true
	- SLAM是什么
	  collapsed:: true
		- Simulataneous Localization and Mapping 同时定位与地图构建
		- 传感器在没有先验环境特定信息的环境下，
		  collapsed:: true
			- 于运动过程中建立环境的模型（地图，表示不唯一，空间信息也可以），
			- 同时估计自己的运动（他的轨迹）
	- 两类传感器：
	  collapsed:: true
		- 环境之中：必须在环境中设置，限制了机器人的使用范围
		- 携带在机器人身上：测量间接的物理量，而不是直接的位置信息
		- 建图难度不同：环境之中的比较容易（因为都对环境条件做出了假设），我们此处针对携带在机器人身上的（限制相比于前者小很多）
	- 在slam中传感器分类：
	  collapsed:: true
		- 激光型
		- 相机型
			- 便宜，轻便，信息丰富，计算量很大
			- 需要一定的工作条件：光线要充足，不能被挡住，环境需要有一定的纹理
	- 有关相机：
	  collapsed:: true
		- 视觉SLAM主要关注的是利用相机
		- 相机的种类
		  collapsed:: true
			- 单目Monocular：更难一些，只有一个图像
				- 优点：结构简单，成本低
				- 缺点：没有深度，通过移动相机产生深度（运动起来的时候近处的东西移动的快，远处的东西移动的慢）
				- 以及尺度不确定性
			- 双目Stereo：除了图像之外还能知道像素点离我们的距离，是个关键信息
				- 通过视差计算深度（左右眼的微小差异判断远近；近处的变化小，远处的变化大）
				- 基线baseline：双目之间的距离，baseline越大则测量的深度范围越大
				- 缺点：配置和标定复杂，深度量程受限制，视差的计算非常消耗计算资源
			- 深度RGBD：也是
				- 通过物理方法计算深度（有点类似于激光）
				- 结构光ToF，但是主动测量功耗大，测量值较准确，量程小易受干扰，可以恢复三维结构
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
				- 相邻图像估计相机运动
				- 基本原理：通过相邻两张图像计算运动和结构
				- 叫做“里程计”是因为，VO只关注当前相邻时刻，与过去和未来的信息没有关系
				- 因为有误差，不可避免的有漂移Drift，回不到原点->后端优化+回环检测
				- 方法：特征点法，直接法
		- 后端：Optimization
		  collapsed:: true
			- 从带有噪声的数据中优化轨迹和地图状态估计问题
			- 最大后验概率估计MAP：Maximum-a-Posteriori：整个系统的状态估计，以及这个状态估计的不确定性有多大
			- 前期以滤波器（扩展卡尔曼滤波器Extended Kalman Filter，EKF）为主，后期以图优化为主
		- 回环检测：Loop Closing Detection
		  collapsed:: true
			- 检测机器人是否回到早先位置，识别到达过的场景
			- 可以利用二维码等（但是这对环境做出了限制），我们用传感器本身（相机，计算图像间的相似性）去判断
			- 词袋模型
		- 建图：Mapping
		  collapsed:: true
			- 用于导航，通讯，规划，可视化，交互
			- 度量地图vs拓扑地图
				- 度量地图
				  collapsed:: true
					- 稀疏地图：只标出路标，用于定位
					- 稠密地图：建模所有能够看到的东西，用于导航，二维用网格Grid，三维用小方块Voxel（占据，空闲，未知）
				- 拓扑地图
				  collapsed:: true
					- 强调地图元素之间的关系
					- 图，由节点和边组成，只考虑节点的连通性，但是不擅长表达结构复杂的地图
		- 示意图
			- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202401151825669.png)
	- SLAM问题的数学表述
	  collapsed:: true
		- 运动（位置）
			- 离散时间t=1,2,3,...k
			- 所在位置x1,x2,x3...,xk
			- 他们都要看成随机变量，服从概率分布
			- 从上一时刻运动到下一时刻的运动方程：xk=f(xk-1, uk, wk) uk是来自传感器的输入，wk是噪声
		- 观测（位置+路标）
			- 路标（三维空间点）：y1, y2, ... yn
			- 传感器在xk出探测到了yj
			- 观测方程：zk,j = h(xk, yj, vkj)，vkj是一个噪声项
		- 上述两个基本方程：运动方程和观测方程
			- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202401151847681.png)
			- 问题
				- xk代表相机位姿（位置+姿态），在三维中是如何取描述的？向量or矩阵？
				- 如果观测是像素点，应该如何表述
				- 如果我知道输入u和观测z，希望推断自己的位置x和目标点所在位置y
- Chapter3三维空间刚体运动
  collapsed:: true
	- 刚体运动的描述方式
	  collapsed:: true
		- 旋转矩阵
		- 变换矩阵
		- 四元数
		- 欧拉角
	- 点，向量，坐标系，旋转矩阵
	  collapsed:: true
		- 点
			- 点存在于三维空间之中
			- 点和点可以组成向量
			- 点本身由原点指向他的向量所描述
		- 向量
			- 带指向性的箭头
			- 可以进行加法，减法等运算
			- 向量可以由R3坐标表示
				- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202401181205582.png)
			- 坐标系是由三个正交的轴组成
				- 构成线性空间的一组基
				- 左手系和右手系
			- 向量的运算
				- 加减法，缩放
				- 内积
					- 模长乘以夹角的余弦值
						- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202401181209725.png)
				- 外积
					- 相当于模长相乘后乘上sin，即这两个向量组成的平行四边形的有效面积
					- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202401181229695.png)
					- 我们可以发现前面乘的矩阵是一个反对称矩阵，aT=-a
		- 坐标系
		  collapsed:: true
			- 坐标系之间如何变换，如何计算同一个向量在不同坐标系中的坐标
				- SLAM中会有一个固定世界坐标系，还会有一个移动机器人坐标系
				- 旋转+平移：a‘ = Ra + t
					- 平移使用向量
					- 旋转使用旋转矩阵
						- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202401181235505.png)
						- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202401181237413.png)
						- 其中R成为旋转矩阵，旋转矩阵满足如下性质
							- R是一个正交矩阵
							- R的行列式为+1
						- 满足上述性质的也可以成为旋转矩阵
							- 特殊正交群
							- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202401181242533.png)
						- 变换
							- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202401181242207.png)
				- 齐次坐标和变换矩阵
					- 之前使用两种运动分开叠加有一点不便，叠加起来过于复杂
					- 使用矩阵进行统一
						- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202401181249387.png)
					- 齐次坐标：
						- 用四个数表达三维向量
						- 也就是下面多加一行1，如果下面没有多加，那么就叫做非齐次坐标
					- 变换矩阵：
						- 平移和旋转放入同一个矩阵
						- 这种矩阵也叫特殊欧式群
							- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202401181252040.png)
						- 求逆也比较简便
							- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202401181253846.png)
	- 三维旋转：
	  collapsed:: true
		- 方向为旋转轴，长度为转过的角度
		- 称为角轴or旋转向量 w=θn
		- 与旋转矩阵不同的是
			- 旋转矩阵：9个量，有正交约束和行列式约束
			- 角轴：3个量，没有约束，也就是李代数
			- 转换
			  collapsed:: true
				- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202401181700874.png){:height 254, :width 283}
		- 欧拉角
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
			- 和角轴的关系
			  collapsed:: true
				- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202403132001843.png)
			- 四元数旋转一个空间点
				- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202403132004812.png){:height 320, :width 780}
				- 为什么不是直接q×p，因为这样实部不为0，跑到了四维空间里去，但这样计算完实部一定为0
- Chapter4
- Chapter5相机与图像
  collapsed:: true
	- 针孔相机模型，内参与径向畸变参数
	  collapsed:: true
		- 相机模型
		  collapsed:: true
			- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202403202159042.png)
			- 相似三角形，这里面有一个负号，但是实际上相机会自己翻转过来，我们一般翻转到前面处理
		- 成像平面到像素坐标
		  collapsed:: true
			- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202403202205243.png){:height 397, :width 203}
			- 一般成像单位的坐标以米来计量，一米可以有很多个像素，不同分辨率的像素个数也不一样，需要乘上α进行转化（缩放），米和像素之间转化，c代表中心的移动
			- μ和v是以像素为单位，X‘和Y’是以m为单位
			- fx是α*f，一般来说α和β差不多
		- 内参
		  collapsed:: true
			- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202403202214631.png)
			- 中间的矩阵称为内参数，通常在相机生产之后已经固定
			- 左侧是其次坐标，右侧是非齐次坐标
			- 改变Z时，投影点仍是同一个
			- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202403202220838.png)
			- 比如说在这条线上都是会投影到P，因为是经过相机中心的
		- 外参
		  collapsed:: true
			- 从世界坐标系变到相机坐标系
				- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202403202225026.png)
				- R和t(T)：旋转和平移，即相机位姿，称为外参，外参是不断变化的
				- 右侧的式子隐含了一次非齐次到齐次的变化,T是4x4，Pw是4x1，但K是一个3x3的矩阵，需要把最后一维归一化，但是原来最后一维也就是1没什么用，所以取出前三维即可
			- 外参是SLAM估计的目标
		- 过程：世界坐标->相机坐标->归一化平面(三个量都除以z，把z变成1)->像素
		- 畸变：针孔前的镜头会引入畸变
		  collapsed:: true
			- 产生
			  collapsed:: true
				- 镜头前面的凸透会使光线发生聚焦
				- 广角镜头畸变，鱼眼镜头畸变，镜头越边缘的地方弯的越严重
				- 这样就不满足小孔成像，我们需要把畸变描述出来
			- 类型
			  collapsed:: true
				- 径向畸变
				  collapsed:: true
					- 与像素距离中心的距离有关
					- 桶形失真相当于把径向增大，枕形失真相当于把径向减小
					- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202403202242316.png)
				- 切向畸变
				  collapsed:: true
					- 与这个点和中心的夹角有关
					- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202403202242833.png)
			- 数学描述
			  collapsed:: true
				- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202403231538828.png)
				- 径向：如果单纯只是径向，r=根号下(x^2+y^2) ，如果没有畸变的话k1， k2...这些参数就是0 ，后面的次数也可以扩展
				- 切向：一般用x和y的交叉项表示
		- 小结
		  collapsed:: true
			- ![image.png](../assets/image_1711179827050_0.png)
		- 双目
		  collapsed:: true
			- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202403231549850.png)
			- 可以看出量程和基线成正比，这个d还和图像的纹理有关
		- 深度相机
		  collapsed:: true
			- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202403231554318.png)
			- 但是如果材料把光吸收了，或者光直接透过去了，深度就测不到
			- 红外光在室外很容易收到太阳光的干扰，有会互相干扰
	- OpenCV的图像存储与表达能力
	  collapsed:: true
		- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202403231608824.png)
	- 基本的摄像头标定方法（不讲）
- 回环检测与建图
  collapsed:: true
	- 回环检测与词袋
		- 意义
			- VO和后端都存在误差
			- SLAM的建图和定位时耦合的——误差将会累计
				- R，t，单目中的scale
		- Loop Closing步骤
			- 检测回环的发生
			- 计算回环候选帧与当前帧的运动，可以用来作为一个约束
			- 验证回环是否成立
			- 闭环
	- 建图
	- 展望
-
- SLAM的提出：
  collapsed:: true
	- 我在当前环境的什么位置
	- 当前所在的环境是个什么样子
- ICRA
- 机器人的模型（从底到上）
  collapsed:: true
	- 执行器层及数学模型：电机
	- 控制层：怎么让电机有一个稳定的转速，每个模块pid的控制器，aoq控制器等
	- 感知，定位以及构图层 ☆slam在这一层
	  collapsed:: true
		- 之前的单独性定位方法
		  collapsed:: true
			- 利用automitry？编码器差分：左右轮子单独测速，偏转角，在位置上的积分，对于地面（x+y+航向角），但是没有东西去矫正偏差，这个挑战：我有传感器，能得到估计迹，利用贝叶斯概率把控制量（轮子的速度）和观测量（传感器），我先定位把地图构建出来，再基于特征点反定位，用贝叶斯先验概率和后验概率来融合，做一个更强的估计
			- landmarks贴上一些标志点
		- 缺点：只有一种传感器/感知架构，不够鲁棒性
	- 任务规划或决策层
- SLAM在做的事
  collapsed:: true
	- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202403101913845.png){:height 222, :width 399}
	- IMU一般用于做自身估计，包括三个，角速度计，加速度计，磁场计
- SLAM的提出与发展
  collapsed:: true
	- 两种SLAM
		- 可见光视觉：特征，照度
		- 激光：Scan Matching
	- 发展：
		- EKF-SLAM卡尔曼滤波
			- 回归到一个点
			- 实际上是贝叶斯滤波器的一种特殊情况，假设系统中噪声服从高斯分布
		- FAST-SLAM粒子滤波器
			- 随机撒点，在通过观测
			- 但是必须现有地图
		- Graph-SLAM
			- 自己的位置点+环境的特征点，用线连起来，基于图优化直接做高精度定位
		- Javier Civera Andrew J.Davison
			- 单目EKF SLAM：用更廉价的设备做更深刻的信息
			- SLAM++
		- Georg Klein，David Murray
			- PTAM-SLAM开源，AR验证，整个pipeline定义，多线程，图优化
		- RGBD-SLAM，ORB-SLAM，LSD-SLAM
		- 激光SLAM方面
		  collapsed:: true
			- GMapping，SLAM6D
			- Zhang Ji：LOAM，V-LOAM
			- Cartographer：google开源的项目
			- BLAM
			- SegMatch：点云没有意义，需要理解分割
- 从滤波器弹SLAM #
  collapsed:: true
	- 融合
		- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202403102007832.png){:height 378, :width 577}
	- 整个过程
		- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202403102009273.png)
		- Encoder编码器：测速，来估计自己的位置，xyz和航向角
		- Prediction：通过predicted position，估计自己的位置
		- Observation：通过外界的激光传感器再做Observation，因为我在定位时也有感知，进行matching
		- Matching：再去做一个基于观测量的估计
		- Estimation融合，更新，联合估计：这两个东西再去融合得到一个更准确的定位，position update就是进行修正
	- 数学表示
	  collapsed:: true
		- 给定
			- 机器人的控制量Uk
			- 对于环境特征的观测Zk
		- 估计
		  collapsed:: true
			- 特征图（构图）m
			- 机器人的位置Xk
		- bel置信概率
		  collapsed:: true
			- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202403102021271.png){:height 315, :width 514}
			- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202403102021807.png){:height 337, :width 532}
			-
			-
	- 缺点
		- 需要依赖于状态量xk和xk+1，还要依赖于特征，特征需要有很多，时效和存储
- 图优化 #
  collapsed:: true
	- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202403102029772.png)
	- 真正描述的实际上是状态量和地图特征之间的联系，马尔科夫随机场假设未来的和当前的没有任何关系
	- 可以通过速度，对时间积分，从上一个状态xi估计到下一个状态xj
	- 通过环境观测，又能估计出一个位置
	- 按理来说这两个位置之间误差应该是0，实际上误差eij应该是0
	  collapsed:: true
		- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202403102034385.png)
		-
	- 和滤波器处理的思路是一样的，但是转换为线性优化的问题
	- 基于滤波器因为有先验后验，所以必需要设立起关联和联系，滑动窗口，前3副，前十副这种
	  collapsed:: true
		- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202403102037825.png)
- SLAM知识架构
  collapsed:: true
	- 整体架构
		- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202403102038017.png)
	- 传感器
	  collapsed:: true
		- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202403102041702.png)
	- 基本理论
	  collapsed:: true
		- 滤波
		  collapsed:: true
			- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202403102045331.png)
			- x‘是上一时刻状态，u是控制量，x是当前状态
			- 通过后验概率，再做联合估计，进行更新
			- 粒子滤波器：基于蒙特卡洛采样，进行重定位
		- 坐标系
		  collapsed:: true
			- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202403102049492.png)
		- 相机模型
		  collapsed:: true
			- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202403102159760.png)
			- 真实场景到相机平面的一个投影，视觉中心在原点，从视觉中心到相机平面是焦距
			- 长焦距相当于图像靠近视觉中心，图像偏小，可视区域也不一样
			- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202403102202184.png){:height 323, :width 514}
			- 立体视觉，两帧之间分pose，position+orientation两个信息，观察图像特征，通过找apipollho？两个图像之间有极限集合的关系，构成三角面，计算几何
		- 位姿估计与融合，关于里程计
		  collapsed:: true
			- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202403102217802.png)
		- 回路和图优化
		  collapsed:: true
			- 图优化在干什么
			  collapsed:: true
				- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202403102218469.png){:height 317, :width 514}
				- 我们要把所有可能有关系的那些帧，找出他们之中对应的联系，用点和边串联起来，基于线性优化，减小误差
			- 词袋模型
			  collapsed:: true
				- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202403102221889.png)
				- 怎么样检测回路，黑色的椭圆用于描述置信概率，一直在增加，后来稍微收敛了一点，因为收到地图观测的影响，但是误差一直在增大的趋势是无法减小的，长航式导航
				- 回到起始点的时候对不上了，但是通过检测以前来过这样一个地方，词袋模型
				- 图像rgb三个layer，三个matric，特征检测和特征匹配，非常费时间，基于区域，swift，微软谷歌提出vector表示，每个图片里面feature是固定的，把feature用classifier，K-means做聚类，图片里只会有某些类的特征，实际上就是对图像做编码，然后再识别他的回路，然后判断相似度就行，假如说相似度很大，那么就认为之前来过这条路
				- 一旦找到了走过的路，可以接着用图优化来做线性，误差压缩，增强估计
		- 地图和环境表达
		  collapsed:: true
			- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202403102232555.png)
			- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202403111310471.png)
			- 拓扑地图：拓扑连接，图优化图搜索，计算几何
			- 语义地图：这样就知道每一块点云是什么东西
			- 特征地图：视觉基础上，特征提取，特征表达，特征点云（eg银行，摩天大楼，简化了数据量，数据表达的步骤）
		- 深度学习
		  collapsed:: true
			- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202403111314063.png)
			- 单目深度估计：结构光，深度的提取，给每一个点打上标签之后就可以拿到点云，但是单目没有，但是深度学习可以估计，这样直接用icp就可以做slam
			- 环境分割识别：有点云，分割出来，打上颜色。语义：基于CNN，RNN完成三维物体识别和分割，在pointnet中，场景理解
			- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202403111322090.png)
			- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202403111336245.png)
			- slam提供定位，端到端不仅取决于二维，还取决于depth
			- rss
			- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202403111337533.png)
			- deep learning，attention model权重，区域特征，语义特征，LSTM
- ROS机器人操作系统，多进程处理
  collapsed:: true
	- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202403111340318.png)
	- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202403111342731.png)
	- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202403111343343.png)
	- GAZEBO仿真器，可以仿真相机和激光雷达，V-REP仿真可以和ros之间通信
	- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202403111349674.png)
	- Node节点可以认为是一个进程
	- Message就是从驱动中拿到的数据
	- Service做一些修改
	- Master在那上面挂载node，就是什么master
-
- SLAM基本理论一：坐标系，刚体运动和李群
  collapsed:: true
	- SLAM的数学表达
		- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202403122138299.png)
		- 滤波器：预测+观测
			- 给定控制量u1:T和观测量z1:T，想要得到环境的地图m和机器人的路径x0:T
			- 数学表达：XIMU = [WBP WBV WBa BWq wB ba bg]
				- W：World frin，B：Body frin
				- P是位置，V是速度，a是加速度，q表示方向（航向），w是角速度，ba是加速度计，bg陀螺仪的bias？
				- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202403122152584.png)
				- P就是x，y，z，V是P的导数，a是V的导数
				- R用的是欧拉角，但是欧拉角有弊端，美式坐标系（vs苏氏坐标系）
				- a是常量，bcd是虚数（分别在三个轴上的分量）
				- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202403122200832.png)
				- q将角增量变成简单的矩阵相乘，优势很大
				- 单目估计Pk+1，Pk
				- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202403131213796.png)
				- F就是由上面那些PVa...这些构成的列向量加上右边剔除后组成的矩阵
				- w是噪声，Xk就是估计
				- 再利用卡尔曼增益做更新
				- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202403131216419.png)
				- 得到一个自身状态，还得到一个通过观测推出的自身状态，两者之间肯定有差异，我们就要做补偿，传感器融合
				- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202403131218133.png)
				- 绿的表示自己的位置和特征点之间存在的关系
				- 但是存在状态矩阵爆炸的问题
		- 图：状态推算，估计位置和实际位置不重合，利用最小二乘线性回归问题，优化整体位姿
		  collapsed:: true
			- 图优化：
				- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202403131220264.png)
				- 运动：平移t+旋转R
				- 基于自身状态的估计，基于地图的估计，两者相差的error最小化，再矫正
				-
		- 对比：
			- ![](https://raw.githubusercontent.com/qugushihua/blog-images/master/202403131224427.png)
			- 滤波器：估计的越不准，高斯椭圆就越大
	- 欧式坐标系和刚体姿态表示
	  collapsed:: true
		- 比如说摄像头放在无人机上，哪个坐标系才是真正的坐标系？
		  collapsed:: true
			- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202403131233079.png)
			- 本体坐标系：无人机自身和摄像头都算，没有平移，没有旋转，因为固定，无法检测；但是这定义了自身的航向，是很有必要的。但是我们需要考虑到，所有的参照都是在全局World坐标系
			- 世界坐标系：World
			- Vision：地图，我们是利用相机去看在他本体坐标系下的图片，我相对于相机去看也要有计算的关系，做投影时，这个就是相机的外参：相机看到的世界和他的转换关系，如果一直能拿到外参就很厉害
		- 转化
		  collapsed:: true
			- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202403131235971.png)
			- XYZ是世界坐标系，uvw是本体坐标系
			- 初始化的时候两者是重合的
			- 点乘
			  collapsed:: true
				- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202403131237371.png)
				- 相互垂直的点乘为0，相互重合的点乘为1
			- 旋转：
			  collapsed:: true
				- 本体坐标系发生旋转，如何转化到世界坐标系中
				- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202403131238283.png)
				- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202403131239363.png)
				- 我们先来看二维
				  collapsed:: true
					- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202403131241497.png)
					- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202403131242977.png)
					-
			- 平移：
			  collapsed:: true
				- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202403131243616.png)
				-
			- 转换矩阵
			  collapsed:: true
				- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202403131247328.png)
				- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202403131247534.png)
				- 只有旋转的话则无法解，满足的量不够，因为只有变量没有常量
		- 存在的问题->提出四元数
		  collapsed:: true
			- x，y，z顺序不一样则结果不一样
			- 当两个坐标轴重合时，自由度从3退化成2，万向锁问题
		- 四元数：一个向量围绕着另一个向量的旋转
		  collapsed:: true
			- 使用向量，就不需要考虑三个轴的问题了
			- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202403131254717.png)
			- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202403131255281.png)
			- 原本是二维，后来推广到三维
			- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202403131901629.png)
	- 李群，李代数
	  collapsed:: true
		- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202403131902787.png)
		- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202403131903924.png)
		- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202403131908428.png)
		- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202403131909080.png)
	- Eigen
		- 一般来说，是旋转需要进行单位化
-
- KItti：数据集
- mpu6090
- https://github.com/EricLYang
- https://zhuanlan.zhihu.com/p/649761612
- https://zhuanlan.zhihu.com/p/39628699
- https://zhuanlan.zhihu.com/p/400020784
- https://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=1638022
- https://ieeexplore.ieee.org/document/1678144?denied=
- 单目深度估计模型，通过原始图片估计单目深度，再用icp的方式orRGDB得到特征点云，再基于pnp投影，检算位姿。但是这完美解决了单目上拿不到尺度信息，因为单目没有参照物，不想imu，因为我们这个只是图像，外参内参，一般来说外参没有意义，因为我们要求相机不能动才有意义
- https://robots.ros.org/
-