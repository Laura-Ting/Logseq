- overview structure
	- Req Analysis
	- OO Design->Software Architecture
	- SQA & Testing
	- Software Management
-
- 类之间的六种关系
  collapsed:: true
	- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202410152203098.png){:height 158, :width 316}
- 软件设计7原则
  collapsed:: true
	- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202410152204885.png){:height 184, :width 404}
-
- OOM: Object-Oriented Modeling
  collapsed:: true
	- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202410151003163.png){:height 188, :width 668}
	- basic concept
		- OO
		  collapsed:: true
			- Structured Programming vs OOP: requirement, analysis, design, implement, test
			  collapsed:: true
				- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202410151005930.png){:height 341, :width 645}
			- OO advantages:
			- object & class
			- principles
			  collapsed:: true
				- abstraction
				- encapsulation
				- decomposition
				- generalization
				- polymorphism
				- hierachy
				- reuse
		- UML: unified modeling language
		  collapsed:: true
			- intro: visualizing, specifying, constructing, documenting, a software-intensive system
			- structural diagrams vs behavior diagrams
			  collapsed:: true
				- structural diagrams: package diagram, class diagram, component diagram, deployment diagram, object diagram, composite structure diagram
				- behavior diagrams: use case diagram, activity diagram, state machine diagram, interaction diagram, sequence diagram, communication diagram, interactive overview diagram, timing diagram
			- 4+1 views
			  collapsed:: true
				- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202410151019461.png){:height 161, :width 441}
			- class diagram
			- object diagram
			- package diagram
			- component diagram
			- deployment diagram
			- Use Case Diagrams
			- Activity Diagrams
			- State Machine Diagrams
			- Sequence Diagram
			- Communication Diagrams
	- OOA
		- business modeling
		  collapsed:: true
			- **业务用例模型**
			- Business Object Model
			- **系统模型**
			- Domain Modeling
		- use case modeling
		  collapsed:: true
			- requirements
				- **从业务模型获取需求**
				- from stakeholders: collect data, field observation, interview, have a meeting, prototype, questionnaire survey
			- model
				- Use case diagrams represent functional requirements
				- actor is beyond boundary and interact information with boundary, can be anything
					- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202410151055905.png){:height 94, :width 60}
					- generalization
						- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202410151059016.png){:height 118, :width 122}
					- actor's documents: description of role, basic characteristic
				- use case
					- Observable, use case value, actor perspective, system execution
					- **用例命名**: •**（状语）动词****+****（定语****+ ****）宾语**
					- **用例粒度**
					- **把包含复杂交互的路径独立出去**,**用例****之间没有顺序的关系**
					- **用例图**
						- •**箭头代表通信的发起方**
						- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202410151105196.png){:height 263, :width 450}
			- **用例文档**
				- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202410151115428.png){:height 279, :width 554}
				- **平衡涉众之间的利益**,•**区分涉众与参与者**
				- Pre condition,Post condition
					- •**条件必须是系统可以感知的**
					- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202410151328134.png){:height 189, :width 160}
				- 事件流
					- **使用****自然语言描述**
					- **说明参与者****与系统****交互具体的步骤**，只描述从系统外部所看到的活动
					- **不细化****GUI**
					- **分支和循环的描述**
					- 基本事件流+备选事件流
						- •**基本事件流**
						- •**用例的主路径、愉快路径（****Happy Path****）**
						- •**通常用来描述一个理想世界，即没有任何错误发生的情况**
						- •**复杂的基本流可以分解成多个子流**
						- •**备选事件流**
						- •**基本事件流中的分支或异常情况**
						- •**注意如何与基本流****衔接**
					- 补充约束：数据需求，业务规则，非功能需求，设计约束
			- **重构****用例模型**
				- •**用例关系**：Include(reuse, must perform), Extend(special, perform in special conditions), Generalization
				- •**用例分级**
				- •**用例分包**
					- 策略：•**基于业务主题的分包**，•**按照参与者分包**，•**基于开发团队的分包**，•**基于发布情况的分包**
		- use case analysis / System analysis
		  id:: 670dcce3-b185-44e5-828a-75b93c940fbc
		  collapsed:: true
			- 系统内部应该提供哪些核心业务元素和关系，以实现需求模型提出的目标
			- 用例模型：表达系统功能。分析模型：表达内部机理
			- 包括两个层次：架构分析和用例分析
			- • 包括两类模型：静态结构(包图、类图)和动态交互(顺序图、通信图)
			- 架构分析
				- 定义备选架构:层次结构，Client-server, MVC
					- 分层
					  collapsed:: true
						- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202410151357434.png)
					- BCE
					  collapsed:: true
						- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202410151359662.png)
						- 边界层(Boundary)负责系统与参与者之间的交互，包含GUI或其它的接口
						- 控制层(Control)处理系统的控制逻辑，包括控制类，根据用户所发的请求类型，调用实体层的相关功能
						- 实体层(Entity)管理系统使用的信息，包括核心业务类，提供核心业务逻辑
				- 关键抽象(领域模型图)
					- 业务的核心价值->实体类，领域专家识别
				- 创建用例实现：软件的内部结构
					- 完善用例文档：user perspective+system perspective, black->white
					- 识别分析类：边界类、控制类和实体类
					  collapsed:: true
						- 边界类
						  collapsed:: true
							- 边界类表示系统与参与者之间的边界• 代表系统与环境的交互• 是接口和外部事物的中间体• 构造型<<boundary>>• 两类边界类• 用户界面类(分为用户图形界面，无图形界面)• 系统和设备接口类
							- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202410151406171.png){:height 111, :width 89}
						- 控制类
						  collapsed:: true
							- 控制类表示系统的控制逻辑• 系统行为的协调器• 构造型<<control>>• 识别控制类• 在系统开发早期，为每个用例定义一个控制类，负责该用例的控制逻辑• 针对复杂用例，可为备选路径分别定义不同控制类
							- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202410151407196.png){:height 88, :width 78}
						- 实体类
						  collapsed:: true
							- 实体代表了待开发系统的核心概念，来自于对业务中的实体对象的归纳和抽象（例如银行业务系统中的Account类）• 实体类的主要职责是存储和管理系统内部的信息• 实体类中所封装的数据往往都是永久的，都是应该存储到数据库之中的。• 使用构造型<<entity>>• 可以从以下文件中找到实体类• 用例事件流(需求) 、• 业务模型(业务建模) 、• 词汇表(需求)实体类记号
							- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202410151408815.png){:height 104, :width 102}
					- 分析交互：将用例行为分配给类(给类增加方法，属性)：Sequence Diagram
					  collapsed:: true
						- 发送消息：箭头指向哪个对象，则该对象所对应的类就包含对应消息的方法。
						- Sequence Diagram
							- 顺序图是一种交互图，描述对象之间的动态交互关系，着重体现对象间消息传递的时间顺序•
							- 对象(Object)：对象、对象的生命线、对象的执行发生和对象的删除•
							- 消息(Message)：简单消息、同步消息、异步消息、返回消息
							  collapsed:: true
								- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202410151413462.png)
							- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202410151413354.png)
							- • 边界类：承担与参与者(演员，角色)进行通信的职责• 控制类：接收参与者由用户界面传递来的请求，然后，根据请求的类型，决定调用哪些实体类。承担协调用例参与者与系统之间交互的职责• 实体类：实体类代表系统的核心概念，来自于业务中实体对象的归纳与抽象，用于记录系统所维护的数据和对数据的处理行为。承担对被封装的内部数据进行操作的职责
							- 专家模式：将职责分配给具有当前职责所需要的数据的类
							- 顺序图中的保护条件和循环• 顺序图主要用于描述顺序的执行流程，对于简单分支或循环，可直接描述• 可以使用保护条件，用[ ]括起来描述，表示条件为真时才执行，否则不执行• 循环条件要在条件前加上“*”来描述，表示条件为真时重复执行• 其他约束用{ }括起来-24
							- 顺序图中的交互使用• UML 2.0之后，允许利用关键词ref，重用在其它地方定义的时序图，其目的是简化复杂的时序图• 交互使用 (Interaction Use)：欲复用在其它地方定义的交互➢使用ref记号表示复用（参考）在其它地方定义的时序图• 循环(Loop): 循环控制结构体,可用在时序图中• 使用Loop记号表示一个循环体(见例子)
					- 完成参与类类图：参与用例实现的类的类图VOPC, View Of Participating Classes Class Diagram
					  collapsed:: true
						- 类图中的元素来自于交互图中的对象
						- 类图中的关系来自于交互图中的消息传递方向，分析阶段主要使用关联关系
					- 定义分析类
						- 定义职责：从Sequence Diagram中获取
						- 定义属性：属性类型应来自业务领域
						- 定义关系
						  collapsed:: true
							- 关联关系：协作关系，语义联系
								- 细化关联关系：名称、端点和角色名、多重性
								- 自反关联
								- Association Class
							- 聚合关系：整体-部分
							- 泛化关系：抽象-具体
						- 统一分析类：单一的明确定义的概念，而不会职责重叠
	- OOD
		- principles
		  collapsed:: true
			- LSP: The Liskov Substitution Principle
			- OCP: The Open-Close Principle
			  collapsed:: true
				- open for extension, but closed for modification
				- an entity can allow its behavior to be modified without altering its source code
				- 好处：扩展
				- Bertrand Meyer(复用实现，而不复用接口)->1990(复用抽象接口，而不是复用实现)
			- SRP: The Single Responsibility Principle
				- 就一个类而言，应该仅有一个引起它变化的原因
				- Cohesion
			- ISP: The Interface Segregation Principle
				- no client (class) should be forced to depend on methods it does not use.
				- 将很大的接口拆分成较小的，更具体的接口；使得客户类只需要知道它所感兴趣的方法
				- 使得一个系统保持较低的耦合，以便于更容易重构，修改与部署
				- 使用多个专门的接口比使用单一的接口好• 一个类对另一个类的依赖性应当是建立在最小的接口上的• 避免接口污染(Interface Pollution)
			- DIP: The Dependency Inversion Principle
				- • 高层模块不依赖于低层模块，二者都依赖于抽象• 抽象不依赖于细节，细节依赖于抽象• 针对接口编程，不要针对实现编程• 又称控制反转(IoC，Inversion of Control)、依赖注入
		- Refine Architecture
		  collapsed:: true
			- design:Decomposition design,Family Pattern design,Invention design
			- Design Elements: Package, Design Classes,Subsystem,Interface,Active Class
			- 包图: 将模型元素分组的机制
			  collapsed:: true
				- 依赖关系
				  collapsed:: true
					- 包元素的可见性
					- merge:import, access
				- 设计原则
				  collapsed:: true
					- 内聚性原则
						- 复用发布等价原则（REP）：包内的公有元素要么都是可复用的，要么都是不可复用的
						- 共同复用原则（CRP）：包中所有的类应该是共同复用的；如果复用了一个类，就要复用包中所有的类
						- 共同封闭原则（CCP）：一个变化会对一些类产生影响，而不会对其它类产生影响；将那些受影响的类放在一个包里面。
					- 耦合性原则
						- 无环依赖原则（ADP）：包图中的依赖关系不允许存在环
						- 稳定依赖原则（SDP）：朝着稳定的方向依赖，如果一个包被许多包所依赖，那么它就是稳定的
						- 稳定抽象原则（SAP）：稳定的包应该包含抽象类(接口)，以便支持扩展（因此稳定）
			- 组织设计类: 分析类->设计类，打包设计类
			  collapsed:: true
				- 如果系统边界(用户界面、系统接口)不太可能进行大的更改，则可以将边界类和在功能上与它们相关的类打包到一起
			- 接口(Interface)是类、子系统或构件提供的操作的集合• 接口允许用户以公开的方式定义多态，并且和实现没有直接联系• 接口支持“即插即用”的结构
			- 子系统与接口
			  collapsed:: true
				- 子系统(Subsystem)是一种介于包和类之间的一种设计机制，它实现一个或多个接口所定义的行为。具有包的语义：能够包含其它模型元素。具有类的语义：具有行为
				- 子系统：• 提供行为• 完全封装实现细节• 容易替换
				- 包：• 不提供行为• 不完全封装实现细节• 难以替换
			- 永久数据存储
			  collapsed:: true
				- 关系数据库（RDBMS)（SQL server, Oracle, MySQL Server
				- 面向对象数据库（OODBMS）
		- General Principles in Assigning Responsibilities
		  collapsed:: true
			- Information Expert Pattern
			  collapsed:: true
				- 将责任分配给信息专家类(拥有实现该责任的信息的类)
				- 支持信息封装，因为对象使用自己的信息来完成任务
				- 支持低耦合，这将导致更健壮和可维护的系统
				- 支持高内聚，行为被分配到相应的专家类里面使得你能够写出更加内聚的轻量级的类
			- Creator Pattern
			  collapsed:: true
				- B  aggregates  A  objects
				- B contains A objects
				- B records  instances of  A objects
				- B closely uses A objects
				- B has the initializing data that will be passed to A when it is created (thus B is an Expert class for creating A objects).
				- B is a creator of A objects
				- 使用创建者模式的好处
				- 支持低耦合,从而意味着低维护依赖，高复用。
			- Low Coupling Pattern
			  collapsed:: true
				- 通过分配责任使得得到低耦合的设计
			- Controller Pattern
			  collapsed:: true
				- Assign the responsibility for receiving or handling a system event message to a class representing one of the following choices:
				- Represents the overall system, device, or subsystem(facade controller).
				- Represents a use case scenario within which the system event occurs,(use-case or session controller).
				- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202410151515156.png){:height 323, :width 369}
				- 增加用户图形界面的复用，产生可插拔的用户图形界面可能性.• 保证应用逻辑不包含在GUI层.• 将系统操作代理给一个Controller类的好处是支持应用逻辑在将来的应用程序中的复用
			- Polymorphism Pattern
			  collapsed:: true
				- When related alternatives or behaviors vary by type (class), assign responsibility for the behavior-using polymorphic operations-to the types for which the behavior varies.
				- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202410151518607.png)
			- Pure Fabrication Pattern
			  collapsed:: true
				- 专家模式）将责任分配给域层软件类会导致内聚性或耦合性差，低复用的可能性
				- Assign a highly cohesive set of responsibilities to an artificial or convenience class that does not represent a problem domain concept• something made up, to support• high cohesion,• low coupling, and• reuse
				- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202410151521699.png){:height 277, :width 357}
			- Indirection Pattern
			  collapsed:: true
				- Assign the responsibility to an intermediate object to mediate between other components or services so that they are not directly coupled.
				- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202410151522410.png){:height 219, :width 399}
			- Protected Variations
			  collapsed:: true
				- Identify points of predicted variation or instability; assign responsibilities to create a stable interface around them
			- Law of Demeter
			  collapsed:: true
				- the methods of a class should not depend in any way on the structure of any class, except the immediate(top-level) structure of their own class.
-
- System Analysis and Design
  collapsed:: true
	- Overview
	  collapsed:: true
		- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202410151616971.png){:height 297, :width 358}
	- 软件需求与需求获取
	  collapsed:: true
		- Software Requirements
		  collapsed:: true
			- Clear, concise, consistent and unambiguous, Expectations in terms of function, behavior, performance, design constraints, etc
			- good: completeness, correctness, feasibility, necessity, prioritization, unambiguity, verifiability
		- collapsed:: true
		  2. 需求的分类
			- Business Requirements: high-level objectives, vision and scope
			- User Requirements: functional and non-functional, external behavior
			- Functional Requirements, FR: functions and services
			- Non-Functional Requirements, NFR: quality and performance
			- Business Rule: Internal execution logic
			- Data defination
			- Constraints: belong to non-function, e.g. language
			- External Interface Requirement: How does the system interact with the external environment
		- 过程
		  collapsed:: true
			- Requirement Elicitation->Requirement Analysis->Software Requirement Specification, SRS->Requirement Verification->Requirement Management
		- collapsed:: true
		  3. 需求获取方法
			- 收集现有书面资料,面对面访谈,需求研讨会(Workshop),现场观察/体验,头脑风暴(Brainstorming)
		- collapsed:: true
		  4. 需求分析建模-事件和事物
			- 模型和建模
			  collapsed:: true
				- 数学模型、描述模型、图形模型
			- 事件和系统需求
			  collapsed:: true
				- 外部事件、临时事件、状态事件
				- 事件属于描述性模型，事件列表
					- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202410151638823.png){:height 184, :width 404}
			- 事物和系统需求
			  collapsed:: true
				- 事物：
				  collapsed:: true
					- 在传统的开发方法中，事物就是构成系统存储信息的相关数据
					- 在面向对象的开发方法中，事物就是在系统中相互交互的对象
				- 事件 – 发生在瞬间，有一定的随机性。事物 – 客观存在，不以主观意志为转移
				- 类型
				  collapsed:: true
					- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202410151644728.png)
				- 关系：基数（重数）
				- 事物的属性：属性，标示符（关键字），复合属性
				- 数据实体和对象
					- 数据实体：在传统的系统开发方法中，事物被称为数据实体
					- 对象：在面向对象的系统开发方法中，将事物称为对象
			- 实体-关系图ERD-传统
			  collapsed:: true
				- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202410151647462.png){:height 214, :width 277}
				- 关联实体：处理多对多关系
			- 类图-面向对象
	- 需求/系统分析：结构化分析
	  collapsed:: true
		- 结构化方法vs面向对象方法
		  collapsed:: true
			- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202410151719891.png){:height 233, :width 330}
			- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202410151720904.png){:height 169, :width 321}
			- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202410151720608.png){:height 163, :width 327}
		- 1.需求的结构化分析方法
		  collapsed:: true
			- 结构化分析方法(SA)：将待解决的问题看作一个系统，从而用系统科学的思想方法(抽象、分解、模块化)来分析和解决问题
			- 帮助开发人员定义系统需要做什么（处理需求），系统需要存储和使用哪些数据（数据需求），系统需要什么样的输入和输出以及如何把这些功能结合在一起来完成任务
			- 基于数据流的需求分析建模 -- DFD, 数据分析建模 -- ERD/IDEF1X图
		- 2.数据流图DFD
		  collapsed:: true
			- 用处理、外部实体、数据流以及数据存储来（DFD） 表示系统需求的图表
			  collapsed:: true
				- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202410151724972.png){:height 203, :width 409}
				- 外部实体发出的“处理请求”，即一个事件。指向“外部实体”的“数据流”一般是“处理”的反馈或处理结果
			- 特点
			  collapsed:: true
				- 图形元素少且符号简单易懂
				- 较充分表达系统的主要需求：输入、输出、处理和数据存储
				- 最终用户、管理人员和系统开发人员只需稍加培训即可读懂DFD图，方便交流
			- DFD建模
			  collapsed:: true
				- DFD图可以描述高层次的具有高度概括的系统处理也可以描述低层次的具有更详细分解的系统处理
				- 抽象层次：把系统分解成一个逐步细化的分层集合的建模技术
				- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202410151950711.png)
				- 关联图：
				  collapsed:: true
					- 在单个处理符号中概括系统内所有处理活动的DFD，数据存储不画在关联图中
				- DFD片段
				  collapsed:: true
					- 用一个单一处理符号表示系统响应一个事件的DFD
					- 在DFD片段中，展示了处理、外部实体和内部数据存储之间的交互细节
					- 每个DFD片段仅显示要响应该事件的相关的那些数据存储
					- 一个DFD片段是为事件表中的每一个事件创建的
				- 0层图：事件分离的系统模型/0层图
				- 1层DFD图：将0层DFD中的处理进一步细化等到的DFD图“处理”的编号为“i.j”
				- 2层DFD图：将1层DFD中的处理进一步细化等到的DFD图“处理”的编号为“i.j.k”
			- DFD的质量评估
			  collapsed:: true
				- 高质量的DFD：可读性强、内部一致、能够准确描述系统需求
				- 措施： 最小化复杂度 保证数据流一致性
				- 最小化复杂度：就是使每幅DFD图尽量简单易懂，避免信息超量
				- 构造DFD图的7±2规则：单个DFD中不应有超过7±2个处理 单个DFD中不应超过7±2个数据流进出同一个处理/数据存储
				- 接口最小化： DFD中各个元素之间的连接数越少越好
				- 保证数据流一致性
				  collapsed:: true
					- 一个“处理”和该“处理”被详细分解后在数据流内容上应该一致
					- 对一个“处理”，有数据流入则必须有相对应的数据流出
					- 对一个“处理”，有数据流出则必须有相对应的数据流入
				- 若输入数据流不完全用来产生输出数据流，称之为黑洞。若输出数据流不完全依赖于输入数据流，称之为奇迹
			- DFD细节内容描述
			  collapsed:: true
				- 1. “处理”细分解，层层分解，直到可详细描述细节
				- 2. 结构化语言/伪代码
				- 3. 决策表/决策树
		- 3.数据字典(DD)
		  collapsed:: true
			- 数据项定义，数据结构定义，数据流描述，数据存储描述
			- 数据处理（广义DD）
		- 4.数据分析(ERD、IDEF1X)
		  collapsed:: true
			- 关联实体：数据存储需求包括数据实体、数据实体的属性以及它们之间的关系
	- 需求分析：用户故事与用例建模
	  collapsed:: true
		- 1.敏捷开发中的User Story分析
		  collapsed:: true
			- User Story
			  collapsed:: true
				- 对软件用户（或所有者）有价值的功能性的简明的书面描述
				- Card+Conversation(正面)+Confirmation(背面)
				- As a [user role] I want to [goal] so I can [reason]
			- INVEST: Independent, Negotiable, Valuable, Estimatable, Small, Testable
			- 在写代码之前先写测试(Test-Driven Development, TDD)
			- 敏捷迭代计划
				- Story Point估算其工作量,团队成员分别估计,形成估算清单
				- 对用户故事排列优先级,安排责任人,汇聚为迭代计划并发布,开发过程中监控进度
		- 2.面向对象方法中的Use Case建模
		  collapsed:: true
			- Use Case: 站在用户角度定义软件系统的外部特性
			- 四大特征：sequences of actions(不可再分解),system performs,observable result of value,particular actor
			- 用例模型: Actor, Use Case, Communication Association
				- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202410151707345.png){:height 100, :width 305}
			- 优点:
				- 系统被看作是一个黑箱，并不关心系统内部是如何完成它所提供的功能的
				- 首先描述了被定义系统有哪些外部使用者(抽象为Actor)、这些使用者与被定义系统发生交互
				- 针对每一参与者，又描述了系统为这些参与者提供了什么样的服务(抽象成为Use Case)、或者说系统是如何被这些参与者使用的
				- 用例模型容易构建、也容易阅读
				- 完全站在用户的角度上，从系统外部来描述功能
				- 帮助系统的最终用户参与到需求分析过程中来，其需求更容易表达出来
		- 3.用例建模的基本过程
		  collapsed:: true
			- Step1:识别并描述参与者(actor)
			  collapsed:: true
				- 特殊的参与者：系统时钟
				- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202410151710286.png){:height 89, :width 293}
			- Step2:识别用例(use case),并给出简要描述
			  collapsed:: true
				- 每个用例至少应该涉及一个actor
				- 每个参与者也必须至少涉及到一个用例
			- Step3:识别参与者与用例之间的通讯关联(Association)
			- Step4:给出每一个用例的详细描述
			  collapsed:: true
				- 事件流:常规流+备选流
			- Step5:细化用例模型
			  collapsed:: true
				- – 参与者与参与者之间的泛化(generalization)
				- – 用例和用例之间的包含(include)
				- – 用例和用例之间的扩展(extend)
				- – 用例和用例之间的泛化(generalization)关系
			- note:用例的粒度,用例是actor与系统的交互,actor与外部系统的区分
		- 4.业务活动分析（Activity Diagram&泳道图）
		  collapsed:: true
			- 传统的活动图：只涉及一个参与者
			- 泳道图(swim-lane diagram)：侧重于描述多个参与者的活动之间的交互关系
			- 活动图的基本要素：– 起始点、结束点– 活动；决策点– 活动之间的时序连接；活动之间的并发点– 泳道
		- 5.用例建模的提交物
		  collapsed:: true
			- 用例模型,每个用例的详细描述,术语表：所用到的术语说明,补充规约：非功能性需求的说明
			- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202410151715351.png){:height 335, :width 429}
	- 系统分析：面向对象分析
	  collapsed:: true
		- 1.面向对象的分析方法概述
		  collapsed:: true
			- 分类
			  collapsed:: true
				- 功能模型：从用户的角度获取功能需求，由用例模型表示
				- 静态结构模型(分析对象模型)：描述系统的概念实体，由类图表示
				- 动态行为模型：描述对象之间的交互行为，由时序图和协作图表示
			- 过程
			  collapsed:: true
				- 业务领域分析
				- 发现和定义对象和类
				- 识别对象的外部联系
				- 建立系统的静态结构模型
				- 建立系统的动态行为模型
		- 2.建立静态结构模型
		  collapsed:: true
			- – Step 1：从用例模型入手，识别分析类；
			  collapsed:: true
				- – 实体类：表示系统存储和管理的持久性信息
				- – 边界类：表示参与者与系统之间的交互
				- – 控制类：表示系统在运行过程中的业务控制逻辑
			- – Step 2：描述各个类的属性；
			- – Step 3：定义各个类的操作；
			- – Step 4：建立类之间的关系；
			- – Step 5：绘制类图(class diagram)
		- 3.建立动态行为模型
		- 4.案例分析
	- 系统设计：结构化设计
	  collapsed:: true
		- 1.自顶向下的结构化模块层次及其接口设计
		  collapsed:: true
			- – 模块的层次化
			- – 模块之间的接口
		- 2.模块内部逻辑设计
		- 3.具有系统自动化边界的DFD
		  collapsed:: true
			- structure chart
			- 结构图定义：以模块为基础、以模块间的调用为关联所构成的图称模块结构图，简称结构图
			- 结构图的作用： 通过层次的方法来描述系统每部分的功能和子功能 展示计算机程序模块间的联系
			- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202410152133856.png){:height 289, :width 259}
		- 4.系统模块结构图
		  collapsed:: true
			- 从一个DFD片断建立一个结构图的步骤
			  collapsed:: true
				- 确定主要的信息流
				- 找出能代表输入和输出间最基本变化的过程
				- 重画数据流图并把输入放在左边，输出放在右边
				- 初步建立一个结构图草图
				- 加入其它模块实现下列功能– 数据输入、数据处理、数据输出
				- 使用结构化语言或决策树添加模块间逻辑
				- 进一步求精
			- 结构图的创建方法
			  collapsed:: true
				- 转换分析方法
				  collapsed:: true
					- – “处理”对输入到输出的转换（I-P-O）
					- – 结构图包括输入子树、计算子树和输出子树
					- – 用数据流图片断作为输入
				- 事务分析方法
				  collapsed:: true
					- – 含有“处理”分支的情况
					- – 某“处理”对输入数据流进行分析，根据分析结果选择不同的“处理”
			- DFD到系统结构图转换的基本模式
			  collapsed:: true
				- 简单变换型DFD→系统结构图转换
				  collapsed:: true
					- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202410152138381.png){:height 217, :width 313}
					- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202410152138121.png){:height 189, :width 315}
				- 复杂变换型DFD→系统结构图转换
				  collapsed:: true
					- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202410152139006.png){:height 251, :width 339}
					- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202410152140370.png){:height 195, :width 342}
				- 事务型DFD→系统结构图转换
				  collapsed:: true
					- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202410152140416.png){:height 155, :width 329}
					- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202410152141666.png){:height 261, :width 342}
					- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202410152141257.png){:height 234, :width 349}
					- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202410152142494.png){:height 268, :width 341}
		- 5.结构化分析与设计案例
	- 系统设计：面向对象设计
	  collapsed:: true
		- 1.面向对象设计概述
		- 2.OO分析到系统设计的转换
		- 3.包的设计
		- 4.对象设计
		- 5.基于UML的面向对象分析与设计案例
	- 系统设计：数据库设计
	  collapsed:: true
		- 1.数据库系统及关系数据库简介
		  collapsed:: true
			- 数据库系统=数据库(DB)＋数据库管理系统(DBMS)
			- 关系型数据库管理系统将数据存储成表和关系的结构
			- 概念ERD设计->逻辑ERD设计->物理ERD设计
			  collapsed:: true
				- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202410152108457.png){:height 181, :width 394}
		- 2.数据库逻辑模型设计
		  collapsed:: true
			- 1. 识别所有“自然”数据实体（Entity） 来自DFD中的全部“数据存储” 来自“分析类图”中的部分“类”
			- 2. 为数据实体命名
			- 3. 给出实体的属性
			- 4. 识别实体之间的关联关系及关联重数 关联关系需命名 关联重数需判别（1:1、1:M、M:N）
			- 5. 建立关联实体（“人造”实体）来消除M:N重关系
			- 6. 实体规范化（一般需满足3NF）
			- 7. 评价ERD质量并做必要的改进
		- 3.ERD模型及质量评价
		  collapsed:: true
			- 至少符合3NF Normal Form
			- – 1NF – 没有重复的属性或属性组
			- – 2NF – 是1NF 且每个非主属性均函数依赖于主属性(主键)
			- – 3NF – 是2NF且非主属性间均不存在函数依赖
		- 4.物理数据库设计及建立
		  collapsed:: true
			- 1. 为每个ERD实体创建一个二维表（即数据库Table）
			- 2. 根据ERD中的实体属性，在对应的Table中定义对应的字段（Field），并给出适当的数据类型和取值范围
			- 3. 定义每个Table的主键(PK：PrimaryKey)
			- 4. 针对1:M关联关系的子表添加外键（FK：ForeignKey）即对应主表的主键
			- 5. 定义完整性约束
		- 5.物理数据库提高效率的技巧
		  collapsed:: true
			- (1) 降低范式、增加冗余；少用触发器、多用存储过程
			- (2) “体外运算”
			- (3) 水平/垂直分割表
			- (4) 对数据库管理系统DBMS进行系统优化，即优化各种系统参数，如缓冲区个数
			- (5) 在使用面向数据的SQL语言进行程序设计时，尽量采取优化算法
		- 6.数据库的不同体系结构模型
		  collapsed:: true
			- 单数据库服务器体系结构,带备份的数据库服务器体系结构,将数据库模式划分为多个客户访问子集,分区数据库服务器体系结构,联合数据库服务体系结构
		- 7.OO分析类图映射到ERD
		  collapsed:: true
			- persistent data
			- 类型与适用范围
			  collapsed:: true
				- 数据文件:存储大容量数据、临时数据、低信息密度数据
				- 数据库:并发访问要求高、系统跨平台、多应用程序使用相同数据
				- 关系数据库: – 复杂的数据查询– 数据集规模大
				- 面向对象数据库: – 数据集处于中等规模– 频繁使用对象间联系来读取数据
			- Object Relational Mapping，ORM
			  collapsed:: true
				- 1 : 1
				- 1 : *
				- * : *
		- 8.关系型数据库规范化实例
	- 系统设计：用户界面设计
	  collapsed:: true
		- collapsed:: true
		  1. 用户界面设计的基本概念
			- HCI-Human-Computer Interface，User Interface
			- 以用户为中心、方便用户、易用高效
			- 用户界面结构设计（采用低保真的原型）
			- 用户界面的交互设计（采用高保真的原型）
		- collapsed:: true
		  2. 用户界面形式及设计原则
			- （1）用户界面分类：CUI → GUI → MUI
			  collapsed:: true
				- Character User Interface
				  collapsed:: true
					- 容易设计，效率高
					- 界面死板单调，记忆量大,不宜非专业人员使用
				- Graphics User Interface
				  collapsed:: true
					- 直观、美观、易用
					- 实现复杂，资源耗费大
				- Multimodal User Interface
				  collapsed:: true
					- 智能化，人性化，使用更方便
					- 资源耗费大，成本高，不易普及
				- 从应用软件功能角度分类： 操作系统用户界面 文字处理软件用户界面 工业控制系统用户界面 管理信息系统用户界面 网页用户界面 移动工具用户界面 游戏软件用户界面
			- （2）用户界面风格
			- （3）用户界面形式及设计原则
			  collapsed:: true
				- 形式
				  collapsed:: true
					- 命令语言界面
					- 菜单界面
					- 数据输入界面
					- Direct Manipulation和WIMP界面
					- 提示、帮助和出错界面
				- 原则
				  collapsed:: true
					- 1. 一致性原则2. 提供信息提示与反馈 [反例]3. 合理利用空间、保持界面简洁4. 合理利用颜色与显示效果5. 实现内容与形式的统一6. 恰当使用图形和形象比喻7. 用户出错宽容性8. 尽量提供快捷方式9. 尽量允许操作可逆性（Undo与Redo）10.尽量减少用户的记忆性要求11.合理的系统响应和低的系统成本12.良好的联机帮助（Help功能）
		- collapsed:: true
		  3. WIMP用户界面设计Windows, Icons, Menus, Pointers/Windows, Icons, Mouse, Pull-down menu
			- 1. 窗口设计
			- 2. 菜单设计
			- 3. 填表输入界面设计
			- 4. 操作控制元素设计
			- 5. 声音输出界面设计
			- 6. 图标设计
			- 7. 个性化界面设计
			- 8. UnDo/ReDo设计,环形栈
		- collapsed:: true
		  4. 网页界面设计
			- collapsed:: true
			  1. 网页界面的构成、特性及形式
				- 网页界面的构成要素:文字,图形,页面版式,色彩,多媒体
				- 网页界面的特性:1.主体经济性： 2.手段先进性： 3.对象广泛性： 4.内容丰富性： 5.形式多样性： 6.操作交互性
				- 网页界面的主要形式:1.信息查询类 2.大众媒体类 3.宣传窗口类 4.电子商务类 5.交流平台类 6.网络社区类
			- collapsed:: true
			  2. 网页界面中文字的编排设计
				- 2.1 文字编排的规律2.2 文字群体的编排
			- 3. 网页界面中版式设计
			- 4. 网页界面色彩搭配
			- 5. 网页设计之“三”原则
			- 6. 网页界面设计中提高下载速度的考虑
		- collapsed:: true
		  5. 缺省设计问题
			- 减少用户输入数据量，提高系统使用效率
			- 缺省值的设定一定要考虑使用的效率，即从统计结果看，每次输入平均输入量（含清除缺省值的输入量） ＜ 无缺省值情况下的平均输入量。
		- collapsed:: true
		  6. 输入验证设计问题
			- 输入验证的内容及方法
			  collapsed:: true
				- 数据位数验证,字符集验证,范围验证,合理性验证,数据格式验证,安全性验证,异常性验证
			- 操作错误处理的机制及方法
			  collapsed:: true
				- 选择性确认,操作中断允许,回滚,Undo/Redo
		- collapsed:: true
		  7. 系统响应及信息反馈问题
			- 目的
			  collapsed:: true
				- 系统响应: 及时响应,适时响应（不能过快/过慢）
				- 信息反馈: 反馈信息具体、准确, 人性化反馈
			- 系统响应的内容及方法
			  collapsed:: true
				- 立即确认用户的输入（在0.1秒内）
				- 提供忙状态指示（对处理时长在1-10秒数量级）
				- 任务进度指示器（处理时间在数10秒以上）
				- 首先反馈重要信息
				- 处理任务分解、化整为零
			- 信息反馈的内容及方法
			  collapsed:: true
				- 反馈信息明确,建设性指导原则与积极的基调,以用户为中心的措辞,用非拟人化的设计
-
-
- Web Application development technology
	- overview
	  collapsed:: true
		- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202410152146217.png){:height 251, :width 471}
	-
-
- Software Architecture and Design Patterns
	- overview
	  collapsed:: true
		- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202410152201739.png){:height 178, :width 391}
	- design patterns
		- pattern classification
			- Creational Patterns: 将对象的创建和使用解耦
			- Structural Patterns: 结构型模式是将不同功能解耦
			- Behavioral Patterns: 行为型模式是将不同的行为解耦
		- Creational Patterns
		  collapsed:: true
			- 单例模式：某个类只能生成一个实例
			  collapsed:: true
				- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202410152238213.png){:height 157, :width 362}
			- 原型模式：对象的创建由原来对象克隆完成
			  collapsed:: true
				- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202410152239181.png){:height 160, :width 377}
			- 工厂方法模式：由专门的工厂生产对象
			  collapsed:: true
				- Simple Factory Method Pattern
				  collapsed:: true
					- 分离变化点
					- 优点：
					  – 选择创建对象的逻辑被放在了工厂类里面
					  – 客户类Client不用自己创建对象
					  – 责任分离 (Responsibility separation)
					- • 缺点：
					  – 在产品类层次结构中添加新的子类需要修改工厂类的源代码，即，需要在工厂方法里面的代码再增加一个条件语句，因此违背了“开闭原则”。
					- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202410152212408.png)
				- Factory Method Pattern
				  collapsed:: true
					- 去掉if...else
					  collapsed:: true
						- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202410152214867.png)
					- 优点：
					  – 用户只需要知道具体工厂的名称就可得到所要的产品，无须知道产品的具体创建过程；
					  – 在系统增加新的产品时只需要添加具体产品类和对应的具体工厂类，无须对原工厂进行任何修改，满足开闭原则；
					- • 缺点：
					  – 每增加一个产品就要增加一个具体产品类和一个对应的具体工厂类，这增加了系统的复杂度。
				- compare
				  collapsed:: true
					- 中心不同 Different Centers
					  – 简单工厂方法模式的中心是具体的工厂类.
					  – 工厂方法模式的中心是抽象工厂超类 (或接口类)
					- • 工厂方法不同
					  – 简单工厂的工厂方法是一般是静态的。
					  – 工厂模式中，工厂方法不是静态的，并且分布在各个具体的工厂子类里面
					- • 工厂模式支持开闭原则，但是简单工厂不支持开闭原则
			- Abstract Factory Pattern：由专门的工厂生产产品家族
			  collapsed:: true
				- 是一种为访问类提供一个创建一组相关或相互依赖对象的接口，且访问类无须指定所要产品的具体类就能得到同族的不同等级的产品的模式结构。
				- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202410152237814.png)
				- 产品不同
				  – 对于工厂方法模式，产品是单个产品类层次结构；
				  – 对于抽象工厂模式，产品是一组产品类层次结构。
				- • 扩展性
				  – 工厂方法模式遵循开闭原则
				  – 抽象工厂模式仅部分遵循开闭原则
			- 建造者模式：通过装配方式构造对象
			  collapsed:: true
				- 建造者（Builder）将一个复杂对象的构建与表示分离。即：将一个复杂的对象分解为多个简单的对象，然后一步一步构建而成。它将变与不变相分离，即产品的组成部分是不变的，但每一部分是可以灵活选择的
				- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202410152241344.png)
		- Structural Patterns
		  collapsed:: true
			- Proxy：为其他对象提供一种代理以控制对这个对象的访问。在某些情况下，一个对象不适合或者不能直接引用另一个对象，而代理对象可以在客户端和目标对象之间起到中介的作用。
			- Adapter：两个不兼容接口之间适配的桥梁
			  collapsed:: true
				- 类适配器
				  collapsed:: true
					- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202410152245001.png)
				- 对象适配器
				  collapsed:: true
					- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202410152246814.png)
				- compare
				  collapsed:: true
					- 类适配器模式实现方式：定义一个适配器类来实现当前系统的业务接口，同时又继承现有组件库中已经存在的组件
					- 对象适配器模式实现方式：将现有组件库中已经实现的组件引入适配器类中，该类同时实现当前系统的业务接口。
			- Bridge：相同功能抽象与实现解耦，抽象与实现可以独立升级
			  collapsed:: true
				- 将两个变化的维度分开设计，可以使各维度独立地，互不影响地增加类，并满足开闭原则
				  collapsed:: true
					- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202410152248148.png)
					- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202410152248333.png)
				- 优点：
				  collapsed:: true
					- 解耦：将抽象部分和实现部分分离，使它们可以独立地变化。实现部分聚合到抽象部分，可以在运行时动态配置
					- 扩展性：容易添加新的抽象部分和实现部分，扩展系统的功能。
					- 可维护性：修改抽象部分或实现部分的代码不会对另一部分产生影响，易于维护。
				- 使用场景：多维度扩展，避免类爆炸，增加灵活性
			- Decorator：向一个现有的对象添加新的功能，同时又不改变其结构
			- Facade：向现有的系统添加一个接口，客户端访问此接口来隐藏系统的复杂性
			- Flyweight：尝试重用现有的同类对象，如果未找到匹配的对象，则创建新对象
			- Composite：相似对象进行组合，形成树形结构
		- Behavioral Patterns
		  collapsed:: true
			- 类行为模式 vs 对象行为模式
			  collapsed:: true
				- 类行为模式：采用继承机制来在类间分派行为
				- 对象行为模式：采用组合或聚合在对象间分配行为
				- 由于组合关系或聚合关系比继承关系耦合度低，满足“合成复用原
				  则”，所以对象行为模式比类行为模式具有更大的灵活性。
			- Template Method：父类定义算法骨架，某些实现放在子类
			- Strategy：每种算法独立封装，根据不同情况使用不同算法策略
			  collapsed:: true
				- 该模式定义了一系列算法，并将每一个算法封装起来，使它
				  们可以相互替换，且算法的变化不会影响使用算法的客户
				- 算法的责任和算法的实现分割开来
				- 满足开闭原则、依赖倒置原则
				- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202410152256414.png)
				- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202410152256147.png)
				- 使用场景：
				  collapsed:: true
					- – 一个系统需要动态地在几种算法中选择一种时，可将每个算法封装到策略类中。
					- – 一个类定义了多种行为，并且这些行为在这个类的操作中以多个条件语句的形式出现，可将每个条件分支移入它们各自的策略类中以代替这些条件语句。
					- – 系统中各算法彼此完全独立，且要求对客户隐藏具体算法的实现细节时。
			- State：每种状态独立封装，不同状态内部封装了不同行为
			  collapsed:: true
				- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202410152315384.png)
				- 优点
				  collapsed:: true
					- – 解决switch-case、if-else带来的难以维护的问题，这个很明显，没什么好说的；
					- – 结构清晰，提高了扩展性，不难发现，Context类简洁清晰了，扩展时，几乎不用改变，而且每个状态子类也简洁清晰了，扩展时也只需要极少的改变。
				- 缺点
				  collapsed:: true
					- – 随着状态的扩展，状态类数量会增多，这个老生常谈了，几乎所有解决类似问题的设计模式都存在这个缺点；
					- – 增加了系统复杂度，使用不当将会导致逻辑的混乱，因为，状态类毕竟增多了嘛，而且还涉及到状态的转移，思维可能就更乱了；
					- – 不完全满足开闭原则，因为扩展时，除了新增或删除对应的状态子类外，还需要修改涉及到的相应状态转移的其它状态类。
				- 应用场景
				  collapsed:: true
					- 一个操作中含有庞大的分支结构，并且这些分支决定于对象的状态时，就可以考虑使用状态模式
				- 重复创建问题：享元模式
				  collapsed:: true
					- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202410152317991.png)
				- ![Replaced by Image Uploader](https://raw.githubusercontent.com/qugushihua/blog-images/master/202410152317279.png)
			- Command：将一个请求封装为一个对象，使发出请求的责任和执行请求的责任分割开
			- Chain of Responsibility：所有处理者封装为链式结构，依次调用
			- Observer：维护多个观察者依赖，状态变化通知所有观察者
			- Mediator：取消类/对象的直接调用关系，使用中介者维护
			- Iterator：定义集合数据的遍历规则
			- Visitor：分离对象结构，与元素的执行算法
			- Memento：把核心信息抽取出来，可以进行保存
			- Interpreter：定义语法解析规则
	- software architecture
-
- Software Development Process and Project Management
-
- Software Testing and Quality Assurance