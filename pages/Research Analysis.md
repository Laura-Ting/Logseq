- GiftedNav corner case -- my writing
	- v0
	  collapsed:: true
		- 在三维重建中，遮挡和局部观测是从二维观察三维时天然存在的问题。多个视角的观测能够帮助我们更好地确定物体的语义。目前一个流行的解决范式是通过分割模型得到每一帧类别无关的分割区域再用Foundation Model获取对应区域的特征，再将帧间特征进行融合。但是这种方法论存在问题，依赖于分割结果的粒度和获取到的特征的准确性。
		- 首先，这种方法论缺乏有效的局部与全局上下文信息整合机制，导致模型无法充分利用不同尺度下的信息，即使是在理想情况下当分割模型粒度恰好，刚刚好能够分出一个物体时。有些方法通过根据局部特征(基于mask内像素提取)和全局特征(基于整张图像提取)的相似度加权local和global信息来缓解这个问题。但是由于这两种特征是通过独立路径生成的，而不是在同一空间中协同学习的，仅仅通过加权机制进行特征拼接会导致局部和全局之间的联系较为松散，无法实现真正的上下文感知，依然无法处理特定类别。尤其是当local预测错误时global的权重很低而无法拉回错误的local。比如cabinet会因为外面那层黑色条状条纹而被识别为air vent根据局部信息无法判断。这就导致后续将每一帧上的2D图像域特征融合到3D时关联了很多错的特征。
		- 其次，目前的分割模型存在强烈的过分割现象，而这是因为图像域类别无关的分割受到很多因素的影响，比如复杂的纹理，几何形状，光照等等。而过分割导致的问题就是被识别成多个非常不相关的其他物体而不是一个完整的正确的物体，损害了一个物体的语义一致性。比如在图1中的这个黑色sofa被分割成很多bag，jacket，blanket，pillow。再比如图2中的sofa被分割为tissue，chair和column。比如图3中的wardrobe被识别成blinds，air vent，door。而分割错误会直接影响后续特征提取和融合的质量。而且，过分割导致的局部信息碎片化直接加剧了上下文信息缺失的问题。过多的错误分割区域使得在整合多视角信息时，系统难以形成对物体整体结构和位置的准确理解。比如当看到mirror时，会识别成mirror里面的door frame。
		- 而我们的方法中使用YOLO-World，其方法论在设计上直接避免局部和全局信息割裂的问题，也不会出现过分割现象。YOLOWorld通过多尺度特征提取，将局部和全局信息作为一个整体来建模。在这种设计中，局部特征不再是独立提取的，而是从不同尺度的特征中共享上下文信息。特征图的低分辨率部分（大感受野）提供了全局上下文信息，如场景的大致布局和物体的整体位置；高分辨率部分（小感受野）保留了局部的细节信息，如边缘、纹理等。多尺度特征的联合建模使得局部细节在被建模时已经考虑到了全局语义约束，局部和全局在同一特征空间中具有语义一致性。
	- v1
	  collapsed:: true
		- 在语义导航任务中，系统需要具备准确的空间感知能力，以识别场景中的物体及其相对位置，从而生成高质量的语义地图。然而，由于单视角观测受到遮挡和局部视野限制，系统常难以完整捕获物体的语义属性或结构，因此多视角观测成为一种解决方案。一个流行的范式是：首先通过类别无关的分割模型将每一帧图像划分为多个区域，再利用Foundation Model提取这些分割区域的特征，最后通过帧间特征融合生成三维的语义表示。然而，这种方法论仍存在问题，它的效果高度依赖于分割模型的粒度提取特征的准确性。
		- 首先，这种方法论缺乏有效的局部与全局上下文信息整合机制，导致模型无法充分利用不同尺度下的信息，即使是在理想情况下当分割模型粒度恰好，刚刚好能够分出一个物体时。有些方法通过根据局部特征(基于mask内像素提取)和全局特征(基于整张图像提取)的相似度加权local和global信息来缓解这个问题。但是由于这两种特征是通过独立路径生成的，而不是在同一空间中协同学习的，仅仅通过加权机制进行特征拼接会导致局部和全局之间的联系较为松散，无法实现真正的上下文感知，依然无法处理特定类别。尤其是当局部特征预测错误时全局特征的权重很低而无法纠正错误的特征。比如cabinet会因为外面那层黑色条状条纹而被识别为air vent根据局部信息无法判断。进一步地，这种误判在多帧2D图像域特征融合到3D时会导致特征关联错误，从而影响地图语义的最终结果。
		- 其次，目前的分割模型存在显著的过分割现象。由于图像域类别无关的分割模型受到纹理复杂性、几何形状和光照等多种因素的影响，往往会将单一物体划分为多个碎片化区域。这种现象会破坏物体的语义一致性，使其被误识别为多个不相关的其他物体。例如，图1中黑色sofa可能被分割为bag、jacket、blanket和pillow等碎片区域。图2中的sofa被分割为tissue，chair和column。比如图3中的wardrobe被识别成blinds，air vent，door。而分割错误会直接影响后续特征提取和融合的质量。而且，过分割导致的局部信息碎片化直接加剧了上下文信息缺失的问题。过多的错误分割区域使得在整合多视角信息时，系统难以形成对物体整体结构和位置的准确理解。比如当看到mirror时，会识别成mirror里面的door frame。损害了语义特征的准确性。
		- 相反，我们的方法使用YOLO-World，从设计上直接避免了局部和全局信息割裂的问题，并且有效解决了过分割现象。YOLOWorld通过多尺度特征提取，将局部和全局信息在同一框架中进行建模。特征图的低分辨率部分（大感受野）捕捉全局上下文信息，例如场景的大致布局和物体的整体位置；高分辨率部分（小感受野）保留局部的细节信息，例如边缘、纹理等。通过多尺度特征的联合建模，局部特征生成时已经受到全局语义约束，确保局部和全局特征在同一特征空间中保持语义一致性。此外，YOLOWorld在直接从整张图像生成目标特征时，通过整体语义建模避免了依赖碎片化的分割区域，从而从根本上杜绝了过分割问题。
	- v1-en
	  collapsed:: true
		- In semantic navigation tasks, systems require accurate spatial perception to identify objects in the scene and their relative positions, enabling the generation of high-quality semantic maps. However, single-view observations are inherently ==limited by occlusion and partial observation==, making it difficult for the system to fully capture the semantic attributes or geometry of objects. To address this, multi-view observations have become a common solution. One popular paradigm involves using a ==class-agnostic segmentation model== to divide each image frame into multiple regions, extracting semantic features of these segmented regions with a visual language foundation model, and subsequently fusing the inter-frame features to produce 3D semantic representations. However, this paradigm still faces challenges, as its effectiveness heavily depends on the ==granularity of the segmentation model and the accuracy of the extracted features.== [[#red]]==这里表述有问题，the accuracy of the extracted features相当于什么都没说==
		- First and foremost, this methodology lacks an effective mechanism to ==integrate local and global contextual information==. This limitation persists even in ideal cases where the segmentation model achieves perfect granularity, just enough to isolate an entire object. Some methods attempt to address this by ==weighting== local (extracted from mask-based pixels) and global (extracted from the entire image) features ~~based on their semantic similarity~~. However, since these two features are generated through independent pathways rather than [[#red]]==being learned in a shared space==, simply fusing them via weighting results in loose connections between them, failing to [[#red]]==achieve genuine context awareness==.  Especially when local predictions are incorrect, global feature weights tend to remain low due to low similarity to correct false local predictions. For example, a cabinet in Figure 5 might be misclassified as an air vent due to the black striped patterns on its surface, and a mirror in Figure 4 might be misinterpreted as the door frame reflected within it, as this way of combining global information cannot resolve this ambiguity. Furthermore, such misclassifications can propagate during the fusion of multi-frame 2D features into 3D, resulting in incorrect feature associations and degrading the semantic quality of the map.
		- Additionally, current segmentation models often suffer from ==significant over-segmentation==. Class-agnostic segmentation in the image domain is highly influenced by factors such as ==complex textures, geometric shapes, and lighting conditions,== which frequently cause a single object to be fragmented into multiple unrelated regions, undermining the semantic consistency of the object. For instance, as illustrated in Figure 1, a black sofa might be segmented into fragments ==labeled as bag, jacket, blanket, and pillow==. Similarly, in Figure 2, ==a sofa might be segmented into tissue, chair, and column==, while in Figure 3, a wardrobe could be misidentified as blinds, air vent, and door. Such segmentation errors directly affect the quality of subsequent feature extraction and fusion. Moreover, the fragmentation of local information caused by over-segmentation exacerbates the loss of contextual information. The abundance of erroneous segmented regions makes it challenging for the system to form an accurate understanding of the overall structure and position of objects when integrating multi-view information.
		- In contrast, our method leverages YOLO-World, which inherently avoids the issue of weak connection between local and global information while effectively addressing over-segmentation. YOLO-World ==integrates local and global information== within a unified framework through [[#blue]]==multi-scale feature extraction 这里描述可以再清楚具体一些==. The low-resolution parts of the feature map (with a large receptive field) capture global contextual information, such as the general layout of the scene and the overall position of objects, while the high-resolution parts (with a small receptive field) preserve local details, such as edges and textures. Through the joint modeling of multi-scale features, local features are generated with global semantic constraints already in place, ensuring semantic consistency between local and global features within the same feature space. Additionally, YOLO-World directly generates target features from the entire image while avoiding reliance on fragmented segmentation regions through holistic semantic modeling, thereby fundamentally eliminating over-segmentation issues.
	- mirror Fig4
	  collapsed:: true
		- ![Replaced by Image Uploader](https://raw.githubusercontent.com/Laura-Ting/blog-images/master/202501101629938.png){:height 210, :width 458}
		- ![Replaced by Image Uploader](https://raw.githubusercontent.com/Laura-Ting/blog-images/master/202501101629894.png){:height 264, :width 362}
	- air vent, caninet Fig 5
	  collapsed:: true
		- ![Replaced by Image Uploader](https://raw.githubusercontent.com/Laura-Ting/blog-images/master/202501101630103.png){:height 197, :width 386}
		- ![Replaced by Image Uploader](https://raw.githubusercontent.com/Laura-Ting/blog-images/master/202501101631540.png){:height 265, :width 384}
	- black sofa Fig1
	  collapsed:: true
		- ![Replaced by Image Uploader](https://raw.githubusercontent.com/Laura-Ting/blog-images/master/202501101631735.png){:height 240, :width 498}
		- ![Replaced by Image Uploader](https://raw.githubusercontent.com/Laura-Ting/blog-images/master/202501101632660.png){:height 341, :width 488}
	- wardobe Fig3
	  collapsed:: true
		- ![Replaced by Image Uploader](https://raw.githubusercontent.com/Laura-Ting/blog-images/master/202501101632003.png){:height 263, :width 566}
		- ![Replaced by Image Uploader](https://raw.githubusercontent.com/Laura-Ting/blog-images/master/202501101633795.png){:height 366, :width 548}
	- sofa Fig2
	  collapsed:: true
		- ![Replaced by Image Uploader](https://raw.githubusercontent.com/Laura-Ting/blog-images/master/202501101633778.png){:height 308, :width 585}
		- ![Replaced by Image Uploader](https://raw.githubusercontent.com/Laura-Ting/blog-images/master/202501101633195.png){:height 444, :width 557}
	- v2-en
	  collapsed:: true
		- In semantic navigation tasks, systems require accurate spatial perception to identify objects in the scene and their relative positions, enabling the generation of high-quality semantic maps. However, the ability to infer semantic attributes  of objects from a single view is inherently limited by occlusion and partial observation. To address this, multi-view observations have become a common solution. One popular paradigm involves using a class-agnostic segmentation model to divide each image frame into multiple regions, extracting semantic features of these segmented regions with a visual language foundation model, and subsequently fusing the inter-frame features to produce 3D semantic representations. However, this paradigm still faces challenges, as its effectiveness heavily depends on the granularity of the segmentation model and the inherent lack of contextual information within segmented regions.
		- First and foremost, this methodology lacks an effective mechanism to ==integrate local and global contextual information==. This limitation persists even in ideal cases where the segmentation model achieves perfect granularity, just enough to isolate an entire object. Some methods attempt to address this by ==weighting== local (extracted from mask-based pixels) and global (extracted from the entire image) features, but they fail to achieve [[#blue]]==context awareness==.  Especially when local predictions are incorrect, global feature weights tend to remain too low to correct false local predictions. For example, the mirror in Figure 4 might be misinterpreted as the door frame reflected within it, as this way of combining global information cannot integrate contextual information effectively. Furthermore, such misclassifications can propagate during the fusion of multi-frame 2D features into 3D, resulting in incorrect feature associations and degrading the semantic quality of the map.
		- Additionally, current segmentation models often suffer from ==significant over-segmentation==. Class-agnostic segmentation in the image domain is highly influenced by factors such as ==complex textures and lighting conditions,== which frequently cause a single object to be fragmented into multiple regions. This over-segmentation exacerbates the loss of contextual information, as these fragments are frequently misidentified as unrelated regions, undermining the semantic consistency of the object. For instance, as illustrated in Figure 1, the black sofa might be segmented into regions with features corresponding to a bag, jacket, blanket, and pillow. The abundance of erroneous segmented regions makes it challenging for the system to form an accurate understanding of the overall structure and position of objects when integrating multi-view information.
		- In contrast, our method leverages YOLO-World, which inherently provides contextual information while effectively addressing over-segmentation. YOLO-World ==integrates local and global information== within a unified framework through [[#blue]]==multi-scale feature extraction==. The low-resolution parts of the feature map (with a large receptive field) capture global contextual information, such as the general layout of the scene and the overall position of objects, while the high-resolution parts (with a small receptive field) preserve local details, such as edges and textures. Through the joint modeling of multi-scale features, local features are generated with global semantic constraints already in place, ensuring semantic consistency between local and global features within the same feature space. Additionally, YOLO-World directly generates target features from the entire image while avoiding reliance on fragmented segmentation regions through holistic semantic modeling, thereby fundamentally eliminating over-segmentation issues.
	- local-global问题：mirror，cabinet(air vent)
	- 过分割问题：sofa被分割成很多bag，sofa过分割成tissue，wardrobe被分割成很多blinds
	- 多视角不一致性
-
- GiftedNav当前文章结构
	- Intro
		- focus在构建queryable地图上，但是问题是closed-set
		- open-vocabulary进展，dense-feature field-based(基于backprojection，differentiable rendering)和scene graph-based(在内存中保留所有点云)
		- 这些方法应用于离线构建，而不是在线。离线方法不能处理丢帧(还有什么)？。task-oriented。->提出open-vocabulary, online, and task-oriented map representation
		- key challenges：
			- perceptual inconsistency
			- Ensure memory efficiency
			- Achieve fast convergence
			- Require active exploration
-
-
-
-
- Architecture不同设定 -- 希望继续研究residual结构作用
  collapsed:: true
	- ![Replaced by Image Uploader](https://raw.githubusercontent.com/Laura-Ting/blog-images/master/202501140029020.png)
	- embed_full: 1 + 2
	- embed_pos: 2
	- geo_pos: 3 + 2
	- color_pos: 2 + 4
-
- Gaussian GPU并行处理
  collapsed:: true
	- ![55d50747662b8cf335aac73909eddb9.png](../assets/55d50747662b8cf335aac73909eddb9_1737097490255_0.png){:height 827, :width 633}
	- 希望通过images和poses给整个场景的高斯上语义特征
	- 1，images和poses分组，分组个数和第一组GPU数量N相同，多组可以实现并行，每一组内串行处理SAM分割mask，计算每个mask的CLIP embedding，每一组输入整个高斯场景用于做特定poses下的rasterization得到二维渲染图片，根据SAM分割结果可以知道是哪些区域被分割，也就是可以知道三维高斯场景的哪些区域被分割的parameter
	- 2，将上面步骤得到的不同GPU上的parameter信息和自己对应的embedding信息写入同一共享内存后重新分组，分组数量与第二组GPU数量M相同。只需要聚合这些高斯的parameter和它对应特征进行整合，归一化，可以得到整合后的segment-wise embedding。(相当于data association吧，只不过全程都是全局Gaussian parameter)
	- 3，可以知道这些parameter对应的高斯子区域，并赋给它对应的embedding
	- 4，这些高斯在NoSQL DB中按照空间位置坐标进行索引，就是使用三维空间中的坐标（如世界坐标）作为键，对数据进行排序，按区域存储数据，可以快速找到自己对应的语义向量所对应的分区
	-
	- 感觉比较有优势的点就是，
	- 1，可以并行来搞的原因就是不同区域的高斯是局部的，互不影响的，比如一个房间和另一个房间，一个房间左侧的物体和右侧的物体这种。
	- 2，最后存储embedding的粒度不是在每个高斯上，而是segment或者region(类似于node那种稀疏的)，efficient，或许还可调？
	- 3，一直是对一个全局Gaussian scene进行局部操作，而不是把局部通过复杂的关联整合起来，或者最近邻再去找。这样整合的时候可以处理边界的划分的问题。就是感觉用到了高斯这个渲染和可以逆着找回去的过程。
	- 4，如果按照物理顺序存储感觉可能也有优点，感觉和局部性原理有点关系。在一开始的images/poses subset划分这一块，如果不故意打乱顺序，就使用拍摄顺序的话，在第一步完成之后顺序写入第二步的storage bucket or shared memory如果也按顺序的话，实际上得到的就是这个局部的embedding，空间上邻近的点通常具有语义相关性（例如，属于同一个物体或区域），按顺序存储可以简化聚合操作。而且按顺序存储也可以减少GPU在处理共享存储时的随机访问开销。
	-
	- 如果物理位置临近的点存储在一起，后续的处理（如聚合、查询或三维建图）可以更高效地访问这些数据。
	- 在三维语义建图任务中，空间上邻近的点通常具有语义相关性（例如，属于同一个物体或区域），按顺序存储可以简化聚合操作。
	- 按顺序存储可以减少GPU在处理共享存储时的随机访问开销，提高数据访问的带宽利用率。
-
-