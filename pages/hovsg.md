- 按照论文逻辑
- 1) geometry acc: floor seg + class-agnostic region seg
	- floor: Acc=100%
	- room: recall明显高。包含少数区域的小场景下的precision和recall会更高
	- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202411102124915.png){:height 105, :width 452}
	- floor是thresh用0.5m
	- room的recall和precision计算方法来自Hydra
- 2) semantic acc: region semantic + object semantic
	- room
		- use gt room segmentation
		- hovsg: view-embedding, voting
		- privileged baseline: gt object categories，label
		  unprivileged baseline: hovsg object categories，label
		- metric:
			- Acc=(text-wise replicability)
			- Acc≈(human eval，semantic correct就行) 不能全靠标签理解，LLM幻象，每个房间很多物体加重此现象。适用于unprivileged 面对under segmentation(人去判断分割不好的情况呗)。手动过滤了八个场景中的所有输出，并检查 LLM 是否倾向于正确答案
		- result:
			- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202411102127044.png){:height 244, :width 434}
			- unprivileged: view embedding在Acc=和Acc≈都超过unprivileged
			- privileged: GPT3.5超过，GPT4差不多
	- 3) object
		- AUCtopk: 计算obj和可能的文本特征的余弦相似度
		- 反映了在找到正确的标签前平均要进行多少次错误的
		- 不是每个k单独的，而是归一化之后的。gt类别是1624个
		- baseline
			- VLMaps
			- ConceptGraphs
			- pred和gt之间通过线性分配，iou>50%为真
			- VLMaps使用HOVSG的obj mask，平均所有mask feature作为obj feature
		- result
			- VLMaps: 不太好，因为密集特征聚集，依赖于一个微调的VLM和LSeg
			- HOVSG和ConceptGraph差不多，但好一点
			- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202411121450292.png){:height 136, :width 535}
- 4) query
	- 用GPT3.5 parsing，解析出floor, room, object，层次查询
	- result: hovsg和增强版的ConceptGraph
		- hovsg在o-r-f上增加了 11.69%，在o-r上 增加了2.2%。ConceptGraph在大场景上不好，hovsg虽然设计上会划分错房间但是性能更好
			- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202411121049144.png){:height 149, :width 538}
		- 机器人停在匹配的地面真实点云之一旁，并且与该点云的距离小于 1 米，则我们认为导航成功
			- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202411121052428.png){:height 148, :width 553}
- 按照代码逻辑
- 评估方面：几何，语义
- 楼层：
	- 几何
		- gt楼层上下边界排序，形成一个列表
		- pred楼层上下边界排序，形成一个列表
		  collapsed:: true
			- ```
			          gt_floors = [node for node in self.gt_graph.nodes if node.type == "floor"]
			          pred_floors = [node for node in pred_graph.nodes if type(node) == Floor]
			          
			          gt_floors_bounds = []
			          for gt_floor in gt_floors:
			              gt_floors_bounds.append([gt_floor.lower, gt_floor.upper])
			          gt_floors_bounds = [y for x in gt_floors_bounds for y in x]
			          gt_floors_bounds.sort()
			          
			          gt_floors_bounds_ = [
			              (gt_floors_bounds[i] + gt_floors_bounds[i + 1]) / 2 for i in range(1, len(gt_floors_bounds) - 1, 2)
			          ]
			          gt_floors_bounds = [gt_floors_bounds[0]] + gt_floors_bounds_ + [gt_floors_bounds[-1]]
			  
			          pred_floors_bounds = []
			          for pred_floor in pred_floors:
			              pred_floor_center, pred_floor_dims = find_box_center_and_dims(pred_floor.vertices)
			              pred_floor_y_bounds = [
			                  pred_floor_center[1] - pred_floor_dims[1] / 2,
			                  pred_floor_center[1] + pred_floor_dims[1] / 2,
			              ]
			              pred_floors_bounds.append(pred_floor_y_bounds)
			          pred_floors_bounds = [y for x in pred_floors_bounds for y in x]
			          pred_floors_bounds.sort()
			          pred_floors_bounds_ = [
			              (pred_floors_bounds[i] + pred_floors_bounds[i + 1]) / 2 for i in range(1, len(pred_floors_bounds) - 1, 2)
			          ]
			          pred_floors_bounds = [pred_floors_bounds[0]] + pred_floors_bounds_ + [pred_floors_bounds[-1]]
			  ```
		- 对应位置计算距离，阈值不超过0.5算为True, 即TP += 1
		  collapsed:: true
			- ```
			          # calc distance between each gt floor and each openmap floor
			          dist = np.abs(np.array(gt_floors_bounds) - np.array(pred_floors_bounds))
			  
			          TP, TN, FP, FN = 0, 0, 0, 0
			          floor_dist_threshold = 0.5
			          for i in range(len(dist)):
			              if dist[i] < floor_dist_threshold:
			                  TP += 1
			          FP = len(pred_floors_bounds) - TP
			          FN = len(gt_floors_bounds) - TP
			          precision = TP / (TP + FP) if (TP + FP) > 0 else 0
			          recall = TP / (TP + FN) if (TP + FN) > 0 else 0
			          accuracy = (TP + TN) / (TP + TN + FP + FN) if (TP + TN + FP + FN) > 0 else 0
			          
			          floor_metrics = {"tp": TP, "tn": TN, "fp": FP, "fn": FN, "acc": accuracy, "prec": precision, "recall": recall}
			          self.metrics["floors"] = floor_metrics
			  ```
		- ![](https://i-blog.csdnimg.cn/blog_migrate/d7a4b71d1f2393e35df6988fd990c2d9.jpeg){:height 253, :width 259}
		- TP: gt和pred都存在的，匹配上的楼层
		- FP: pred中存在但gt中不存在的楼层, pred_all - TP
		- FN: gt存在但是pred中不存在的楼层, gt_all - TP
		- TN: 没有计算逻辑，我们也不关心，永远是0
		- precision: TP/(TP+FP), pred中预测gt对的概率
		- recall: TP/(TP+FN), gt中有多少由pred预测对的概率
		- accuracy: TP+TN/(TP+FN+FP+TN), 所有边界上正确的结果
- 房间：
	- 几何
		- 都使用gt楼层
		- 创建大小为pred*gt的room_association_matrix，row有pred个数量，column有gt个数量
		  collapsed:: true
			- ```
			  gt_floors = [node for node in self.gt_graph.nodes if node.type == "floor"]
			  gt_rooms = [node for node in self.gt_graph.nodes if node.type == "room"]
			  pred_rooms = [node for node in pred_graph.nodes if isinstance(node, Room)]
			  
			  # find openmap room corresponding to each gt room based on distance between centers
			  room_association_matrix = np.zeros((len(pred_rooms), len(gt_rooms)))
			  ```
		- pred和gt计算在y=min_gt_height上的bev投影点云(即所有点都在地面上)，overlap并存到对应位置（经验证不是对称矩阵）
		  collapsed:: true
			- ```
			  for gt_floor in gt_floors:
			      for gt_room in gt_rooms:
			          if gt_room.mean_height > gt_floor.lower and gt_room.mean_height < gt_floor.upper:
			              for pred_room in pred_rooms:
			                  pred_room_points = np.asarray(pred_room.pcd.points)
			                  pred_mean_height = pred_room.room_zero_level + pred_room.room_height / 2
			                  if pred_mean_height > gt_room.min_height and pred_mean_height < gt_room.max_height:
			                      pred_room.bev_pcd = o3d.geometry.PointCloud()
			                      pred_room_points[:, 1] = gt_room.min_height  # overwrite this to get planes on same height
			                      pred_room.bev_pcd.points = o3d.utility.Vector3dVector(pred_room_points)
			                      gt_room.bev_pcd = gt_room.bev_pcd.voxel_down_sample(voxel_size=0.05)
			                      pred_room.bev_pcd = pred_room.bev_pcd.voxel_down_sample(voxel_size=0.05)
			                      overlap = find_overlapping_ratio_faiss(pred_room.bev_pcd, gt_room.bev_pcd, 0.05)
			  
			                      room_association_matrix[pred_rooms.index(pred_room)][gt_rooms.index(gt_room)] = overlap
			  ```
		- 计算hydra
			- hydra_overlap_pred(预测点云中多少点在真实点云半径范围内)和hydra_overlap_gt(真实点云中多少点在预测点云半径范围内)也是pred*gt大小，但是根据各自bev点数归一化
			  collapsed:: true
				- ```
				          hydra_room_overlap_over_pred = np.zeros((len(pred_rooms), len(gt_rooms)))
				          hydra_room_overlap_over_gt = np.zeros((len(pred_rooms), len(gt_rooms)))
				          for gt_floor in gt_floors:
				              for gt_room in gt_rooms:
				                  if gt_room.mean_height > gt_floor.lower and gt_room.mean_height < gt_floor.upper:
				                      for pred_room in pred_rooms:
				                          pred_room_points = np.asarray(pred_room.pcd.points)
				                          pred_mean_height = pred_room.room_zero_level + pred_room.room_height / 2
				                          if pred_mean_height > gt_room.min_height and pred_mean_height < gt_room.max_height:
				                              pred_room.bev_pcd = o3d.geometry.PointCloud()
				                              pred_room_points[:, 1] = gt_room.min_height  # overwrite this to get planes on same height
				                              pred_room.bev_pcd.points = o3d.utility.Vector3dVector(pred_room_points)
				                              pred_room.bev_pcd.colors = o3d.utility.Vector3dVector(
				                                  np.array([[0, 0, 1] for i in range(len(pred_room.bev_pcd.points))])
				                              )
				                              gt_room.bev_pcd.colors = o3d.utility.Vector3dVector(
				                                  np.array([[1, 0, 0] for i in range(len(gt_room.bev_pcd.points))])
				                              )
				                              gt_room.bev_pcd = gt_room.bev_pcd.voxel_down_sample(voxel_size=0.05)
				                              pred_room.bev_pcd = pred_room.bev_pcd.voxel_down_sample(voxel_size=0.05)
				                              overlap_over_pred = min(
				                                  find_intersection_share(
				                                      np.asarray(gt_room.bev_pcd.points), np.asarray(pred_room.bev_pcd.points), 0.05
				                                  ),
				                                  1.0,
				                              )
				                              overlap_over_gt = min(
				                                  find_intersection_share(
				                                      np.asarray(pred_room.bev_pcd.points), np.asarray(gt_room.bev_pcd.points), 0.05
				                                  ),
				                                  1.0,
				                              )
				                              hydra_room_overlap_over_pred[pred_rooms.index(pred_room)][
				                                  gt_rooms.index(gt_room)
				                              ] = overlap_over_pred
				                              hydra_room_overlap_over_gt[pred_rooms.index(pred_room)][
				                                  gt_rooms.index(gt_room)
				                              ] = overlap_over_gt
				  ```
			- hydra_precision为累计的max的hydra_overlap_pred / pred总数
			- hydra_recall为累积的max的hydra_overlap_gt / gt总数
			  collapsed:: true
				- ```
				          hydra_precision = 0.0
				          hydra_recall = 0.0
				          for i in range(len(pred_rooms)):
				              # get max overlap for each pred room
				              prec_max_overlap = np.max(hydra_room_overlap_over_pred[i, :])
				              hydra_precision += prec_max_overlap
				  
				          for j in range(len(gt_rooms)):
				              # get max overlap for each gt room
				              rec_max_overlap = np.max(hydra_room_overlap_over_gt[:, j])
				              hydra_recall += rec_max_overlap
				  
				          hydra_precision = hydra_precision / len(pred_rooms)
				          hydra_recall = hydra_recall / len(gt_rooms)
				  ```
		- 用linear_sum_assignment找到所有最大的行索引row_ind和列索引col_ind
		  collapsed:: true
			- ```
			          # calculate TP, FP, FN for rooms
			          row_ind, col_ind = linear_sum_assignment(room_association_matrix, maximize=True)
			          acc_values = list()
			          prec_values = list()
			          rec_values = list()
			  ```
		- 生成0-1划分11个刻度的threshold，TP为room_association_matrix[row_ind][col_ind]>threshold个数总和
		- 计算FP, FN同房间，计算每轮precision和recall和accuracy，数组acc_values, prec_values, rec_values存起来
		  collapsed:: true
			- ```
			          for eval_idx, thresh in enumerate(np.linspace(0.0, 1.0, 11, endpoint=True)):
			              TP, TN, FP, FN = 0, 0, 0, 0
			              iou_threshold = thresh
			              TP = np.sum(room_association_matrix[row_ind, col_ind] > iou_threshold)
			              FP = len(pred_rooms) - TP
			              FN = len(gt_rooms) - TP
			              precision = TP / (TP + FP) if (TP + FP) > 0 else 0
			              recall = TP / (TP + FN) if (TP + FN) > 0 else 0
			              accuracy = (TP + TN) / (TP + TN + FP + FN) if (TP + TN + FP + FN) > 0 else 0
			              acc_values.append(accuracy)
			              prec_values.append(precision)
			              rec_values.append(recall)
			  ```
		- 根据prec_values和rec_values计算AP(计算prec和rec的线下面积)
		  collapsed:: true
			- ```
			          rec_values.sort()
			          avg_prec = np.trapz(prec_values, rec_values)
			  
			          print("----- Room Evaluation ------")
			          room_metrics = {
			              "acc@IoU=0.5": acc_values[6],
			              "ap": avg_prec,
			              "prec@IoU=0.5": prec_values[6],
			              "recall@IoU=0.5": rec_values[6],
			              "gt": len(gt_rooms),
			              "pred": len(pred_rooms),
			              "hydra_prec": hydra_precision,
			              "hydra_recall": hydra_recall,
			          }
			          self.metrics["rooms"] = room_metrics
			          for k, v in room_metrics.items():
			              print("{}: {}".format(k, v))
			          print("----------------------------")
			  ```
	- 语义
- 物体：
	- 几何(instance evaluation)
		- 计算bbox iou存到obj_iou_assoc_matrix, 只要iou>0就计算overlap,overlap存到obj_overlap_assoc_matrix
		  collapsed:: true
			- ```
			          object_metrics = {"instances": {}, "instances_iou50": {}, "ins_semantics": {}}
			          self.metrics["objects"] = object_metrics
			          print("----- Object Evaluation ------")
			  
			          gt_objects = [node for node, n_data in self.gt_graph.nodes(data=True) if (node.type == "object")]
			          pred_objects = [node for node in pred_graph.nodes if (type(node) == Object)]
			  
			          # Evaluation of class-agnostic instance segmentation on point clouds
			          # find openmap object corresponding to each gt object based on distance between centers
			          obj_iou_assoc_matrix = np.zeros((len(pred_objects), len(gt_objects)))
			          obj_overlap_assoc_matrix = np.zeros((len(pred_objects), len(gt_objects)))
			  
			          for gt_obj in gt_objects:
			              gt_obj_center = gt_obj.obb_center
			              gt_obj_dims = gt_obj.obb_dims
			  
			              gt_obj_bbox = np.array(gt_obj.pcd.get_axis_aligned_bounding_box().get_box_points())
			              for pred_obj in pred_objects:
			                  pred_obj_bbox = np.array(pred_obj.pcd.get_axis_aligned_bounding_box().get_box_points())
			                  pred_obj_center, pred_obj_dims = find_box_center_and_dims(pred_obj_bbox)
			  
			                  iou = get_3d_iou(gt_obj_center, gt_obj_dims, pred_obj_center, pred_obj_dims)
			                  obj_iou_assoc_matrix[pred_objects.index(pred_obj)][gt_objects.index(gt_obj)] = iou
			  
			                  overlap = 0.0
			                  if iou > 0.0:
			                      overlap = find_overlapping_ratio_faiss(pred_obj.pcd, gt_obj.pcd, 0.02)
			                      obj_overlap_assoc_matrix[pred_objects.index(pred_obj)][gt_objects.index(gt_obj)] = overlap
			  ```
		- 根据选择确定iou还是overlap但是后续都是在overlap矩阵中计算，只不过是用谁的row_ind和col_ind
		  collapsed:: true
			- ```
			          if eval_metric == "iou":
			              row_ind, col_ind = linear_sum_assignment(obj_iou_assoc_matrix, maximize=True)
			          elif eval_metric == "overlap":
			              row_ind, col_ind = linear_sum_assignment(obj_overlap_assoc_matrix, maximize=True)
			  ```
		- 这步同房间：生成0-1划分11个刻度的threshold，TP为obj_overlap_assoc_matrix[row_ind][col_ind]>threshold个数总和
		- 计算FP, FN，计算每轮precision和recall和accuracy，数组acc_values, prec_values, rec_values存起来
		  collapsed:: true
			- ```
			          acc_values = list()
			          prec_values = list()
			          rec_values = list()
			          for eval_idx, thresh in enumerate(np.linspace(0.0, 1.0, 11, endpoint=True)):
			              TP, TN, FP, FN = 0, 0, 0, 0
			              TP = np.sum(obj_overlap_assoc_matrix[row_ind, col_ind] > thresh)
			              FP = len(pred_objects) - TP
			              FN = len(gt_objects) - TP
			              precision = TP / (TP + FP) if (TP + FP) > 0 else 0
			              recall = TP / (TP + FN) if (TP + FN) > 0 else 0
			              accuracy = (TP + TN) / (TP + TN + FP + FN) if (TP + TN + FP + FN) > 0 else 0
			              acc_values.append(accuracy)
			              prec_values.append(precision)
			              rec_values.append(recall)
			          rec_values.sort()
			          avg_prec = np.trapz(prec_values, rec_values)
			  
			          obj_instance_metrics = {"ap": avg_prec, "gt": len(gt_objects), "pred": len(pred_objects)}
			          self.metrics["objects"]["instances"] = obj_instance_metrics
			  ```
		- 计算thresh=0.5作为iou50(但是为啥这个要重新算？)
		  collapsed:: true
			- ```
			          TP, TN, FP, FN = 0, 0, 0, 0
			          threshold = 0.5
			          TP = np.sum(obj_overlap_assoc_matrix[row_ind, col_ind] > threshold)
			          FP = len(pred_objects) - TP
			          FN = len(gt_objects) - TP
			  
			          precision = TP / (TP + FP) if (TP + FP) > 0 else 0
			          recall = TP / (TP + FN) if (TP + FN) > 0 else 0
			          accuracy = (TP + TN) / (TP + TN + FP + FN) if (TP + TN + FP + FN) > 0 else 0
			  
			          obj_instance_iou50_metrics = {
			              "acc": accuracy,
			              "prec": precision,
			              "recall": recall,
			              "tp": int(TP),
			              "fp": int(FP),
			              "fn": int(FN),
			              "gt": len(gt_objects),
			              "pred": len(pred_objects),
			          }
			          self.metrics["objects"]["instances_iou50"] = obj_instance_iou50_metrics
			  ```
	- 语义(Top-k semantics evaluation)
		- object_semantics_eval_tp_auc函数
			- 参数：`self, top_k_spec, row_ind, col_ind, pred_objects, gt_objects, gt_text_feats, gt_classes`, top_k_spec一个列表，包含需要计算的top-k值[1, 5, 10]
			- success_k存储在每个`k`值下，预测正确的对象索引
				- ```
				  success_k = {k: list() for k in top_k_spec}
				  ```
			- 遍历由重叠计算出的row_ind和col_ind所在的pred和gt, pred的embedding和每个gt feature都计算cos sim得到一个1624维的dot_sim
			- 计算pred对象的向量和所有的gt文本特征(1624)的余弦相似度，最大的相似度存入similarity_matrix
			- 获取前k个最大值的索引，如果真实类别在top_k_class中，算true
				- ```
				          for pred_idx, gt_idx in zip(row_ind, col_ind):
				              # dot_sim = np.dot(pred_objects[pred_idx].embedding, gt_text_feats.T)
				              dot_sim = (
				                  pairwise_cosine_similarity(
				                      torch.from_numpy(pred_objects[pred_idx].embedding.reshape(1, -1)).float(),
				                      torch.from_numpy(gt_text_feats).float(),
				                  ).squeeze(0).numpy())
				              # sort the dot similarity scores in descending order
				              sorted_dot_similarity = np.sort(dot_sim)[::-1]
				              for k in top_k_spec:
				                  top_k_idx = np.argsort(dot_sim)[::-1][:k]
				                  # get names of top k classes
				                  top_k_classes = [gt_classes[idx] for idx in top_k_idx]
				                  if gt_objects[gt_idx].category in top_k_classes:
				                      success_k[k].append((pred_idx))
				  ```
			- 计算top-k准确率，在所有gt类别上归一化top-k值，计算AUCtop-k
				- ```
				          top_k_acc = {k: len(v) / len(col_ind) for k, v in success_k.items()} #len(v)是成功预测的次数，len(col_ind)是尝试的总次数
				          norm_top_k = [k / len(gt_classes) for k in top_k_spec] #在1624类别上计算
				          tp_top_k_auc = np.trapz(list(top_k_acc.values()), norm_top_k)
				          return top_k_acc, tp_top_k_auc
				  ```
		- 特定阈值下的top-k准确率和一系列阈值下的top-k AUC得分
		  collapsed:: true
			- ```
			          # eval top-k accuracy at specific thresholds
			          top_k_acc_representative, _ = self.object_semantics_eval_tp_auc(
			              self.params.eval.hm3dsem.top_k_object_semantic_eval,
			              row_ind,
			              col_ind,
			              pred_objects,
			              gt_objects,
			              gt_text_feats,
			              gt_classes,
			          )
			          # eval top-k auc score based on a range of thresholds
			          _, tp_multi_top_k_acc_auc = self.object_semantics_eval_tp_auc(
			              [k for k in range(0, len(gt_classes), 10)],
			              row_ind,
			              col_ind,
			              pred_objects,
			              gt_objects,
			              gt_text_feats,
			              gt_classes,
			          )
			          obj_instance_semantics_topk_class_metrics = dict()
			          obj_instance_semantics_topk_class_metrics["tp_top_k_acc"] = top_k_acc_representative
			          obj_instance_semantics_topk_class_metrics["top_k_auc"] = tp_multi_top_k_acc_auc
			          self.metrics["objects"]["ins_semantics"] = obj_instance_semantics_topk_class_metrics
			  ```