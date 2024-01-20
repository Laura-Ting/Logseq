- 官网：[https://www.matthewtancik.com/nerf]
- 一句话概括：用二维图像去重建出三维场景，虚拟相机可以游走
- 优点：
  collapsed:: true
	- nerf的画质较高
	- 深度结果准确（灰度图）
	- 深度重建质量高（小球，遮挡关系）
	- 可输出三维方格拖拽场景
- 论文
  collapsed:: true
	- nerf：神经辐射场，一种空间的表达方式用来做视角合成
	- introduction：
	  collapsed:: true
		- 提出了视角合成的新方法，静态，不能运动
		- 使用一个连续的5维的函数，包括方向角信息（θ，φ），空间点的（x，y，z）
		- 输入到一个MLP（全连接神经网络）中，将这个5维的输入转换成：密度信息+视角有关的RGB信息
		  collapsed:: true
			- 密度值从各个方向上看上去应该是不变的
			- RGB从不同视角看上去是变化的
		- 三点贡献
		  collapsed:: true
			- 使用了5维的神经辐射场
			- 使用了一个经典传统的体渲染技术，利用这个技术去做可微渲染
			- 将这个5维输入转换到更高维的空间，使用position encoding（位置编码）
	- methodology：
	  collapsed:: true
		- 输入：(x, y, z, θ, φ)
		  collapsed:: true
			- 想象成一条射线射过去，同一射线上的点的xyz信息不同，但方向角信息相同
			  collapsed:: true
				- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202401061037425.png){:height 218, :width 330}
		- 输出：颜色信息+密度信息
		  collapsed:: true
			- 颜色信息RGBα，比如说，空气是白色，车身是黄色
			  collapsed:: true
				- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202401061041976.png){:height 254, :width 354}
			- 密度信息
			  collapsed:: true
				- 再利用体渲染公式将这些值映射回来，即得到我们预测出来的像素值
				- 将预测出来的像素值和图像本来的像素值去做一个MSE Loss，优化这个损失，就是优化这个神经网络
				- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202401061044114.png){:height 173, :width 193}
		- 体渲染公式：
		  collapsed:: true
			- r(t) = o + td，就是表示这条射线上的点
			  collapsed:: true
				- o表示原点
				- td表示方向
			- 公式
			  collapsed:: true
				- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202401061050792.png){:height 79, :width 599}
				- 对tn(tnear离相机最近的地方)和tf(tfar离相机最远的地方)进行积分，场景是有界的
				- σ就是不透明度
				- c是颜色值，和r(t)（这个点的位置）与d（看过去的方向）有关
				- T是指光的衰减程度，T(t)到达这个点时我的光强还剩下多少，经过一个不透明物体时光强会迅速衰弱，而空气中衰弱就会比较慢
				- 这个点反射的颜色，是本身还剩下的光强T和不透明度σ的乘积（越不透明，反射的光强越强），反射还要颜色，所以也要和c相乘
				- 把这一条射线上的点都带入公式，然后求积分，就得到了RGB
			- 理想化
			  collapsed:: true
				- 因为t是一个连续化表达，但实际上只能采用离散采样点
				- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202401061108020.png){:height 56, :width 324}
				- 将tf到tn分成N份，采样N的点，ti就是每个点
		- 位置编码：
		  collapsed:: true
			- 假如没有的话，会缺失很多细节
			- 利用的是神经网络的特性：输入一个低频信号，输出也会是一个变化比较低频的东西
			- 比如输入两个距离很近的点，那么输出的RGB也就是比较平滑的，相邻颜色差距较小
			- 思路：将输入的东西变为高频，使用周期信号sin，cos
				- 想映射到几维，L就取几，构成周期信号
				- 注意，一般只是对xyz做，而不对方向角做
				- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202401061116852.png){:height 69, :width 541}
		- 分级采样：
		  collapsed:: true
			- 之前点的采样是均匀采样，从tn到tf很多点是没有贡献的，很多点密度都是0，对最后的成像没有任何影响，不高效
			- 粗网络：首先用等间隔采样得到一个nerf，会得到他的σ密度变化曲线
			- 细网络：之后按照密度采样，在σ较小的地方采少量的样本因为它很可能是空气，在σ更大的地方做更多的更精细的采样
- 代码
	- 调试工具：ipdb
	  collapsed:: true
		- s进入函数
		- n一步一步调试
	- 调试过程
		- 进入train函数
		- 查看命令行参数
		  collapsed:: true
			- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202401061142886.png)
			- N_rand光线采样：随机采样1024
			- N_samples每条光线粗阶段采样64个点
			- 程序分chunk运行
		- 数据集匹配到llff
		  collapsed:: true
			- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202401061145964.png){:height 124, :width 709}
			-
		- 进入到load_llff_data中
			- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202401061146193.png){:height 97, :width 655}
			- 这个函数很重要，如何解析nerf数据集，如果想要训练自己的模型，就要写一个自己的load函数
			- 参数
				- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202401061152382.png){:height 239, :width 613}
				- 数据路径datadir，下采样参数factor（是一个数字，代表几倍下采样）
				- poses是相机位置，bds是相机的场景范围（深度范围），imgs图像本身
				- 处理poses
					- 对poses进行解析，实际上是进行了一个坐标转换，poses（3，5，20）是3*5的列表，20是20张图像
					- imags的shape是378*504，有20张，每个pose对应一张图像
					- poses实际上是从poses_bounds.npy文件中读出，npy是数据集所提供的
					- poses坐标转换
					  collapsed:: true
						- ```
						  poses = np.concatenate([poses[:, 1:2, :], -poses[:, 0:1, :], poses[:, 2:, :]], 1)
						  ```
						- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202401061202020.png){:height 230, :width 332}
					- 将batch移到第一维
					  collapsed:: true
						- ```
						  poses = np.moveaxis(poses, -1, 0).astype(np.float32)
						  ```
					- 对相机位姿做中心化
						- 我们刚刚对poses坐标变换的时候就是在变换世界坐标系
						- 现在是将原来的坐标系（重建物体处）搬到相机集中处
						  collapsed:: true
							- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202401061210734.png){:height 218, :width 310}
						- ```
						  poses = recenter_poses(poses)
						  ```