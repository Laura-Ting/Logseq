- NerF-SLAM-Benchmarking
  collapsed:: true
	- 题目：Benchmarking Implicit Neural Representation and Geometric Rendering in Real-Time RGB-D SLAM
	- 摘要：
	  collapsed:: true
		- 隐式神经表示(INR)缺乏一个统一的评估的表示。第一个开源基准框架去评估广泛常用INR的表现和建图以及定位的渲染方法
		- 目的：(1)直观地了解不同的inr和呈现函数如何影响映射和定位(2)建立一个统一的评估协议。设计选择可能影响建图和定位
		- 在这个框架下，我们做了很多实验，提供也很多选择INR和几何渲染函数的见解：比如稠密特征网格比其他INR(tri-plane和哈希网格)，即使几何和颜色特征被联合编码、为了把发现扩展到实际场景中，一个混合编码策略被提出去带来最好的精度和基于网格和基于分解的INRs补全。我们进一步提出显式混合编码为了高清稠密网格建图去核RGB-D SLAM配合，增强鲁棒性和计算效率。
	- 简介：
	  collapsed:: true
		- 介绍SLAM，介绍NeRF，介绍两者结合
		- NeRF-SLAM结合大体分为两类：(1)以NeRF为中心的方法，NeRF被用于场景重建和位姿估计。(2)以SLAM为中心的方法，定位由其他SLAM系统提供。
		- 限制：
		  collapsed:: true
			- 缺少统一和全面的基准框架
			  collapsed:: true
				- 这阻碍了不同的在RGBD-SLAM方法中NeRFs的比较。SOTA的NeRF-SLAM方法经常表现出关于 INR 和渲染以外的组件的多种策略，例如，如何选择训练数据（参见 SLAM 中的关键帧选择）。这使得直接比较系统并捕捉 NeRF 设计的实际进展几乎是不可能的。因此，在设计不同用途的SLAM系统时，必须了解NeRF的个体特征。
			- 缺乏NeRF组件变化对SLAM性能的评估
			  collapsed:: true
				- NeRF通过隐式神经表示F和几何渲染G定义，有很多种影响SLAM系统的性能的变体。F的网络架构的选择，例如，对于准确和有效的学习场景表示很重要。某些框架，egTri-plane的联合颜色和几何编码可以快速收敛但损失重要细节，这影响了建图和跟踪的精度。更多的，用于聚合几何和颜色信息的渲染方法也很重要，因为渲染像素的质量直接影响位姿和地图更新。尽管，高精确度的方法改进渲染质量，他们总体上增加了计算需要并且可能在不精确的位姿估计下表现的不好
		- 贡献：
		  collapsed:: true
			- 统一RGB-D SLAM框架下nerf的综合评价
				- 我们提出一种新的RGB-D SLAM基准框架，特征是一个统一的评估协议用于有效的评估不同的NeRF组件。我们从以 NeRF 为中心的范式展开我们的基准测试。 它涵盖了五个主要变量，在图2中分为两类，即统一的SLAM框架（包括统一实施的采样、训练和关键帧选择）和作为G和F组合的NeRF。我们的主要目标是研究 NeRF 如何在两个既定场景（实验室和实际场景（第 3.4 节））中的统一控制配置（第 3.2 节）下影响 SLAM 性能。
			- 关键的见解和衍生的新设计
				- 我们揭示了 INR 在 RGB-D SLAM 问题中的性能显着差异归因于它们的结构范式。 具体来说，简化 4D 特征空间的混合表示（例如哈希网格和三平面）通常需要对几何和外观进行单独编码才能实现最佳性能，而完整形式的表示（例如密集网格和纯 MLP）则不需要 这。 我们还发现，在具有完整轨迹循环的数据集中，例如 复制数据集[39]，基于网格的INR（哈希网格和密集网格）表现出更好的性能，而基于分解的INR（三平面和分解）在随机、无循环轨迹下表现出优异的功效，例如 [1]中的序列。 这种现象启发我们提出一种基于网格和基于分解的方法的新颖混合。
			- 扩展建图的工程技巧包
- SLAM
  collapsed:: true
	- ![ORB-SLAM2.pdf](../assets/ORB-SLAM2_1715914635704_0.pdf)
	- ORB-SLAM2：an Open-Source SLAM System for Monocular, Stereo and RGB-D Camerras
	  collapsed:: true
		- ![image.png](../assets/image_1715914395873_0.png)
		- Tracking：跟踪，视觉里程计，通过视觉算法估计相机的运动轨迹
		  collapsed:: true
			- 输入逐帧提取特征点，用一个运动模型去预测初始位姿(通常是恒速模型)，用PnP之类的计算优化相机位姿，根据关键帧选取策略来选取关键帧
		- Local Mapping：根据刚才选出的关键帧，把这一帧注册到地图里面，再更新一下点云地图(新点插入和修剪之类)，这部分也会有优化来确保关键帧和地图的准确
		- Loop Correction：回环检测和回环矫正，到过的地方
		- Full BA：根据所有关键帧的信息和点云地图联合优化
- NeRF-SLAM
  collapsed:: true
	- 把NeRF塞到后端地图构建的部分
	- ![Orbeez-SLAM.pdf](../assets/Orbeez-SLAM_1715915497881_0.pdf)
	- Orbeez-SLAM：A Real-time Monocular Visual SLAM with ORB Features and NeRF-realized Mapping 前端用ORB-SLAM2追踪，后端用NeRF建图
	  collapsed:: true
		- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202405171111751.png)
		- 追踪线程：获知相机位姿
		- 建图线程：根据关键帧上的特征点建立稀疏点云，用稀疏点云和带有位姿的信息训练NeRF，最小化重投影误差稀疏点云给NeRF提供指导，NeRF的渲染结果又填充稀疏点云使其变得稠密
		- 评价：是很好的框架，但是对创新的指导意义不大，而且两者没有融合
	- ![NICE-SLAM.pdf](../assets/NICE-SLAM_1715916318614_0.pdf)
	- Nice-SLAM：Neural Implicit Scalable Encoding for SLAM 实现NeRF和SLAM的耦合，前身是iMAP
	  collapsed:: true
		- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202405171146661.png)
		- 从右往左看，由相机匀速运动模型得到相机初始位姿，用初始位姿做体渲染
		- 使用多分辨率体素网格储存场景，总共3层，coarse，mid，fine
		- 体渲染时，对同一个采样位置，分别到不同分辨率的网格里面用三线性插值对格点存储的特征插值，然后送到小的，不同的MLP中解码积分渲染，得到深度图和RGB图，最小化和真实颜色和深度之间的误差，更新特征，同时优化地图和相机位姿
		- 评价：很好的融合，但是赶不上传统的ORB-SLAM2，Nice-SLAM的体素网格不能扩展，但是传统的只要内存足够就可以扩张
	- Vox-Fusion：Dense Tracking and Mapping with Voxel-based Neural Implicit Representation
	  collapsed:: true
		- 输入数据RGB-D，追踪线程：从初始位姿做射线采样，体渲染得到渲染图和真实图求误差更新pose
		- 地图存储在八叉树里面，想要扩展很方便
		- 渲染时：取采样点，对格点上的特征做三线性插值，再MLP解码
		- 前后端共用的数据：关键帧，地图网格，隐式MLP
		- 后端建图：动态体素的创建，实现地图扩展的关键 ，和前面追踪类似，但是是分开的
		- 评价：相比于Nice-SLAM的模块化划分更加清晰，但是为了地图扩展牺牲了一部分场景预测的能力
	- 总结
	  collapsed:: true
		- 1．成熟的传统SLAM方法不能契合基于NeRF的后端地图（传统SLAM基于特征点，NeRF重建基于网格和隐式特征);
		- 2．没有较好的相机位姿估计方法(只能通过优化的手段来求取位姿);
		- 3．不能完整发挥NeRF的重建能力（相机位姿不准确、训练视角少、视角分布受限、训练时间短)。
	- Co-SLAM
	  collapsed:: true
		- 摘要
		  collapsed:: true
			- Co-SLAM是一个神经RGB-D SLAM系统，基于混合场景表达，能够实现鲁棒的相机位姿估计和高保真的实时表面重建。
			- Co-SLAM把场景表示为多分辨率网格，利用了它高速的收敛和表达高频局部特征的能力。(哈希网格来自Instant-NGP)。
			- 还引入了one-blob编码，有助于表面一致性以及补全之前没有见过的区域。哈希编码+one-blob这样的联合参数编码带来了实时而且鲁棒的表现，同时实现了两个目标：快速收敛和表面空洞填补。
			- 射线采样策略能够进行全局ba。之前的方法需要选取关键帧，维持一小部分的活跃关键帧来完成优化。Co-SLAM可以以10-17Hz。
		- 联合坐标和参数化编码
		  collapsed:: true
			- 为什么需要编码？直接基于坐标的方法训练慢，且对于序列数据存在遗忘问题
			- 为什么不使用正弦编码？难以进行空洞填补和场景平滑
			- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202405171402492.png){:height 247, :width 402}
			- 没有渲染体密度是为了后面做深度监督更方便一些，效率高一点，更加注重表面渲染
			- one-blob编码：让不同的位置之间产生关联，从单点变到区域，从而预测周围，类似于one-hot但是符合高斯分布，空洞填补和场景平滑
			- 哈希编码：用哈希索引的特征网格，去哈希表中查询格点特征，格点特征做三线性插值，使用类似于Instant-NGP的多分辨率网格1-L个
			- 没有用视角编码：对于依赖于视角的反光没有那么好
			- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202405171413172.png)
		- 体渲染
		  collapsed:: true
			- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202405171413581.png){:height 191, :width 424}
			- 深度：由sdf转化而来，sdf越靠近截断距离就意味着这个点距离表面距离越远，这个分数值就越大，sigmoid输出为0-1，如果这个点刚好在表面上那么输出就是0.5，两个对称的相乘，只有在表面上的点的输出最大
		- 损失函数
		  collapsed:: true
			- L2误差，rgb和d
				- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202405171420456.png){:height 99, :width 428}
			- SDF监督
				- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202405171421643.png){:height 88, :width 410}
			- Free-space loss
				- 让落在截断距离之外的点靠近截断距离，让空的位置或者距离表面比较远的位置能够平滑一些，原本是随机分布，现在让他们都趋近SDF值
				- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202405171425688.png){:height 109, :width 355}
			- Smooth loss 特征平滑损失
				- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202405171426931.png){:height 134, :width 359}
				- 为了让相邻网格的边缘能够有相近的特征，避免出现不连续或者锯齿的特征，加一个损失让边缘附近的采样点有一个相近的特征向量，每一个△是给采样坐标加上一个小的扰动。有可能导致整个场景过于平滑，而无法建模细节，所以只能在一些随机区域里运行
		- 相机位姿优化
		  collapsed:: true
			- 李代数，恒速模型
		- 全局优化
		  collapsed:: true
			- iMAP、NICE-SLAM：根据信息增益或可视区域重叠，选取关键帧;取关键帧的部分采样射线，联合优化相机位姿和场景地图。
			- Co-SLAM：只保存关键帧的部分像素，组成关键帧像素集合;关键帧像素集合中随机,选取部分像素，先优化地图，再优化相机位姿。
		- 总结
		  collapsed:: true
			- 1．本文提出了一个稠密且实时的神经RGB-D SLAM系统;
			- 2.本文联合坐标编码和哈希编码，以及小的MLP作为场景表达，实现
			  了高质量建图和空洞填补;
			- 3．本文采用全局BA优化，提高了相机追踪精度。
- 自动驾驶-大规模场景重建
  collapsed:: true
	- NeRF
	  collapsed:: true
		- Mega-NeRF
		- Switch-NeRF
	- 3D Gaussian
	  collapsed:: true
		- Vast Gaussian: Vast 3D Gaussians for LargeScene Reconstruction 重建大规模场景，静态大场景
		- Street Gaussians for Modeling Dynamic Urban Scenes 建模动态城市场景
		- Driving Gaussian: Composite Gaussian Splatting for Surrounding Dynamic Autonomous Driving Scenes 重建动态自动驾驶场景
		- Periodic Vibration Gaussian: Dynamic Urban Scene Reconstruction and Real-time Rendering周期震荡高斯，动态
- 高斯相关
  collapsed:: true
	- Gaussian-SLAM
	- 摘要
	  collapsed:: true
		- 提出一种新颖有效的设置新高斯种子的策略为新探索区域，他们有效的优化和场景大小独立，可以应用于大场景。通过管理场景为单独优化的子地图实现，不需要在内存中保存。通过最小化帧-模型的相机跟踪最小化光度和几何损失。
	- 介绍
	  collapsed:: true
		- NeRF只适用于小的合成场景，重渲染质量也不好。
		- 高斯比NeRF快很多，场景表示直接可翻译，对下游任务理想。
		- Gaussian-SLAM，一个稠密RGBD SLAM系统
		  collapsed:: true
			- 使用3D高斯做场景表示
			- 高斯泼溅的一个扩展，更好的编码几何，允许由单个相机初始化的辐射场之外的重建
			- 在线优化，加工地图为子地图，引入有效种子和优化策略
			- 最小化光度和几何损失
	- 相关工作
	  collapsed:: true
		- 深度视觉SLAM和在线建图
		  collapsed:: true
			- Curless和Levoy提出TSDF。之后一系列工作围绕通过高效的实现和体积集成来提高速度，通过体素哈希和八树数据结构来提高可扩展性，以及使用稀疏图像特征和回环检测来跟踪。为了解决深度图不可靠的问题，RoutedFusion引入了一种基于学习的融合网络，用于更新体积网格中的TSDF。这个概念由NeuralFusion和DI-Fusion进一步发展，它们采用隐式学习进行场景表示，增强了对异常值的鲁棒性。最近的研究已经成功地实现了仅使用RGB相机的密集在线重建，绕过了对深度数据的需要。
			- 最近，测试时间优化方法由于能够适应动态的未知场景而变得流行起来。持续神经建图，比如，应用了一个持续的建图策略，从一系列深度地图去学习场景表示。
			- 最新的方法在合成数据集上表现好，在真实世界数据集上表现不好。而且，这些方法对于真实世界义勇不使用，由于计算需要，慢的速度，不能有效更新位姿，因为神经表示依赖于位置编码。相反，我们的方法展示了优秀的表现在真实数据集上。有竞争力的跟踪和运行时，使用场景表示允许自然地位姿更新。
		- SLAM的场景理解
		  collapsed:: true
			- 主要的SLAM稠密3D场景表示是基于网格，基于点，基于网络，或者混合。在其中，基于网格的技巧被最广泛的研究。他们进一步将方法划分为使用稠密网格，层次树，哈希网格为了有效的内存管理。网格提供了简单和快速的邻居搜索和上下文整合。然而，一个关键的限制是需要预定义网格分辨率，这在重建过程中不容易调整。这可能导致空区域的内存使用效率低下，同时由于分辨率限制而无法捕获更精细的细节。
			- 基于点的方法解决了一些网格相关的挑战，并已有效地用于三维重建。与网格分辨率不同，这些方法中的点密度不必预先确定，并且可以在整个场景中自然变化。此外，点集可以有效地集中在表面周围，而不会在建模空白空间上花费内存。这种适应性的代价是寻找邻近点的复杂性，因为点集缺乏结构化的连通性。在密集SLAM中，可以通过投影到关键帧将3D邻域搜索转换为2D问题，或者通过在网格结构中组织点来加速搜索，从而减轻这一挑战。
			- 基于网络的密集三维重建方法通过隐式地用基于坐标的网络建模来提供连续的场景表示。这种表示可以捕获高质量的地图和纹理。然而，由于无法更新局部场景区域和缩放更大的场景，它们通常不适合在线场景重建。最近，提出了一种结合基于点和基于神经的优点的混合表示。在解决两种表示的一些问题时，它与现实世界的场景作斗争，并且不能无缝地将轨迹更新集成到场景表示中。
			- 在这三个主要类别之外，一些研究探索了其他表征，如冲浪和神经平面。参数化表面元素通常不能很好地建模灵活的形状模板，而特征平面由于其过度压缩的表示而难以进行包含多个表面的场景重建。最近，Kerbl等人提出用三维高斯函数表示场景。通过多视图监督下的差分绘制对高斯参数进行优化。虽然非常高效并获得令人印象深刻的渲染结果，但这种表示是为完全观察的多视图环境量身定制的，并且不能很好地编码几何形状。与我们的工作同时进行的是动态场景重建和跟踪。然而，它们都是离线方法，不适合单相机密集SLAM设置。
			- 同时，已有几种方法使用高斯溅射进行SLAM。虽然大多数基于飞溅的方法使用类似的基于梯度的地图密度化，但我们采用一种更可控的方法，通过利用快速最近邻搜索和alpha掩蔽来精确阈值。此外，与所有其他并发工作不同，我们的映射管道不需要在GPU内存中保存所有3D高斯，允许我们的方法扩展，并且不会随着覆盖更多区域而减慢速度。此外，虽然在并行工作中3D高斯分布是非常密集的，但我们的颜色梯度和基于掩码的播种策略允许更稀疏的播种，同时保持SOTA渲染质量。最后，与之相反，我们的跟踪不依赖于显式计算的相机姿态导数，而是在PyTorch中实现的。
	- 方法
		- 简介
		  collapsed:: true
			- 我们引入了一种新的高效映射过程，在连续单相机设置中具有有限的计算成本，这是传统3D高斯飞溅的一个挑战。为了使传统的高斯条纹能够呈现精确的几何形状，我们通过添加一个差分深度渲染来扩展它们，显式地计算高斯参数更新的梯度。最后，我们开发了一种新的帧到模型的跟踪方法，依赖于我们的3D地图表示。图2提供了我们方法的概述。我们现在解释我们的管道，从经典高斯飞溅的概述开始，并继续进行地图构建和优化，几何编码和跟踪。
		- pipeline
		  collapsed:: true
			- 对于每个输入的关键帧，相机位姿通过活跃子图的深度和颜色损失被估计。给定一个估计位姿，RGBD帧被转换为3D并且基于颜色梯度和渲染alpha掩码下采样。来自活跃子图中低密度区域的子采样点云将用于初始化新的3D高斯分布。这些稀疏的3D高斯分布随后被添加到活跃子图的高斯点云中，并与该子图的所有贡献关键帧的深度图和彩色图像一起进行联合优化。
		- 高斯泼溅
		  collapsed:: true
			- 速度快，同时不损失渲染质量的方法。3D高斯从稀疏sfm点云中初始化。根据从不同角度观测的图片，高斯参数被优化使用可微渲染。在训练中，3D高斯被适应性的添加或移除，基于启发式
			- 高斯初始化通过均值μ∈R3，协方差∑∈R3*3，不透明度o∈R，颜色c∈R3
			- 3D到2D的高斯投影为 ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202406132243719.png){:height 46, :width 201}
			  collapsed:: true
				- 其中Twc是世界到相机坐标的变换，P是一个R4*4的投影矩阵，π：从R4->R2的像素坐标的投影。
			- 2D协方差的计算是 ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202406132247927.png){:height 39, :width 152}
			  collapsed:: true
				- J∈R2*3是仿射变换，Rwc是Twc的旋转的那部分，投影矩阵更多信息看其他论文
			- 颜色C在一个通道ch在一个像素i上受m个排好序的高斯的影响： ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202406132252012.png){:height 53, :width 294}
			  collapsed:: true
				- αj是 ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202406132253889.png){:height 45, :width 283}
				- 其中∆j ∈ R2是像素坐标和泼溅的高斯二维均值之间的偏移。3D高斯参数是通过最小化光度损失迭代优化的。在优化中，C被用球谐系数SH∈R15编码来解释方向不同的颜色变化
				- 协方差被分解成∑=RSSTRT，其中R是R3*3，S是R3*3，分别代表旋转和缩放，在梯度优化过程中保持协方差正半定性质。
		- 基于3D高斯的地图
		  collapsed:: true
			- 为了避免灾难性遗忘和过拟合，让建图在单个相机流场景上在计算上可行，我们把输入切分成块（子图）。每个子图覆盖几个关键帧，并且用一个独立的3D高斯点云表示。我们定义一个子图高斯点云Ps： ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202406141341491.png){:height 34, :width 307}
-
- ### Human
	- [[SMPL]]
-
- ### Semantic Mapping
	- [[Open-Fusion]]
	- [[Concept-Fusion]]
	- [[Concept-Graph]]
-
- ### Semantic Gaussian SLAM
-
- ### GS-SLAM
	- [[HI-SLAM2]]
	-
-
- 论文分析
	- [[hovsg]]
	- [[clio]]
	- [[ActiveGS]]
-
- 综述
	- [[3D Representation]]
	- [[Representation Ads&Dis]]
	- [[Gaussian Related Work]]
	- [[SLAM Related Work]]
	- [[Dataset]]
	- [[Scene Graph Generation]]
	- [[Semantic]]
	  id:: 67adbe47-00eb-4b00-818b-6c9fdf03fbbf
		- [[VM/VLM Related Work]]
		- [[Semantic NeRF]]
		- [[Semantic Gaussian]]
	- [[Sparse View Reconstructions Related Work]]
	- [[Feed-Forward]]
	-
	-
-
- TO BE CLASSIFIED：
- PLA: Language-Driven Open-Vocabulary 3D Scene Understanding 2023 CVPR
- RegionPLC: Regional Point-Language Contrastive Learning for Open-World 3D Scene Understanding
- CLIP-FO3D: Learning Free Open-world 3D Scene Representations from 2D Dense CLIP 2023 ICCV Workshop
- Towards Open Vocabulary Learning: A Survey 2024 TPAMI
- Mapping High-level Semantic Regions in Indoor Environments without Object Recognition 2024 ICRA
- Online Knowledge Integration for 3D Semantic Mapping: A Survey
-
-
-
-