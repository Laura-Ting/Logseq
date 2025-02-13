- GS-SLAM相关工作：
	- Splatam: Splat, track & map 3d gaussians for dense rgb-d slam.
	- Gaussian splatting slam. 2024 CVPR
	- Gaussian-slam: Photo-realistic dense slam with gaussian splatting. 2023 arxiv
	- Gs-slam: Dense visual slam with 3d gaussian splatting. 2023 arxiv
	- Compact 3d gaussian splatting for dense visual slam. 2024 arxiv
	- Photo-slam: Real-time simultaneous localization and photorealistic mapping for monocular, stereo, and rgb-d cameras. 2023 arxiv
	- Structure gaussian slam with manhattan world hypothesis. 2024 arxiv
- Semantic GS-SLAM相关工作(online)：
	- [[GS3LAM]]: Gaussian Semantic Splatting SLAM(ACM MM2024, 2024.06) [code](https://github.com/lif314/GS3LAM)
	  collapsed:: true
		- {{embed ((67ac0adc-6f5d-48c0-bca0-a67eb38ee10c))}}
	- [[SemGauss-SLAM]]: Dense Semantic Gaussian Splatting SLAM(2024.03) no code
	  collapsed:: true
		- {{embed ((67ac7abb-de6d-4cb7-aa2b-e30161d56988))}}
	- [[SGS-SLAM]]: Semantic Gaussian Splatting For Neural Dense SLAM( ECCV 2024) [code](https://github.com/ShuhongLL/SGS-SLAM)
	  collapsed:: true
		- {{embed ((67ab5d0d-a556-4d52-b5b6-a70e3328e197))}}
	- GaussNav: Gaussian Splatting for Visual Navigation TPAMI
- Semantic
  id:: 67ac0512-66fd-4468-8c83-1d7bee431f36
	- O2V: Open Vocabulary Mapping
	- LanguageGroundedSemseg
- Dynamic
	- Dynamic 3d gaussians: Tracking by persistent dynamic view synthesis. 2024 3DV
-
- Segmentation
	- Gaussian Segmentation
		- SAGA: Segment any 3d gaussians. 2023 arxiv 用SAM产生的mask进行对比学习，将SAM的feature映射到低维空间通过一个可训练的MLP，并复制这些特征以解决不一致问题
		- Click-gaussian: Interactive segmentation to any 3d gaussians. 2024 ECCV 使用一个2-level的粒度的特征场，结合Global Feature-guided Learning(GFL)去解决夸视角不一致性问题
		- InstanceGaussian
	- oterhs
		- Open3DIS: Open-vocabulary 3d instance segmentation with 2d mask guidance. 2024 CVPR
		- Sai3d: Segment any instance in 3d scenes. 2024 CVPR
		- 前两篇通过SAM的mask融合superpoints(Efficient graph-based image segmentation. 2004 IJCV)
		- Maskclustering: View consensus based mask graph clustering for open-vocabulary 3d instance segmentation. 2024 CVPR 引入一个mask graph culster方法基于视图一致率，针对开放词汇 3D 实例分割
		- EmbodiedSAM: Online Segment Any 3D Thing in Real Time
-