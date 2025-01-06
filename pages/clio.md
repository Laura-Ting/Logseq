- 按照代码逻辑
- 计算recall和iou：gt中有多少是est预测对的
	- 传入参数：`est_objects, gt_data, clip_model, visualize`
	- task_list: gt任务列表(里面只有任务因为取的是key), task_feature
		- ```
		      task_list = list(gt_data.task_objects.keys()) # ["get the book", "get the red pen"...]
		      task_features = cliphandler.get_text_clip_features(task_list)
		  ```
	- 初始化total_gt_objects，total_strict_matches，total_weak_matches，sum_iou，est_boxes
		- ```
		      total_gt_objects = 0
		      total_strict_matches = 0
		      total_weak_matches = 0
		      sum_iou = 0
		      est_boxes = []
		  ```
	- 遍历每个task获取对应的gt_bboxes, 找到和gt对应的k个rel_objects，get_k_relevant_objects是根据当前任务特征用cos sim来找est_objects中前k个相关物体的，统计gt_boxes总数
		- ```
		      for i in range(len(task_list)):
		          gt_bboxes = gt_data.task_objects[task_list[i]].copy()
		          num_objs = len(gt_bboxes)
		          rel_objects = eval_utils.get_k_relevant_objects(
		              est_objects, task_features[i], num_objs)
		          total_gt_objects += len(gt_bboxes)
		  ```
	- 计算最佳匹配iou，拿着每一个gt去和每一个rel_objects中的est匹配，存储最好的best_gt_est_pair(gt_id, est_id)，获取到最好的best_gt_bbox和best_est_obj
		- ```
		          while len(gt_bboxes) > 0 and len(rel_objects) > 0:
		              best_iou = 0
		              best_gt_est_pair = None
		              for gt_id, gt_bbox in enumerate(gt_bboxes):
		                  for est_id, est_obj in enumerate(rel_objects):
		                      iou = est_obj.compute_iou(gt_bbox)
		                      if iou > best_iou:
		                          best_iou = iou
		                          best_gt_est_pair = (gt_id, est_id)
		              if best_iou == 0:
		                  break
		              sum_iou += best_iou
		              best_gt_bbox = gt_bboxes[best_gt_est_pair[0]]
		              best_est_obj = rel_objects[best_gt_est_pair[1]]
		  ```
	- 计算最好的匹配的strict_match和weak_match, recall是match个数/gt总数，iou是平均IoU
		- ```
		              if best_est_obj.contains_centroid(best_gt_bbox):
		                  total_weak_matches += 1
		                  if best_est_obj.centroid_contained_in(best_gt_bbox):
		                      total_strict_matches += 1
		              # delete
		              del gt_bboxes[best_gt_est_pair[0]]
		              del rel_objects[best_gt_est_pair[1]]
		      return total_weak_matches / total_gt_objects, total_strict_matches / total_gt_objects, sum_iou / total_gt_objects
		  ```
- 计算precision：est中有多少是gt
	- 传入参数: `est_objects, gt_data, clip_model, min_sim_ratio, visualize`
	- 获取任务列表task_list，初始化est_task_objects字典(key为task，value为list的obj)，初始化max_sims字典(key为task，value为最大cos sim)，计算任务特征task_features
		- ```
		      # first sort all est objects into the different tasks
		      task_list = list(gt_data.task_objects.keys())
		      est_task_objects = {task: [] for task in task_list}
		      max_sims = {task: 0 for task in task_list}
		      task_features = cliphandler.get_text_clip_features(task_list)
		  ```
	- 初始化total_est_objects，total_strict_matches，total_weak_matches
	  collapsed:: true
		- ```
		      total_est_objects = 0
		      total_strict_matches = 0
		      total_weak_matches = 0
		  ```
	- 将est_objects分配到最相关的任务：遍历est_objects, 每一个est_obj和所有task_features计算cos sim，找到最大的cos sim对应的task_id, 加入到est_task_objects对应key为task的value中，max_sims只存储改任务下最大的sim
		- ```
		      for est_obj in est_objects:
		          sims = helpers.compute_sim_to_tasks(task_features, est_obj.feature)
		          task_idx = np.argmax(sims)
		          est_task_objects[task_list[task_idx]].append(est_obj)
		          if sims[task_idx] > max_sims[task_list[task_idx]]:
		              max_sims[task_list[task_idx]] = sims[task_idx]
		  ```
	- 遍历所有的task，获取对应每个任务的gt_bbox, est_objs, task_feature，rel_objects为计算和task_feature大于阈值的物体(阈值和task相关)
		- ```
		      for i in range(len(task_list)):
		          gt_bboxes = gt_data.task_objects[task_list[i]].copy()
		          est_objects = est_task_objects[task_list[i]]
		          task_feature = task_features[i]
		          rel_objects = [obj for obj in est_objects if obj.compute_similarity(
		              task_feature) > min_sim_ratio*max_sims[task_list[i]]]
		          total_est_objects += len(rel_objects)
		  ```
	- 计算最佳iou匹配，以及strict和weak match，计算同recall
		- ```
		          while len(gt_bboxes) > 0 and len(rel_objects) > 0:
		              best_iou = 0
		              best_gt_est_pair = None
		              for gt_id, gt_bbox in enumerate(gt_bboxes):
		                  for est_id, est_obj in enumerate(rel_objects):
		                      iou = est_obj.compute_iou(gt_bbox)
		                      if iou > best_iou:
		                          best_iou = iou
		                          best_gt_est_pair = (gt_id, est_id)
		              if best_iou == 0:
		                  break
		              best_gt_bbox = gt_bboxes[best_gt_est_pair[0]]
		              best_est_obj = rel_objects[best_gt_est_pair[1]]
		              if best_est_obj.contains_centroid(best_gt_bbox):
		                  total_weak_matches += 1
		                  if best_est_obj.centroid_contained_in(best_gt_bbox):
		                      total_strict_matches += 1
		              # delete
		              del gt_bboxes[best_gt_est_pair[0]]
		              del rel_objects[best_gt_est_pair[1]]
		  ```
	- 最后除以est objects总数
		- ```
		      if total_est_objects == 0:
		          return 0, 0
		      return total_weak_matches / total_est_objects, total_strict_matches / total_est_objects
		  ```
- 数据结构
	- GtData
	  collapsed:: true
		- ```
		  class GtData:
		      def __init__(self, gt_yaml):
		          gt_data = yaml.safe_load(open(gt_yaml))
		          self.task_objects = {}
		          for task, gt_detections in yaml.safe_load(open(gt_yaml)).items():
		              self.task_objects[task] = []
		              for gt_detection in gt_detections:
		                  gt_bbox = get_open3d_bbox_from_dict(gt_detection)
		                  self.task_objects[task].append(gt_bbox)
		  ```
	- EvalObjects
	  collapsed:: true
		- ```
		  class EvalObjects:
		      def __init__(self, feature, o3d_mesh, oriented_bbox):
		          self.feature = feature
		          self.o3d_mesh = o3d_mesh
		          self.oriented_bbox = oriented_bbox
		          
		      def compute_similarity(self, other_feature)
		      def compute_iou
		      def contains_centroid
		      def centroid_contained_in
		  ```