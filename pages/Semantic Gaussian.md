- Semantic Gaussian相关工作(offline)：
	- LEGaussian: Language embedded 3d gaussians for open-vocabulary scene understanding. 2023 arxiv 引入不确定性和语义特征属性给每个高斯，渲染语义图并带有不确定性
	  :LOGBOOK:
	  CLOCK: [2025-03-10 Mon 11:30:50]--[2025-03-10 Mon 11:30:50] =>  00:00:00
	  :END:
	- LangSplat: 3D Language Gaussian Splatting 2024 CVPR 使用一个scene-wise自编码器学习语言特征在scene-specific latent space，使用一个2-stage策略进行语义特征提取
	- Feature 3dgs: Supercharging 3d gaussian splatting to enable distilled feature fields. 2024 CVPR distall SAM的encoder的feature到3D高斯，使用SAM的decoder分割2D渲染的feature map
	- 但是基于特征的方法训练成本高，存储要求高，推理时间慢
	  :LOGBOOK:
	  CLOCK: [2025-03-10 Mon 13:59:05]--[2025-03-10 Mon 13:59:06] =>  00:00:01
	  :END:
	- Opengaussian: Towards point-level 3d gaussian-based open vocabulary understanding. 2024 arxiv 使用 SAM mask来训练具有 3D 一致性的instance feature，提出了一种用于特征离散化的两阶段codebook和一种将 3D 点link到 2D mask和 CLIP 特征的instance level的 3D-2D 特征关联方法，然而使用固定的codebook条目数，并且没有充分利用 3D 空间的连续性，这限制了它的性能。
	- Semantic Gaussians: Open-Vocabulary Scene Understanding with 3D Gaussian Splatting(open-set, dynamic, 2024.03) [code](https://github.com/sharinka0715/semantic-gaussians)
	  :LOGBOOK:
	  CLOCK: [2025-03-10 Mon 11:27:53]--[2025-03-10 Mon 11:27:54] =>  00:00:01
	  :END:
	- Gaussian grouping: Segment and edit anything in 3d scenes. 2023 arxiv 对开放世界3D物体同时重建和分割，基于SAM的mask指引和3D空间一致性约束
	- [[3D Vision-Language Gaussian Splatting]] (ICLR2025) [openreview](https://openreview.net/forum?id=SSE9myD9SG)
	  :LOGBOOK:
	  CLOCK: [2025-03-10 Mon 11:23:24]--[2025-03-10 Mon 11:23:25] =>  00:00:01
	  :END:
	- [[GOI]]: Find 3D Gaussians of Interest with an Optimizable Open-vocabulary Semantic-space Hyperplane [code](https://github.com/Quyans/GOI-Hyperplane) 采用超平面划分来选择特征以改进对齐
	- SegmentAnyGauss: scale gate
	- CLIP-GS
	- Open-Ended 3D Metric-Semantic Representation Learning via Semantic-Embedded Gaussian Splatting openreview(), 质量差一点.但是和我们要解决的问题比较相关，作者ICLR2025 withdraw
	- SparseLGS: Sparse View Language Embedded Gaussian Splatting openreview(), 质量差一点，作者ICLR2025 withdraw
	- SeGS: Semantic-aware 3D Gaussian Splatting for Multi-turn Language-guided Robotic Grasping in Changeable Environment动态环境机器人抓取(利用高斯分裂)
	- GSemSplat: Generalizable Semantic 3D Gaussian Splatting from Uncalibrated Image Pairs
	-