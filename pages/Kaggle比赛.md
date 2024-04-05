- 赛事介绍
  collapsed:: true
	- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202403091619663.png)
	- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202403091621258.png)
- 特征工程
  collapsed:: true
	- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202403091636008.png)
	- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202403091637848.png)
	- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202403091639628.png)
- 结构化
  collapsed:: true
	- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202403091647550.png){:height 485, :width 826}
- 模型选择
  collapsed:: true
	- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202403091649586.png)
	- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202403091653857.png)
	- 这三个库多核，多机，分布式，比普通的sklearn要好；树模型也是可以使用GPU的，会快7-8倍
	- 调参：网格搜索，随机搜索，贝叶斯搜索...
	- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202403091657925.png)
	- 深度学习需要自定义模型
		- 实例化模型，定义损失函数，定义优化器，定义学习率
	- 不是所有的模型都需要做缺失值填充，那三种高阶的模型就不用
	- 数据集划分，训练集和验证集；定义两个调参的函数，调参用optuna（可以对超参数预估）；用得到的超参数初始化现在的模型
- 机器学习
	- 有监督：在工业界常用
		- 连续型
		  collapsed:: true
			- 回归
			- 基于树：决策树，随机森林
		- 分类型
		  collapsed:: true
			- KNN：目前用的比较少
			- Trees：一下几个工业界比较多
			- Logical Regression：逻辑回归
			- Naive-Bayes：朴素贝叶斯，简单，但是数据足够好时效果比较好
			- SVM：中小型数据集上效果好
	- 无监督
		- 连续型
			- SVD，PCA用来降维（尽量让每个维度没有那么大，关联性削弱）
			- K-means：一般来说做辅助，先聚类，然后看一类的特征，之后用于有监督训练
		- 离散型
			- Association analysis关联规则：Apriori，FP-Growth频繁模式树
			- Hidden Markov Model：一般不用了，用循环神经网络代替
-
- 夏老师您好，我是2021级软件工程的李婷，现在跟您上人工智能概论这门课，我目前学分绩89，之后挺想从事机器学习和深度学习这个方向的，但是感觉自己学的比较浅，没有深入了解和具体的方向，平常听您讲课觉得您对学生很负责，而且会让学生理解和思考算法模型，挺想跟您学习和做一些更多的研究的，想问下您现在还收人嘛
-