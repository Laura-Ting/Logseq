- splatam参数摘除。sensor去掉。rviz去掉。
- config配置
- launch: splatam
- scripts
	- batch:
	- judges
	- nodes
		- dataloader_node: only when real world
		- mapper_node:
			- simulation: local dataset
			- update_dataset从habitat simulator中一帧一帧获取观测
		- __init__: service
	- src
		- dataloader
			- dataloader.py
				- HabitatDataset