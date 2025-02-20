- contribution
  id:: 67ac7abb-de6d-4cb7-aa2b-e30161d56988
	- 1，将语义特征嵌入高斯表征
	- 2，feature-level loss更新高斯表征
	- 3，基于语义的BA，多帧关联联合优化相机位姿和高斯表征
- 高斯语义如何表示
	- 16通道的语义特征嵌入e
	- 不随机初始化这些特征嵌入，而是将从图像中提取的二维语义特征作为三维高斯作为初始值，以实现语义高斯优化的更快收敛。
	- 使用通用特征提取器 Dinov2，然后使用预训练分类器构建分割网络
- Tracking
	- 还是基于轮廓，但是只使用rgb和depth损失
	- ![image.png](../assets/image_1739419846469_0.png){:height 47, :width 291}
- Mapping
	- Ls：交叉熵语义损失
	- Lf：特征损失，特征的直接L1，特征损失对中间特征提供了直接指导
	- ![Replaced by Image Uploader](https://raw.githubusercontent.com/Laura-Ting/blog-images/master/202502131211334.png){:height 50, :width 476}
- Semantic-informed Bundle Adjustment
	- 利用多视角语义的一致性来建立约束，使用估计的相对姿态变换T j i 将渲染的语义特征扭曲到其共可见帧j，并与帧j的渲染语义特征G（Tj，e）构建损失以获得LBA-sem： 其中G（Ti，e）表示使用相机姿态Ti从3D高斯G进行的splatted语义嵌入。
	- ![Replaced by Image Uploader](https://raw.githubusercontent.com/Laura-Ting/blog-images/master/202502131220288.png){:height 64, :width 350}
	- ![Replaced by Image Uploader](https://raw.githubusercontent.com/Laura-Ting/blog-images/master/202502131221515.png){:height 108, :width 345}
	- ![Replaced by Image Uploader](https://raw.githubusercontent.com/Laura-Ting/blog-images/master/202502131221940.png){:height 31, :width 350}
- ![Replaced by Image Uploader](https://raw.githubusercontent.com/Laura-Ting/blog-images/master/202502131224724.png){:height 309, :width 677}