- traditional sparse mapping using point clouds and voxels
  collapsed:: true
	- Monoslam: Real-time single camera slam. 2007
	- Orb-slam: a versatile and accurate monocular slam system. 2015
	- Dtam: Dense tracking and mapping in real-time. 2021 ICCV
	- SLAM++: Simultaneous localisation and mapping at the level of objects. 2013 CVPR mesh-based建模世界为一个graph，每个节点有估计位姿并用mesh表示3D物体
	- Ovd-slam: An online visual slam for dynamic environments. 2023 Senior Journal
	- Vins-mono: A robust and versatile monocular visualinertial state estimator. 2018 TOR
	- 有限的语义理解不能准确理解场景：
		- Dense 3d semantic mapping of indoor scenes from rgb-d images. 2014 ICRA voxel
		- Panopticfusion: Online volumetric semantic mapping at the level of stuff and things. 2019 IROS pcd, sdf
	- point/surfel
		- ORB-SLAM2: An Open-Source SLAM System for Monocular, Stereo, and RGB-D Cameras. 2017 TOR
		- Real-time Scalable Dense Surfel Mapping. 2019 ICRA
		- Elasticfusion: Dense slam without a pose graph. 2015 RSS
	- grids
		- KinectFusion: Real-time Dense Surface Mapping and Tracking. 2011 ISMAR
	- voxel
		- Hierarchical Voxel Block Hashing for Efficient Integration of Depth Images. 2016 RAL
		- Efficient Online Surface Correction for Real-time Large-Scale 3D Reconstruction 2017 arxiv
		- Real-time 3D Reconstruction at Scale Using Voxel Hashing. 2013 ToG
	- 传统表征不能预测未知区域，同时受表征有限的分辨率限制
- learning-based SLAM
  collapsed:: true
	- Codeslam—learning a compact, optimisable representation for dense visual slam. 2018 CVPR
	- Nodeslam: Neural object descriptors for multiview shape reconstruction. 2020 3DV
- NeRF-base SLAM
  collapsed:: true
	- implicit(MLP-based)
		- imap: Implicit mapping and positioning in real-time. 2021 ICCV 第一个使用Neural Radiance进行tracking和mapping，但是单个MLP受限于大场景
	- hybrid
		- Nice-slam: Neural implicit scalable encoding for slam. 2022 CVPR 使用预训练多个MLP层次化场景表示，分层多特征grids
		- Co-slam: Joint coordinate and sparse parametric encodings for neural real-time slam. 2023 CVPR 基于pixel的keyframe追踪基于one-blob encoding，采用多分辨率hash grids
		- Vox-fusion: Dense tracking and mapping with voxel-based neural implicit representation. 2022 ISMAR 使用八叉树做动态地图扩展
		- Point-slam: Dense neural point cloud-based slam. 2023 ICCV 使用神经点云体渲染
		- Eslam: Efficient dense slam system based on hybrid representation of signed distance fields. 2023 CVPR 使用tri-plane体渲染，增强mapping能力
	- Plgslam: Progressive neural scene represenation with local to global bundle adjustment. 2024 ICCV
	- vmap: Vectorised object mapping for neural field slam. 2023 CVPR
	- Go-slam: Global optimization for consistent 3d instant reconstruction. 2023 ICCV 使用Droid-SLAM作为tracking，多尺度hash encoding(InstantNGP)作为mapping
	- Droid-slam: Deep visual slam for monocular, stereo, and rgbd cameras. 2021 NIPS
	- InstantNGP: Instant neural graphics primitives with a multiresolution hash encoding. 2022 TOG
	- Plvs: A slam system with points, lines, volumetric mapping, and 3d incremental segmentation. arxiv 2023 使用3D先验
- Neural Implicit Semantic SLAM
  collapsed:: true
	- DNS-slam: Dense neural semanticinformed slam. 用一个2D语义prior提供多视图几何约束但是没有用semantic优化3D重建。额外的MLP channel剑麻和解码语义label的同时优化相机位姿和语义场景。
	- SNI-slam: Semantic neural implicit slam. 2023 arxiv 前两者也提供了semantic loss给几何监督但是受限于NeRF的体渲染
	- Orbeez-slam: A real-time monocular visual slam with orb features and nerf-realized mapping 2023 ICRA
	- Incremental joint learning of depth, pose and implicit scene representation on monocular camera in large-scale scenes. 2024 arxiv
	- Neslam: Neural implicit mapping and self-supervised feature tracking with depth completion and denoising. 2024 arxiv
	- Long-term visual simultaneous localization and mapping: Using a bayesian persistence filter-based global map prediction. 2023 RAM
	- Ddn-slam: Real-time dense dynamic neural implicit slam with joint semantic encoding. 2024 arxiv
	- End-to-end rgb-d slam with multi-mlps dense neural implicit representations. 2023 RAL
	- Mod-slam: Monocular dense mapping for unbounded 3d scene reconstruction. 2024 arxiv
	- NIDS-SLAM: Neural implicit dense semantic slam. 2023 arxiv 使用Obr-SLAM3做tracking，Instant-NGP做mapping但是没有共同优化语义
	- Neural implicit dense semantic slam. 2023 arxiv
- Real-time dense semantic SLAM
  collapsed:: true
	- Kimera: an open-source library for real-time metric-semantic localization and mapping. 2020 ICRA 在mesh的face上标注语义，允许对3D mesh的寓意度量重建
	- Slam++(traditional)
	- Codeslam(learning-based)
	- SemanticFusion: Dense 3D Semantic Mapping with Convolutional Neural Networks. 基于surfel，基于ElasticFusion，使用CNN预测像素类别，贝叶斯更新去跟踪每个surfel的类别概率分布，建立一个全局一致的语义图
	- Semantic-direct visual odometry. 2022 RAL
	- Vso: Visual semantic odometry. 2018 ECCV
- Voxel？
  collapsed:: true
	- Fusion++: Volumetric object-level slam. 2018 3DV
- Semantic SLAM
  collapsed:: true
	- points/surfels
		- Multi-resolution Surfel Maps for Efficient Dense 3D Modeling and Tracking. JVCIR
	- Unsupervised continual semantic adaptation through neural rendering. 2023 CVPR 传统稠密语义SLAM不能预测未知区域
	- Language-EXtended Indoor SLAM (LEXIS): A Versatile System for Real-time Visual Scene Understanding arxiv 2024 目前感觉貌似什么都没中
-
- 自动驾驶
  collapsed:: true
	- Sfgan: Unsupervised generative adversarial learning of 3d scene flow from the 3d scene self. 2022 AIS
	- 3d scene flow estimation on pseudo-lidar: Bridging the gap on estimating point motion. 2022 IEEE TII
	- 3dsflabelling: Boosting 3d scene flow estimation by pseudo auto-labelling. 2024 arxiv
	- Ffpa-net: Efficient feature fusion with projection awareness for 3d object detection. 2022 arxiv
	- Rogs: Large scale road surface reconstruction based on 2d gaussian splatting. 2024 arxiv