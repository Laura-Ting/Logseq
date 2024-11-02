- Basic concepts and experience of cloud computing
	- basic concepts
		- background
			- The needs of computing resources->higher requirements, traditional solution is buying equipment->leasing from third party
			- The cloud is a resource connected via the Internet to support all types of computing
				- Users purchase resources according to their needs.
				- Cloud computing is not a special way of computing, but
				  for a wide range of applications.
				- Integrate resources and have strong computing power.
				- Users can use resources and enjoy them anytime and anywhere
				  Computing services.
				- Use fault-tolerant technology, node swap and other measures to protect the reliability of the service is impaired
			- AWS, Google Cloud, IBM Cloud
			- Ali Cloud, Baidu Cloud, Huawei Cloud
		- Definition
			- User perspective
				- Cloud computing is a customized service platform provided by a third party through the Internet, including hardware services, software services, data resource services, etc.
				- The user only needs to pay for the service received.
			- Cloud computing provider perspective
			  collapsed:: true
				- Cloud computing is a platform for providing cloud computing services that require resource integration and technology integration.
				- problems need to be solved
				  collapsed:: true
					- Large -scale issues: the mass of computing resources and the large scale of data
					- low cost problem of data:  more discounts to customers, to maximize resource utilization rates
					- Service operation problems
			- Platform perspective
			  collapsed:: true
				- calculation: parallel and distributed computing system composed of a group of internal interconnecting physical servers
				- service: the platform of elastic hardware, software and data services provided by the Internet-social service
				- storage: permanent storage user data storage platform
		- Service Type
		  collapsed:: true
			- Infrastructure as a Service
			  collapsed:: true
				- infrastructure is that services are provided to users, including computing, storage, networks, and other basic computing resources. Users can deploy and run any software, including operating systems and applications.
				- Virtualization technology, distributed storage technology, High -speed network technology, large -scale resource management technology, cloud service billing technology
			- Platform as a Service
			  collapsed:: true
				- Provide software research and development platforms as a service to users.
				- PaaS is a distributed platform service that provides users with a complete service platform including application design, application development, application testing and application hosting.
				- The main users of PaaS are developers.
				- High integrated and friendly development environment, Multi -tenant mechanism, Strong elasticity, rich services, fine management and control
			- Software as a Service
			  collapsed:: true
				- "Software that needs to be used" is a software delivery mode based on the Internet.
				- 1. running in the cloud, not local. 2. Users can run software anytime, anywhere, and can run software anytime, anywhere, and can run software. 3. User data hosting can also be stored on the local device on the cloud server of the SaaS service provider. 4. Users only have the right to use software and have no ownership. 5. Users only need to pay according to the amount of use
				- Simple use, support public agreement, multi -charter mechanism, strong elasticity, low cost, security guarantee
		- Deployment Model
		  collapsed:: true
			- Public Cloud
			  collapsed:: true
				- open to the public. is operated by the cloud service provider and provides various IT resources for end users to support the concurrent requests of a large number of users.
				- do not need a large amount of investment and long construction process in the early stage, advantage of scale and its operating costs are relatively low
				- Data security and privacy issues are more worried when using public clouds
			- Private Cloud
			  collapsed:: true
				- built by the company or other organizations to use itself.
				- deployed on the enterprise's internal network, support dynamic and flexible infrastructure
				- enterprises need to have a large number of early investment and use traditional business models.
			- Hybrid Cloud
			  collapsed:: true
				- a hybrid cloud computing model built by private cloud and public cloud providers.
				- Using a hybrid cloud computing mode, institutions can run non -core applications on public clouds, while supporting its core programs and internal sensitive data on private clouds.
				- higher requirements for providers
				- public cloud and private cloud are independent, but the interior of the cloud can combine each other
	- Core Technique
	  collapsed:: true
		- 1. Data center technology
		- 2.Network technology
		- 3. Elastic calculation
		- 4. Cloud storage technology
		- 5. Cloud database
		- 6.monitor
		- 7. Safety
		- 8. Cloud computing middleware
		- 9. Data calculation
	- Typical scenes
		- OLTP：OnLine Transaction Processing
		- OLAP：OnLine Analytical Processing
		- Big data analysis application application
		- Search application
		- Micro -service application
	- Cloud computing platform architecture
	  collapsed:: true
		- Cloud platform provider often starts from its own
		- A region refers to a physical node globally, and each area consists of multiple available areas.
		- The available area consists of one or more decentralized data centers.
- Data Center
  collapsed:: true
	- A set of complex facilities, Warehouse-Scale Computer
	- constitution
	  collapsed:: true
		- Server
		  collapsed:: true
			- Basic calculation unit of data center: Parallel computing, large -scale storage, high -throughput network services
			  id:: 670f5ec4-4962-402a-bc3a-5e1f9ef4015d
			- Higher handling ability, better stability and reliability, and high requirements for scalability and managementability
		- network and other IT devices
		  collapsed:: true
			- The core of the data center provides services is online telecommunications equipment.
			- Data Center is connected to external network operators through a major entrance.
			- include at least one main distribution area (MDA), one or more horizontal distribution areas, and a multi -device distribution area.
			  collapsed:: true
				- The main distribution area is the center of the cable infrastructure in the data center.Computer room central routers, central LAN switches, central storage regional networks, etc. The central control equipment is usually located in the main distribution area.
				- (2) The trunk cable of the data center may cross multiple buildings and connect to each level distribution area.
				- (3) In the horizontal distribution area, the horizontal cable extends between telecommunications equipment in the same layer until each rack and server.
		- power system equipment
		  collapsed:: true
			- The power system equipment is the lifeline of the data center.
			- **a layered redundant power supply method** to ensure the availability and stability of the power supply equipment at all levels of the data center.
			  collapsed:: true
				- Power system
				- energy storage system
				- power distribution system
		- air conditioning refrigeration equipment 15-32°C
		  collapsed:: true
			- Excessive temperature may cause the server motherboard and semiconductor original device to stop running or even damage.
			- Excessive temperature will lead to an increase in cooling power consumption and waste of resources.
	- Cloud computing data center
	  collapsed:: true
		- separate->integrate: HPC Data Center+Internet Data Center
		- Calculate dense data centers
		  collapsed:: true
			- provide users with fast response calculation services
			- search engine. For user requests, the final processing result must be returned within a certain period of time.
		- storage dense data centers
		  collapsed:: true
			- Applications include network hard disk, CDN (Content Delivery Network) service, also known as content distribution service and video service
		- Equipment scalability of computing device
		  collapsed:: true
			- Modular design:
			  collapsed:: true
				- Vertical extension: Refers to add resources to the same logical unit to increase the capacity.For example, upgrade the server CPU, increase the capacity of memory bar, and so on.
				- Horizontal expansion: It refers to adding multiple logical units and making them work together.Most cluster technologies are expanded through horizontal extensions.
			- hardware restructuring: redesign, network, motherboard, server, rack, etc., so as to adapt to cloud computing data centers
	- Data Center network architecture
	  collapsed:: true
		- traditional: Hierarchical Inter-Networking Model
		  collapsed:: true
			- Access Layer / Edge Layer / ToR（Top of Rack）
				- Access switches are usually located on the top of the rack. They are physically connected to the server
			- Aggregation Layer / Distribution Layer
				- Gather the switch to connect to the ACCESS switch, and provide other services, such as firewalls, SSL Offload, intrusion detection, network analysis, etc.
			- Core Layer
				- The core switch provides high -speed forwarding for the package of the import and export data center, and provides connectivity for multiple concentration layers. The core switch usually provides an elastic L3 route network for the entire network.
			- ![](https://raw.githubusercontent.com/qugushihua/blog-images/master/202410161457963.png){:height 233, :width 459}
		- problem
		  collapsed:: true
			- High requirements for top -level network equipment;
			- Poor network fault tolerance;
			- Insufficient bandwidth of east and west traffic.
		- Solution: The two -layer architecture
		  collapsed:: true
			- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202410161501223.png){:height 154, :width 267}
			- The core switches of the sophomore architecture are the division of L2 and L3.The core switch is the L2 network, that is, the entire data center.
			- problem: Broadcast，Unknown unicast, Multicast
		- still use traditional
- 3 基础设施及服务（Infrastructure as a Service，IaaS）【7】
	- 1. 概述
	- 2. 基本功能
	- 3. 整体架构
	- 4. 服务器虚拟化技术
	- 5. OpenStack, StarVCenter 为例，介绍各项技术
- 4 平台即服务（Platform as a Service，PaaS）【4】
	- 1. 概述
	- 2. 核心系统
	- 3. Cloud Foundry，Hadoop
- 5 软件即服务（Software as a Service，SaaS）【1】
	- 1. 概述
	- 2. 支撑平台
	- 3. SaaS 应用
	- 4. SaaS 发展趋势
- 6 商业云计算平台了解与实践【1】
	- 1. Amazon 云计算平台
	- 2. Google 云计算平台
	- 3. Saleforce 云计算平台
	- 4. Microsoft Azure 云计算平台
	- 5. 阿里云
	- 6. 百度云
	- 7. 华为云