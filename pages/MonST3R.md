- MonST3R:A Simple Approach for Estimating Geometry in the Presence of Motion
- Abs：
  collapsed:: true
	- 从动态场景中估计几何由于物体的移动和形变是困难的。目前方法大多依赖于多阶段或全局优化。monst3r处理动态视频，估计每个时间步的点云和每一帧的相机位姿和内参。
	- 困难是缺少合适的训练数据，我们把它视作一个微调任务，识别几个合适的数据集，在有限的数据集上做有策略的训练。
	- 引入新的优化对于下游任务，比如深度和相机位姿估计
- Intro：
  collapsed:: true
	- 对于静态场景，dust3r引入了新的范式去直接回归出场景几何。给定一组图片，dust3r产生点图表示，把图片中的每个像素和相关的3D位置关联，并且把这对点图和第一帧的相机坐标系对齐。对于多帧，dust3r积累pairwise匹配到一个全局点云，并且用它解决多个3D任务，比如单帧深度估计，多帧深度估计，相机内参和外参估计。
	- 我们的想法是每一时间步都可以估计出点图，将它们表示在同一摄像机坐标系中对于动态场景仍然具有概念意义。
	- 但是由于DUSt3R的训练数据只包含静态场景，因此DUSt3R不能正确地将场景的点图与运动物体对齐；它往往依赖于移动的前景对象进行对齐，从而导致对静态背景元素的错误对齐。
	- 其次，由于其训练数据大多由建筑物和背景组成，DUSt3R有时无法正确估计前景物体的几何形状，而不考虑它们的运动，并将它们放置在背景中。原则上，这两个问题都源于训练和测试时间之间的域不匹配。
- Method：
	- Background and Baseline
		- 模型架构
			- 基于dust3r，ViT架构，以半监督模式在夸视角任务上预训练。两个输入图像输入一个共享的encoder中，基于transformer的decoder通过cross-attention处理输入特征，两个单独的head输出对齐后的第一帧点图和第二帧点图
		- baseline with mask
			- 在image和token上去掉动态物体作为我们的baseline，这带来位姿估计的退化，可能和ood有关
	- Training for Dyanamics
	  collapsed:: true
		- main idea：和dust3r类似，两帧It和It’，点图  ![Replaced by Image Uploader](https://raw.githubusercontent.com/Laura-Ting/blog-images/master/202503181402992.png){:height 30, :width 115} ，置信度图 ![Replaced by Image Uploader](https://raw.githubusercontent.com/Laura-Ting/blog-images/master/202503181403160.png){:height 33, :width 113}。与DUSt3R的关键区别在于，MonST3R中的每个点地图在时间上都与单个点有关
		- 训练数据集：合成视频数据集。在训练过程中，我们对数据集进行非对称采样，在PointOdyssey (更多动态的、铰接的物体)上增加权重，在TartanAir (良好的场景多样性但静态性差)和Waymo (一个高度专业化的领域)上减少权重。对图像进行降采样，使其最大维数为512。
		- 训练策略：由于数据集size较小，我们采用了几种训练策略最大化数据有效性。
		  collapsed:: true
			- 首先，在冻结编码器的同时，我们只对网络的预测头和解码器进行微调。该策略保留了CroCo特征中的几何知识，应减少微调所需的数据量。
			- 其次，我们通过采样两个时间步长从1到9不等的帧来为每个视频创建训练对。采样概率随步长线性增加，选择步长9的概率是步长1的2倍。这给我们带来了更大的相机和场景运动的多样性，也更重了更大的运动。
			- 第三，我们使用具有不同图像尺度的中心作物的视场增强技术。这鼓励模型在不同的相机内参之间进行泛化，尽管这种变化在训练视频中相对较少。
	- Downstream Applications
	  collapsed:: true
		- 内参和相对位姿估计
		  collapsed:: true
			- 由于相机内参估计是基于在自己坐标系下的点图，dust3r中的假设和计算仍然成立，只需要通过相机内参Kt求解ft
			- 动态物体违反几何一致性（对极几何，Procruste对齐），不能直接用于，我们使用RANSAC和PnP，在大多数像素是静态的情况下，点的随机样本将更侧重于静态元素，并且可以使用内点稳健地估计相对位姿。
		- 置信的静态区域
		  collapsed:: true
			- 利用图像光流和相机运动的光流作差，静态区域会是一直的。相机的光流通过估计出的相机参数。
			- ![Replaced by Image Uploader](https://raw.githubusercontent.com/Laura-Ting/blog-images/master/202503181446715.png){:height 34, :width 327}
			- ![Replaced by Image Uploader](https://raw.githubusercontent.com/Laura-Ting/blog-images/master/202503181446482.png){:height 42, :width 218}
			- 现成光流使用SEA-RAFT计算
	- Dynamic Global PCD and Camera Pose
	  collapsed:: true
		- video graph
		  collapsed:: true
			- dust3r采用了连通图构建所有的pairwise帧，十分昂贵。我们用sliding temporal window
			  collapsed:: true
				- 视频 ![Replaced by Image Uploader](https://raw.githubusercontent.com/Laura-Ting/blog-images/master/202503181500648.png){:height 29, :width 115}
				- 计算在窗口内的所有pairs： ![Replaced by Image Uploader](https://raw.githubusercontent.com/Laura-Ting/blog-images/master/202503181501217.png){:height 36, :width 287}
		- 动态全局点云和位姿优化
			- 最初的目标是加速所有的点图预测到同一个全局坐标系，使用dust3r的对齐loss，添加两个video特殊的loss，camera 轨迹平滑和流投影
			- 1) 我们重新参数化全局点图Xt使用相机位姿Pt，Kt和每帧深度图Dt，这允许我们直接定义损失在相机参数上 ![Replaced by Image Uploader](https://raw.githubusercontent.com/Laura-Ting/blog-images/master/202503181546996.png){:height 44, :width 286}
			- 2)首先找到一个简单的刚体变换Pt;e把pairwise估计和世界坐标点图对齐，因为这两者在相同的相机坐标系
			- ![Replaced by Image Uploader](https://raw.githubusercontent.com/Laura-Ting/blog-images/master/202503181549912.png){:height 44, :width 455}
			- σe​ 是逐对的尺度因子。Ct;e是权重矩阵
			- 3)为了鼓励相机运动平滑，惩罚相邻时间步之间相机旋转和平移的剧烈变化 ![Replaced by Image Uploader](https://raw.githubusercontent.com/Laura-Ting/blog-images/master/202503181553129.png){:height 58, :width 383}
			- 为了鼓励全局点云和相机位姿与估计的光流一致，计算静态区域的全局光流场
			- 4)为了鼓励全局点云和相机位姿与估计的光流一致，计算静态区域的全局光流场。给定两帧t和t‘。使用他们的全局点图，相机外参和内参，我们计算光流，假设场景是静态的，从t到t’移动相机，我们把这个光流记作 ![Replaced by Image Uploader](https://raw.githubusercontent.com/Laura-Ting/blog-images/master/202503181559771.png){:height 37, :width 73}
			- ![Replaced by Image Uploader](https://raw.githubusercontent.com/Laura-Ting/blog-images/master/202503181600051.png){:height 61, :width 427}
			- 置信静态掩码是使用Sec3.3中描述的成对预测值(点图和相对姿态)初始化的。 在优化过程中，我们使用全局点图和相机参数来计算Fglobal cam和更新置信的静态掩模。
			- 整个优化过程： ![Replaced by Image Uploader](https://raw.githubusercontent.com/Laura-Ting/blog-images/master/202503181603533.png){:height 41, :width 443}
- Experiment
-