#### Motication:

	- Open-set, Multimodal(text, image,audio)
#### Contribution:
- open-set multimodal 3D mapping,  query in a zero-shot manner
- compute pixel-aligned (local) features + image-level (global) feature, capture long-tailed concepts better
- UnCoCo RGBD dataset

#### Notation:
- Observation: $L={It(t∈{0...T})}$ It contains Ct+Dt
- query vectors: ![Replaced by Image Uploader](https://raw.githubusercontent.com/Laura-Ting/blog-images/master/202501091053063.png){:height 35, :width 68}, from multidimensional signals encoded by ![Replaced by Image Uploader](https://raw.githubusercontent.com/Laura-Ting/blog-images/master/202501091055723.png){:height 43, :width 37}

#### A. Fusing pixel-aligned foundation features to 3D
- ##### Map representation
	- a vertex position ![Replaced by Image Uploader](https://raw.githubusercontent.com/Laura-Ting/blog-images/master/202501091108537.png){:height 30, :width 51}
	- a normal vector ![Replaced by Image Uploader](https://raw.githubusercontent.com/Laura-Ting/blog-images/master/202501091109672.png){:height 31, :width 50}
	- a confidence count ![Replaced by Image Uploader](https://raw.githubusercontent.com/Laura-Ting/blog-images/master/202501091110890.png){:height 27, :width 46}
	- a 3D color vector (optional)
	- a concept vector ![Replaced by Image Uploader](https://raw.githubusercontent.com/Laura-Ting/blog-images/master/202501091110592.png){:height 33, :width 17}
- ##### Frame preprocessing
	- calculate vertex-normal maps (Vt, Nt), camera pose estimates Pt
	- the semantic context embedding of each pixel in Xt: ![Replaced by Image Uploader](https://raw.githubusercontent.com/Laura-Ting/blog-images/master/202501091116275.png){:height 32, :width 67}
- ##### Feature fusion
	- use camera pose Pt to project depth map and normal map into 3D
	- filter noisy depth values points using depth fusion, other points are fused into global map G
	- each pixel (u, v)t in Xt has a point pk in M
	- feature is accumulated using weighted average, the weight(confidence count) is determined by the distance γ from current pixel to the camera center(radial distance) and a fixed scaling term σ.
	- ==d.h. the nearer pixel to camera is, the higher confidence score it can get==
	- ![Replaced by Image Uploader](https://raw.githubusercontent.com/Laura-Ting/blog-images/master/202501091135227.png){:height 36, :width 103}
	- ![Replaced by Image Uploader](https://raw.githubusercontent.com/Laura-Ting/blog-images/master/202501091134011.png){:height 85, :width 171}

#### B. Computing pixel-aligned features
- ##### Overview: global fG, pixel-aligned fP, local fL
	- fG is the embedding of the entire image
- ##### Local embeddings
	- a segmentation model to obtain bi = bbox(ri), calculate embedding fLi = F(bi).
- ##### Fusing local and global features
	- a weighted combination of fG and fL
	- local-global
		- ![Replaced by Image Uploader](https://raw.githubusercontent.com/Laura-Ting/blog-images/master/202501091157884.png){:height 48, :width 203}
	- local-local
		- cos sim between all pairs of local embeddings: ![Replaced by Image Uploader](https://raw.githubusercontent.com/Laura-Ting/blog-images/master/202501091158543.png){:height 29, :width 99}
		- calculate the average: ![Replaced by Image Uploader](https://raw.githubusercontent.com/Laura-Ting/blog-images/master/202501091200032.png){:height 54, :width 102}, indicate uniqueness. ==d.h. the higher it is, the less unique it is, other regions all have the similar pattern==
	- fuse the feature
		- mixing weight wi ∈ [0, 1], with a temperature τ=1 default
		- ![Replaced by Image Uploader](https://raw.githubusercontent.com/Laura-Ting/blog-images/master/202501091203261.png){:height 89, :width 193}
		- pixel-aligned feature: ![Replaced by Image Uploader](https://raw.githubusercontent.com/Laura-Ting/blog-images/master/202501091206409.png){:height 31, :width 165}, normalized and mapped to the pixels (u, v) in ri
		- d.h. when global features dominate more?
		  background-color:: yellow
			- current local is similar to global(larger pixel area with consistent visual feature?)
			  background-color:: yellow
			- exist similar patterns between locals(the whole image's style, material, color, repeated patterns?)
			  background-color:: yellow
		- when a fL is predicted error, and fG is predicted correct. their similarities become low, and the fG can not correct the false fL
		  background-color:: blue
		- the problems of belonging to multiple regions
		  background-color:: blue
		- allow each pixel to belong to multiple regions, fP(u,v) is normalized once it accumulates features fPi from regions ri
- ##### Capturing long-tailed concepts
	- capture fine-grained and long-tailed concepts better
	- LSeg and OpenSeg need to be finetuned on datasets with limited concepts, in order to obtain the ability of segmentation, but this  harms their zero-shot ability to generalize to long-tailed and fine-grained concepts

#### C. Multimodal querying over 3D feature-fused maps
- text, image, click, audio query

#### D. Building complex 3D spatial query modules
- 3D spatial comparator (3DSC): input relation signature RELATION(QUERYa, QUERYb) and output a scalar or boolean value
- HOWFAR(qa, qb)
- ISTOTHERIGHT(qa, qb), ISTOTHELEFT(qa, qb), ONTOPOF(qa, qb), UNDER(qa, qb) return TRUE or FALSE
- the language queries are directly fed into the CLIP text encoder without any preprocessing
- p.s. when using SAM, not calculate uniqueness term
-