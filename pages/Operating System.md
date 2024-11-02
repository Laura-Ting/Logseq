- 不全：https://zhangt.top/CS/Operating-System-Study-Notes/
- 很全：https://www.cnblogs.com/Roshin/p/WangDao_OS_notes.html
- ## Chapter 1--Operating System Overview
  collapsed:: true
	- ### concepts, functions, and goals
	  collapsed:: true
		- concept
		  collapsed:: true
			- Operating System (OS): controlling and managing the **hardware and software resources** of the entire computer system, and rationally **organizing and scheduling** computer work and resource allocation.
			- To provide users and other software with convenient interfaces and environments; it is **the most basic system software** in the computer system.
			- Offline command = batch command
			- Online command = interactive command
			- Internal commands: A collection of system-defined, memory-resident handlers
		- operating system hierarchy
		  collapsed:: true
			- Users can interact with the operating system through direct commands or through the program interface provided by the operating system.
			- ![操作系统层次结构](https://s2.loli.net/2022/06/17/VFmrxdDX5iT8UJP.png){:height 223, :width 346}
	- ### Four characteristics: concurrency, sharing, virtuality, and asynchronous
	  collapsed:: true
		- Concurrency: the existence of multiple programs running at the same time in a computer system
		- Sharability: the resources in the system can be used by **multiple concurrently executing processes** in the memory.
		- Virtuality: Space division multiplexing (virtual memory)+Time division multiplexing (virtual processor)
		- Asynchronous: the execution of the process is not consistent,advancing at an unpredictable speed.
		- **Concurrency and sharing complement each other**
	- ### Operating system development and classification
	  collapsed:: true
		- **manual operation**
		  collapsed:: true
			- Disadvantages: The user monopolizes the entire machine, and the conflict between human and machine speeds results in low resource utilization.
		- **Single batch processing**
		  collapsed:: true
			- Introducing **offline input/output** technology, and **the supervisory program** is responsible for controlling the input and output of the job
			- advantage: alleviates **the contradiction between human and machine speed** to a certain extent, and improves resource utilization.
			- shortcoming: Only **one program can be run** in the memory, and the next program can only be loaded after the program is finished running. **The CPU spends a lot of time idle waiting for I/O to complete** . Resource utilization remains low.
		- **time-sharing operating system**
		  collapsed:: true
			- The computer **takes turns serving each user/job in units of time slices** , and each user can interact with the computer through the terminal.
			- advantage: User requests can be responded to immediately, **solving human-computer interaction problems** . Allows multiple users to use a computer at the same time, and users can operate the computer independently of each other without feeling the presence of others.
			- shortcoming:**Some urgent tasks cannot be prioritized** . The operating system is completely fair to each user/job and serves each user/job for a time slice cyclically without distinguishing the urgency of the task.
			- feature: Multiplexing, independence, interactivity, timeliness
		- **real-time operating system**
		  collapsed:: true
			- the computer system processes external signals in a timely manner after receiving them.
			- And **the incident must be handled within strict time limits** .  **timeliness and reliability**
			- advantage: Able to respond to some urgent tasks with priority, and some urgent tasks do not require time slots
			- Classification:
				- Hard real-time systems that must be completed within absolutely strict time limits
				- Soft real-time systems, accepting occasional violations of time regulations
		- network operating systems
		- distributed operating systems: **distribution and parallelism**
	- ### OS operating mechanism and architecture
	  collapsed:: true
		- #### Instructions and operating system status
		  collapsed:: true
			- **Instructions** : The most basic commands that the processor can recognize and execute
			  collapsed:: true
				- Privileged command: User is not allowed to use
				- unprivileged instructions
			- Two processor states, marked by the program status register (PSW), 0 user, 1 core
			  collapsed:: true
				- User mode (eye mode): The CPU can only execute unprivileged instructions
				- Core state (management state): Privileged instructions and non-privileged instructions can be executed
			- two procedures
			  collapsed:: true
				- Kernel program: system management, both instructions can be executed, **running in the kernel state**
				- Application: Can only run unprivileged instructions, **executed in user mode**
		- #### operating system kernel
		  collapsed:: true
			- **The kernel** is the underlying **software** configured on the computer and is the most basic and core part of the operating system;
			- Those programs that implement the kernel functions of the operating system are **kernel programs**
			- **Kernel functions**
			  collapsed:: true
				- close to hardware: Clock management, interrupt handling, primitives (device driver, CPU switching)
				- Close to the upper level: Process management, memory management, device management, etc... (divided into different operating systems)
		- #### operating system architecture
		  collapsed:: true
			- macrokernel:
			  collapsed:: true
				- all the main functional modules of the operating system are used as the system kernel and run in the core state
				- Advantages: High performance
				- Disadvantages: The kernel code is huge, the structure is confusing, and it is difficult to maintain
			- Microkernel:
			  collapsed:: true
				- Keep only the most basic functions in the kernel
				- Advantages: Fewer kernel functions, clear structure, easy to maintain
				- Disadvantages: Need to frequently switch between kernel mode and user mode, low performance
	- ### Interrupts and exceptions
	  collapsed:: true
		- **Essence:** **An interruption** means that **the operating system needs to intervene and start management work**
		  collapsed:: true
			- When an interrupt occurs, the CPU immediately enters core state
			- After an interrupt occurs, the currently running process is suspended and the operating system kernel handles the interrupt.
			- Different interrupt signals will be processed differently.
		- Interrupt
		  collapsed:: true
			- switch between user mode and core mode: Switching is achieved through **interrupts** , and is the **only** way to change **the program status word (PSW).**
			- Interrupt classification
			  collapsed:: true
				- **Internal** interrupts (**exception**, exception, trap), come from **within the CPU** and are **related to the currently executing instructions.**
				  collapsed:: true
					- trap: Access instructions used when making system calls
					- fault: Hardware failure --- page missing
					- abort: Software failure --- divide by 0, unrecoverable fatal error
				- **External** interrupt (interrupt), comes from **outside the CPU** and has **nothing to do with the currently executed instruction.**
					- Peripheral request---Signaling completion of I/O operation
					- Manual intervention---The user forcibly terminates the process, kill
			- Handling of external interrupts
			  collapsed:: true
				- After each instruction is executed, the CPU checks whether there is an external interrupt
				- save context
				- Transfer to the corresponding interrupt handler according to the interrupt type
				- Restore the CPU environment of the original process and exit the interrupt, return to the original process and continue execution.
	- ### System calls
	  collapsed:: true
		- what is system call
		  collapsed:: true
			- The program interface provided by the operating system to the user is composed of a set of system call programs. Applications **request services from the operating system through system calls** . Various shared resources in the system are uniformly managed by the operating system, so in the user program, all resource-related operations (such as storage allocation, V/o operations, file management, etc.) must be provided to the operating system through system calls.
			- The service request is completed by the operating system. This **can ensure the stability and security of the system** and prevent users from illegal operations.
			- System call related processing is processed in **the core state**
			- Applications can either directly make system calls or call library functions to make system calls.
		- **Calling process**
		  id:: 670f73bd-b95a-4a3e-b066-0207e9711125
			- Pass system call parameters
			- Execute trapped instructions ( **user mode** )
			  collapsed:: true
				- **The trapped instruction** is executed in **the user mode** . **An internal interrupt** is triggered immediately after executing the trapped instruction, and the CPU enters **the core mode.**
				- The request is made in **user mode** , and the corresponding processing is in **core mode.**
				- It is the only instruction that can only be executed in user mode and **cannot be executed in core mode.**
			- Execute system call corresponding service program ( **core state** )
			- Return to user program
- ## Chapter 2--Process
  collapsed:: true
	- Process & Thread
	- ### definition, composition, organization, characteristics
	  collapsed:: true
		- #### process definition
		  collapsed:: true
			- **Program** : a sequence of instructions
			- **the process entity (process image)** : three parts ** the program segment, data segment, and PCB**
			- **PCB is the only sign of the existence of the process!**
			- from different perspectives: **the dynamic nature** of the process
			  collapsed:: true
				- A process is an execution of a program.
				- A process is the activity that occurs when a program and its data are executed sequentially on a processor.
				- A process is a process in which a program with independent functions runs on a data collection. It is an independent unit for resource allocation and scheduling in the system.
				- **A process is the running process of a process entity** and **an independent unit for resource allocation and scheduling in the system** .
		- #### Process composition
		  collapsed:: true
			- **Process Control Block** PCB (storage data of operating system management process)
			  collapsed:: true
				- Process description information
				  collapsed:: true
					- **PID** , unique, process identifier
					- **UID** , user identifier
				- Process control and management information
				  collapsed:: true
					- Current status
					- process priority
				- resource allocation list
				  collapsed:: true
					- Program segment ptr
					- data segment ptr
					- Keyboard, mouse, etc.
				- Processor related information
				  collapsed:: true
					- The values ​​of various registers record the current running status of the process, such as which PC value has been executed.
			- Program segment: stores the code that needs to be executed
			- Data segment: stores various data processed during the running of the program
		- #### process organization
		  collapsed:: true
			- **Link** method
			  collapsed:: true
				- Divide PCB into multiple queues according to process status
				- The operating system has pointers to each queue
			- **Index** mode
			  collapsed:: true
				- Create several index tables based on different process statuses
				- The operating system holds pointers to various index tables
		- #### Characteristics of the process
		  collapsed:: true
			- **Dynamic** : A process is an execution process of a program, which is generated, changed and destroyed dynamically.
			- **Concurrency** : There are multiple process entities in the memory, and each process can be executed concurrently
			- **Independence** : A process is a basic unit that can run independently, obtain resources independently, and accept scheduling independently.
				- **Process is the basic and independent unit for resource allocation and scheduling.**
			- **Asynchronous** : Each process advances at an independent and unpredictable speed. The operating system must provide a "process synchronization mechanism" to solve asynchronous problems.
			- **Structural** : Each process is configured with a PCB. Structurally, the process consists of PCB, program segment, and data segment.
	- ### Process status and transitions
	  collapsed:: true
		- #### process status
		  collapsed:: true
			- **New**, the process is being created, the operating system allocates resources to the process and initializes the PCB
			- **Running**, occupies the CPU and executes on the CPU
			- **Ready**, the running conditions are already met, there is no idle CPU and it is not running yet
			- **Blocking**, temporarily unable to run due to waiting for a certain period of time
			- **Terminated**, the process is being withdrawn from the system, the operating system will reclaim the resources owned by the process and clear the PCB
		- #### process switching
		  collapsed:: true
			- ![进程转换](https://s2.loli.net/2022/06/17/kShMogPuKf2Lmje.png)
	- ### Process control
	  collapsed:: true
		- goal:
		  collapsed:: true
			- to effectively manage all processes in the system. It has the functions of creating new processes, canceling existing processes, and realizing process state transitions.
			- ![进程控制](https://s2.loli.net/2022/06/17/3w1VMhz5Rs8kHYL.png)
		- **atomic operation**
		  collapsed:: true
			- How to ensure that PSW modifications are not preempted?
			  collapsed:: true
				- Use **primitives** to implement process control. The characteristic of the primitive is that **it does not allow interruption during execution** and can only be completed in one go. This kind of operation that cannot be interrupted is **an atomic operation** . The primitive is implemented by **"turning off interrupt instructions" and "turning on interrupt instructions".**
			- ```
			  // ...
			  关中断指令
			  原语 code 1
			  原语 code 2
			  开中断指令
			  // ..., 
			  ```
			- the permissions of **the off/on interrupt instructions** are very large, and they must be **privileged instructions** that can only be executed in **the core state** .
		- **Process control primitive core** :
		  collapsed:: true
			- Update information in PCB (such as modifying process status flags, saving the running environment to PCB, restoring the running environment from PCB)
			  collapsed:: true
				- All process control primitives must modify process status flags
				- Depriving the currently running process of CPU usage rights necessarily requires saving its running environment.
				- Before a process starts running, the operating environment must be restored
			- Insert the PCB into the appropriate queue
			- Allocate/recycle resources
		- **Process creation**
		  collapsed:: true
			- Create primitives
			  collapsed:: true
				- Request a blank PCB
				- Allocate required resources to new process
				- Initialize PCB
				- Insert PCB into ready queue
			- The time that caused the process to be created
			  collapsed:: true
				- User login---In the time-sharing system, if the user logs in successfully, the system will create a new process for him.
				- Job scheduling --- In a multi-channel batch processing system, when a new job is put into the memory, a new process will be created for it.
				- Providing services --- When the user makes certain requests to the operating system, a new process will be created to handle the request.
				- Application request---The user process actively requests the creation of a child process
		- **process terminated**
		  collapsed:: true
			- undo primitive
			  collapsed:: true
				- Find the PCB that terminated the process from the PCB collection
				- If the process is running, immediately deprive the CPU and allocate the CPU to other processes.
				- Terminate all its child processes
				- Return all resources owned by the process to the parent process or the operating system
				- Remove PCB
			- Events that cause process termination
			  collapsed:: true
				- End normally, abnormal end, External spring intervention
		- **Process blocking and waking up** should be used in pairs
		  collapsed:: true
			- process blocking
				- blocking primitive
				  collapsed:: true
					- Find the PCB corresponding to the process to be blocked
					- Protect the process running site, set the PCB status information to "blocked state", and temporarily stop the process running.
					- Insert the PCB into the waiting queue for the corresponding event
				- Events that cause process blocking
				  collapsed:: true
					- Need to wait for the system to allocate some kind of resource
					- Need to wait for other cooperating processes to complete their work
			- The awakening of the process
			  collapsed:: true
				- wake primitive
					- PCB found in event waiting queue
					- Remove the PCB from the waiting queue and set the process to ready state
					- Insert the PCB into the ready queue, waiting to be scheduled
				- Events that cause a process to wake up
					- Waiting for the event to occur
		- **Process switching**
		  collapsed:: true
			- switch primitive
			  collapsed:: true
				- Store operating environment information in PCB
				- PCB moves into corresponding queue
				- Select another process to execute and update its PCB
				- The operating environment required to restore the new process based on the PCB
			- Events that cause process switching
			  collapsed:: true
				- The current process time slice is up
				- A higher priority process arrives
				- The current process is automatically blocked
				- Current process terminates
	- ### Process communication
	  collapsed:: true
		- background
		  collapsed:: true
			- **the memory address spaces owned by each process are independent of each other** .
			- To ensure security, **one process cannot directly access the address space of another process** .
			- But information exchange between processes must be implemented. In order to ensure secure communication between processes, the operating system provides some methods.
		- #### shared storage
		  collapsed:: true
			- **Access** to shared space by two processes must be **mutually exclusive**
				- Based on data structure
					- For example, only one array with a length of 10 can be placed in the shared space. This sharing method is slow, has many restrictions, and is a **low-level communication method.**
				- Based on bucket
					- draw a shared storage area in the memory. The form and storage location of the data are controlled by the process rather than the operating system. In comparison, this sharing method is faster and is an **advanced communication method** .
		- #### pipe communication
		  collapsed:: true
			- Pipes **can only use half-duplex communication** , and can only achieve one-way transmission within a certain period of time. If you want to **achieve two-way simultaneous communication, you need to set up two pipes** .
			- Each process must **have mutual exclusive access to the pipe**
			- Data is fed into the pipeline in the form of a stream
			  collapsed:: true
				- When **the pipe is full** , **the write() system call of the writing process will be blocked** , waiting for the reading process to retrieve the data.
				- When the reading process has taken away all the data, **the pipe becomes empty** , and **the read() system call of the reading process will be blocked** .
			- Once the data is read, it is discarded from the pipe, which means that **there can only be one reading process at most** , otherwise the wrong data may be read.
		- #### messaging
		  collapsed:: true
			- in **formatted messages**, The process **exchanges data through the two primitives "send message/receive message" provided by** the operating system.
			- **Message structure**
			  collapsed:: true
				- Message header, including: sending process ID, receiving process ID, message type, message length and other formatting information
				- message body
			- **Delivery method**
			  collapsed:: true
				- Direct delivery, the message is directly hung on the message buffer queue of the receiving process
				- In indirect transmission, the message must first be sent to the intermediate entity (mailbox), so it is also called "mailbox communication method". Eg: E-mail system in the computer network
	- ### Threads and multithreading
	  collapsed:: true
		- #### Thread basics
		  collapsed:: true
			- A thread is a **basic CPU execution unit and the smallest unit of program execution flow** .
			- After the introduction of threads, not only processes can be concurrent, but **also threads within the process can be concurrent** , which further **improves the concurrency of the system** and enables various tasks (such as QQ videos, text chats, etc.) to be processed concurrently within a process. , transfer files)
			- After the introduction of threads, **the process only serves as the allocation unit of system resources other than the CPU** (such as printers, memory address spaces, etc. are all allocated to the process)
			- That is, **the process is the basic unit of resource allocation, and the thread is the basic unit of scheduling.**
			  collapsed:: true
				- Each thread has a thread ID, thread control block (TCB)
				- **Switching of the same thread does not require switching process environment**
				- Threads own almost no system resources\
				- Processes in the same thread share the resources of the process, and communication does not require system intervention.
		- #### Thread implementation
		  collapsed:: true
			- User-Level Thread, ULT
			  collapsed:: true
				- User-level threads are implemented by applications through thread libraries.
				- All **thread management work is handled by the application (including thread switching)**
				- In user-level threads, **thread switching** **can be completed in user mode** without operating system intervention.
				- From the user's perspective, there are multiple threads. But from the perspective of the operating system kernel, it is not aware of the existence of threads. (User-level threads are opaque to the user and transparent to the operating system)
			- Kernel-Level Thread, KLT
			  collapsed:: true
				- **The management of kernel-level threads** is completed by **the operating system kernel** .
				- The kernel is responsible for thread scheduling, switching and other tasks, so **the switching of kernel-level threads** must be completed in **the core state** .
			- **Only kernel-level threads are the unit of processor allocation**
		- #### multi-threaded model
		  collapsed:: true
			- Many-to-one model:
			  collapsed:: true
				- Multiple users and threads are mapped to one kernel-level thread. Each user process corresponds to only one kernel-level thread.
				- Advantages: Switching of user-level threads can be completed in user space without switching to core state. Thread management has low system overhead and high efficiency.
				- Disadvantages: When a user-level thread is blocked, the entire process will be blocked, and the concurrency is not high. Multiple threads cannot run in parallel on multi-core processors
			- One-to-one model:
			  collapsed:: true
				- A user and thread are mapped to a kernel-level thread. Each user process has the same number of kernel-level threads as user-level threads.
				- Advantages: When a thread is blocked, other threads can continue to execute, and **the concurrency capability is strong** . Multithreading can be executed in parallel on a multi-core processor.
				- Disadvantages: A user process will occupy multiple kernel-level threads. Thread switching is completed by the operating system kernel and needs to be switched to the core state. Therefore, **the cost of thread management is high and the overhead is large** .
			- Many-to-many model:
			  collapsed:: true
				- nn Users and threads are mapped to mm kernel-level threads (n≥m)(n≥m) . Each user process corresponds to mm kernel-level threads.
				- It overcomes the shortcomings of low concurrency in the many-to-one model, and also overcomes the shortcomings of the one-to-one model in which one user process occupies too many kernel-level threads and causes too much overhead.
	- Scheduling
	- ### Conceptual hierarchy of processor scheduling
	  collapsed:: true
		- #### Three levels of scheduling
		  collapsed:: true
			- **Advanced Scheduling (Job Scheduling)**
			  collapsed:: true
				- Just care about how to tune in, and automatically tune out when finished
				- Due to limited memory space, sometimes it is not possible to put all the jobs submitted by the user into the memory. Therefore, it is necessary to determine some kind of rule to determine the order in which the jobs are transferred into the memory.
				- According to certain principles, select one (or more) jobs from the jobs in the backup queue on external storage and allocate necessary resources such as memory to them.
				- And **establish the corresponding process (establish PCB)** so that it (them) **obtains the right to compete for the processor** .
				- Advanced scheduling is the scheduling between **auxiliary storage (external storage) and memory** . Each job is loaded only once and loaded once. **The corresponding PCB will be created when the job is loaded in, and the PCB will be canceled when the job is loaded out** .
				- Advanced scheduling mainly refers to the problem of calling in, because only the timing of calling in needs to be determined by the operating system, but the timing of calling out must be called out after the job is finished.
			- **Intermediate scheduling (memory scheduling)**
			  collapsed:: true
				- It is to decide which suspended process will be brought back into memory.
				- A process may be called out and into memory multiple times, so intermediate scheduling **occurs more frequently than high-level scheduling** .
				- After the introduction of virtual storage technology, processes that are temporarily unable to run can be transferred to external memory to wait. Wait until it meets the operating conditions again and the memory is slightly free, then load it into the memory again.
				- The purpose of this is to **improve memory utilization and system throughput** .
				- The status of the process temporarily transferred to the external memory to wait is suspended. The PCB will not be transferred to external memory at the same time, but will be **resident in memory** .
				- The PCB will record the storage location of the process data in the external memory, process status and other information. The operating system passes the PCB in the memory.
				- To maintain monitoring and management of each process. The suspended process **PCB will be placed in the suspension queue** .
			- **Low-level scheduling (process scheduling)**
			  collapsed:: true
				- Its main task is to select a process from the ready queue according to a certain method and strategy and assign the processor to it
				- Process scheduling is **the most basic kind of scheduling in operating systems.** Process scheduling must be configured in general operating systems.
				- **The frequency of process scheduling is very high** , usually once every tens of milliseconds.
		- #### Comparison of seven-state model and three types of scheduling
		  collapsed:: true
			- ![七状态模型](https://s2.loli.net/2022/06/17/7kt5z4C9IyRmfvh.png)
			- ![三种调度对比](https://s2.loli.net/2022/06/17/CDxriQuqfMaUAdP.png)
	- ### Process scheduling timing, switching process, and method
	  collapsed:: true
		- #### Process scheduling timing
		  collapsed:: true
			- give up **voluntarily**
			  collapsed:: true
				- Process terminates normally
				- An exception occurred during operation and terminated.
				- Process actively requests blocking (i/o)
			- **passively** give up
			  collapsed:: true
				- The time slice allocated to the process is exhausted
				- There are more urgent things to deal with (I/O)
				- A process with a higher priority enters the queue
			- **Cannot proceed**
			  collapsed:: true
				- **When handling an interrupt**
				- The process is in **the critical section of the operating system kernel program**
					- Critical resources: Only one process is allowed to use them in a period of time, and processes need mutually exclusive access.
					- Critical section: the section of code that accesses critical resources
					- **The critical section of a kernel program** is generally used to access **certain kernel data structures** , such as the process's ready queue.
					- Processes can perform process scheduling and switching when accessing **common critical sections.**
				- in **atomic operations**
		- #### Process scheduling method
		  collapsed:: true
			- **Non-deprivation scheduling method** , also known as **non-preemptive method** .
			- **Deprivation scheduling mode** , also known as **preemption mode** .
		- #### Process switching and process
		  collapsed:: true
			- **Process scheduling in a narrow** sense refers to **selecting a process to run** from the ready queue. (This process can be the process that has just been suspended. It may also be **another process** . In the latter case, **process switching** is required) **Process switching** refers to the process in which one process gives up the processor and another process takes over the processor.
			- **Generalized process scheduling** includes two steps: selecting a process and process switching.
			- The process of process switching is mainly completed:
			  collapsed:: true
				- Save various data of the original running process
				- Recovery of various data for new processes
			- **Process switching comes at a cost.** Therefore, if process **scheduling and switching** are performed **too frequently** , **the efficiency of the entire system will inevitably be reduced** , so that most of the system's time is spent on process switching, and the time actually used to execute the process reduce
	- ### Evaluation indicators of scheduling algorithms
	  collapsed:: true
		- **CPU utilization**
		- **System throughput**
			- Number of jobs completed per unit time
		- **turnaround time** 4 part
		  collapsed:: true
			- the time interval **from when the job is submitted to the system to when** **the job is completed**
			- The time a job waits for job scheduling (advanced scheduling) on **​​the external storage backup queue** ,
			- The time the process waits on the ready queue for process scheduling (low-level scheduling),
			- The time the process executes on the CPU,
			- The time the process waits for the I/O operation to complete.
		- **waiting time**
		  collapsed:: true
			- For processes, it is the total waiting time on the processor
			- For jobs, we also need to calculate the time the job waits in the external memory waiting queue.
		- **response time**
		  collapsed:: true
			- The time from when a user **submits a request** to **the first response**
	- ### FCFS, SJF, HRRN
	  collapsed:: true
		- #### First come, first served (FCFS)
		  collapsed:: true
			- Thoughts: First come, first served
			- Rules: Serve on a first-come, first-served basis
			- Job/process scheduling: Both can be used. Job scheduling considers who gets to the back queue first, and process scheduling considers who gets to the ready queue first.
			- Preemption: **non-preemptive**
			- Advantages: Fair, simple to implement
			- Disadvantages: **Good for long jobs, bad for short jobs**
			- Hunger: Does not cause hunger
			- Waiting time = Turnaround time - Run time - I/O operation time
		- #### Shortest jobs first (SJF), shortest remaining time first (SRTN)
		  collapsed:: true
			- Idea: Pursue the minimum average waiting time, average turnaround time, average weighted turnaround time
			- Rule: The shortest job/process will be served first ( **the shortest service time** )
			- Job/process scheduling: Both can be used. When used for processes, it is called the " *Shortest Process First* (SPF)" algorithm.
			- Preemption: The default **is non-preemptive** , and there is also **a preemptive type---Shortest Remaining Time First** algorithm (SRTN, *Shortest Remaining Time Next* )
			- Pros: "Shortest" average wait time, turnaround time
			- Disadvantages: **Good for short jobs, bad for long jobs**
			- Hunger: **Hunger occurs**
		- **Shortest remaining time first (SRTN)**
		  collapsed:: true
			- **Scheduling is required whenever a process joins the ready queue and changes** . If **the remaining time** of the newly arrived process **is shorter** than the remaining time of the currently running process,
			- The new process **will seize** the processor, and the currently running process will return to the ready queue. In addition, **scheduling is also required when a process completes**
		- #### High response ratio priority algorithm (HRRN)
		  collapsed:: true
			- Idea: Sum taking into account waiting time and requested service time
			- Rule: Calculate **the response ratio** of each job/process every time it is scheduled, and select **the highest** one to serve
			- **response ratio** = 等待时间+要求服务时间 / 要求服务时间
			- Job/process scheduling: can be used
			- Preemption: **Non-preemptive** , the currently running job/process actively gives up the processor before scheduling
			- Advantages: Comprehensive consideration, combines the advantages of SJF and FCFS
			- Disadvantages: None
			- Hunger: Does not cause hunger
		- Summary of the three algorithms, **none of them are suitable for interactive systems** !
		  collapsed:: true
			- ![作业调度算法总结](https://s2.loli.net/2022/06/17/6HXCvEiGgfSlmFh.png)
	- ### Interactive system scheduling algorithm: time slice rotation (RR), priority, multi-level feedback queue
	  collapsed:: true
		- #### Time slice rotation (RR)
		  collapsed:: true
			- Idea: Fair round-robin service, each process gets a response within a certain time interval
			- Rules: According to the order of each process arriving in the ready queue, each process will take turns to execute a **time slice** (such as 100ms). If the time slice ends and the execution is not completed, the processor will be deprived of it and placed at the end of the queue.
			- Job/process scheduling: **process scheduling** , the time slice can be allocated only after the job is placed in the memory and the process is established.
			- Preemption: **Preemptive** , the clock device generates **a clock interrupt** to rotate the time slice
			- Advantages: Fair, fast response, suitable for time-sharing operating system
			- Disadvantages: High frequency of process switching, no distinction between task urgency
			- Hunger: Does not cause hunger
			- problem
				- If **the time slice is too large** , it will **degrade to FCFS** , **which will increase the process response time.**
				- If **the time slice is too small** and **processes are switched frequently** , it will take a lot of time to switch processes.
			- Generally speaking, the time slice is designed so that the overhead of switching processes **does not exceed 1%.**
		- #### priority scheduling algorithm
		  collapsed:: true
			- Idea: Schedule tasks according to their urgency
			- Rules: Each task has its own priority, and the one with the highest priority is selected when scheduling.
			- Job/process scheduling: both are possible, and can even be used for I/O scheduling
			- Preemption: **Either preemptive or non-preemptive. In** preemptive mode, if the ready queue changes, it may need to be scheduled. Otherwise, the processor time scheduling will be actively given up.
			- Advantages: Priority distinguishes urgency, suitable for real-time operating systems
			- Disadvantages: There will be starvation if there are always high priority entries.
			- hunger: **causes hunger**
			- note
			  collapsed:: true
				- With the same priority, choose the one that came in first.
				- Priority can be changed dynamically:
				  collapsed:: true
					- Static priority: After the process is created, the priority remains unchanged.
					- Dynamic priority: The created process has an initial value, which is then dynamically adjusted according to the situation.
				- usual strategy
				  collapsed:: true
					- Usually system processes have higher priority **than** user processes
					- The foreground process has higher priority **than** the background process
					- The operating system **prefers I/O-type processes (I/O-busy processes)** , as opposed to **computing-type processes (CPU-busy processes)** . The two can run in parallel, and it is better to choose to perform I/O earlier.
				- dynamic strategy
				  collapsed:: true
					- If a process **has been waiting in the ready queue for a long time** , its priority can **be increased** appropriately.
					- If you find that a process **frequently performs I/O operations** , you can **increase** its priority appropriately.
		- #### Multi-level feedback queue scheduling
		  collapsed:: true
			- Idea: Compromise considerations for other algorithms
			- Set up a multi-level ready queue, with priorities **from high to** low and time slices **from small to large**
			- When a new process arrives, **it first enters the first-level** queue and queues up to wait for the time slice according to the FCFS principle. If the time slice is used up and the time slice has not ended, it enters the end of the next-level queue. If it is already in the lowest-level queue, it is placed back at the end of the queue.
			- Only when **the k-th level** queue is empty, the time slice will be allocated to the k+1-level queue.
			- Job/process scheduling: **used for process scheduling**
			- Preemption: **preemptive** , preemptive algorithm. During the running of the process in the k-level queue, if a new process enters the higher-level queue (level 1~k-1), the new process will seize the processor because it is in a queue with a higher priority. , **the originally running process is put back to the end of the k-level queue** .
			- Advantages: It combines the advantages of all previous scheduling algorithms, and for CPU-intensive processes, I/O-intensive processes can adjust the preference (for example: put processes blocked by I/O back into the original queue, so as to ensure I/O O process maintains higher priority)
			- Disadvantages: No obvious disadvantages
			- Hunger: **causes hunger**
		- ![调度算法知识回顾](https://s2.loli.net/2022/06/17/1MAzRbtZfT4olEu.png)
	- synchronization
	- ### Process synchronization, mutual exclusion
	  collapsed:: true
		- **Synchronization** / **direct restriction relationship**
		  collapsed:: true
			- two or more processes established to complete a task, and these processes need to **coordinate** their **work order** in some position. The direct constraint between processes stems from their mutual cooperation.
		- **indirect restriction relationship**
		  collapsed:: true
			- **critical resources**: resources **that are only allowed to be used by one process within a period of time**
			- Many physical devices (e.g. cameras, printers), many variables, data, memory buffers, etc.
			- **Process mutual exclusion** means that when a process accesses a critical resource, another process that wants to access the critical resource must wait. The access of the process currently accessing the critical resource ends and after the resource is released,Only another process can access critical resources.
		- **process mutual exclusion principle**
		  collapsed:: true
			- Let in when free. When the critical section is idle, a process that requests to enter the critical section can be allowed to enter the critical section immediately;
			- If you are busy, wait. When an existing process enters the critical section, other processes trying to enter the critical section must wait;
			- Limited waiting for the process requesting access should ensure that it can enter the critical section within a limited time (to ensure that it will not starve):
			- Let the power wait. When a process cannot enter the critical section, the processor should be released immediately to prevent the process from being busy waiting.
	- ### Software implementation method of process mutual exclusion
	  collapsed:: true
		- #### single mark method
		  collapsed:: true
			- Idea: After two processes **access the critical section,** they will transfer the permission to use the critical section to the other process. That is to say, **the permission of each process to enter the critical section can only be granted by the other process.**
			- ```
			  // P0:
			  {
			      while (turn != 0);
			      critical section;
			      turn = 1;
			      remainder section;
			  }
			  // P1:
			  {
			      while (turn != 1);
			      critical section;
			      turn = 1;
			      remainder section;
			  }
			  ```
			- The access sequence must be `01010101...` , when the critical section is idle and 0 does not access the critical section, `turn == 0` , 1 needs to access but cannot enter.
			- **Does not satisfy the mutual exclusion principle of free entry**
		- #### double mark first check method
		  collapsed:: true
			- Check first, lock later
			- ```
			  bool flag[2];       // flag[i] = true 表示 i 进程想要进入临界区
			  flag[0] = false;
			  flag[1] = false;        
			  
			  // P0:
			  {
			      while (flag[1]);
			      flag[0] = true;
			      critical section;
			      flag[0] = false;
			      remainder section;
			  }
			  // P1:
			  {
			      while (flag[0]);
			      flag[1] = true;
			      critical section;
			      flag[1] = false;
			      remainder section;
			  }
			  ```
			- If two processes 0 and 1 are executed concurrently, when P0 ends the while loop and flag[0] has not been copied, a process switch occurs, and P1 can also enter the critical section. **If the busy-then-wait principle is not satisfied** .
		- #### Double mark post-check method
		  collapsed:: true
			- Lock first, check later
			- ```
			  bool flag[2];       // flag[i] = true 表示 i 进程想要进入临界区
			  flag[0] = false;
			  flag[1] = false;        
			  
			  // P0:
			  {
			      flag[0] = true;
			      while (flag[1]);
			      critical section;
			      flag[0] = false;
			      remainder section;
			  }
			  // P1:
			  {
			      flag[1] = true;
			      while (flag[0]);
			      critical section;
			      flag[1] = false;
			      remainder section;
			  }
			  ```
			- After P0 first executes `flag[0] = true` , a process switch occurs, and P[1] executes `flag[1]` . In the end, neither process can enter the critical section.
			- **Does not satisfy the mutual exclusion principles of idle let-in and finite wait**
		- #### Peterson
		  collapsed:: true
			- Take the initiative to let the other party enter the critical area first
			- ```
			  bool flag[2];
			  int turn = 0;
			  // P0
			  void funcp0(){
			      flag[0] = true;
			      turn = 1;
			      while (flag[1] && turn == 1) ;
			      critical section;
			      flag[0] = false;
			      remainder section;
			  }
			  
			  // P1
			  void funcp1(){
			      flag[1] = true;
			      turn = 0;
			      while (flag[0] && turn == 0) ;
			      critical section;
			      flag[1] = false;
			      remainder section;
			  }
			  ```
			- The process is **busy waiting** and does not satisfy the mutual exclusion principle **of yield waiting** .
	- ### Hardware implementation method of process mutual exclusion
	  collapsed:: true
		- #### Interrupt masking method
		  collapsed:: true
			- Using **on/off interrupt instructions**
			- Advantages: simple and efficient
			- Disadvantages: Not applicable to multiprocessors, only applicable to operating system kernel processes, not user processes (because the on/off interrupt instructions can only run in kernel mode, this set of instructions would be dangerous if users can use them at will)
		- #### TestAndSet directive
		  collapsed:: true
			- Turn locking and checking operations into one-stop atomic operations
			- **implemented in hardware**
			  collapsed:: true
				- ```
				  bool TestAndSet (bool* lock) {
				      bool old;
				      old = *lock;        // 获得lock 旧值
				      *lock = true;
				      return old;
				  }
				  
				  int main(){
				      // TSL logical
				      while (TestAndSet);
				      临界区代码
				      lock = false;
				      剩余区代码
				  }
				  ```
			- Advantages: Simple to implement, suitable for multi-processor environment
			- Disadvantage: **not satisfied with the right to wait** , lock = true, another process will be busy waiting
		- #### Swap command
		  collapsed:: true
			- ```
			  bool old = true;
			  while (old == true)
			      swap(&lock, &old);
			  临界区代码段...
			  lock = false;
			  剩余区代码段...
			  ```
			- Advantages: Simple to implement, suitable for multi-processor environment
			- Disadvantage: **not satisfied with the right to wait** , lock = true, another process will be busy waiting
	- semaphore
	- ### Semaphore mechanism
	  collapsed:: true
		- A semaphore represents the amount of a certain resource in the system
		- A pair of primitives: **wait(S) and signal(S)** , called **P(S), V(S)** operations
		- #### integer semaphore
		  collapsed:: true
			- Integer variable, representing the quantity of a certain resource
			- ```
			  int S = 1;
			  void wait(int S) {
			      while (S <= 0);
			      S --;
			  }
			  
			  void signal(int S) {
			      S ++;
			  }
			  // P0
			  {
			      wait(S);
			      使用资源...
			      signal(S);
			  }
			  ```
			- Disadvantages: **Busy waiting will occur** , **and the right-waiting principle is not satisfied** .
		- #### record semaphore
		  collapsed:: true
			- ```
			  typedef struct {
			      int value;          // 剩余资源数
			      struct process *L;  // 等待队列
			  } semaphore;
			  
			  void wait (semaphore S) {       // 原语操作
			      S.value --;
			      if (S.value < 0) 
			          block (S.L);        // 资源数不够, 把进程挂到S的等待队列中, 进入阻塞态
			  }
			  
			  void signal(semaphore S) {      // 原语操作
			      S.value ++;
			      if (S.value <= 0) {
			          wakeup(S.L);        // 释放资源后, 还要进程在等待, 就将等待队列中阻塞的进程唤起
			      }
			  }
			  ```
			- Advantages: Processes that have not applied for resources will be suspended and enter **the blocking state** , so **busy waiting (waiting for satisfaction)** will not occur, and all mutual exclusion principles are met.
	- ### Semaphore realizes process mutual exclusion, synchronization, and precursor relationship
	  collapsed:: true
		- Treat the critical section resource as a special resource and set the mutex **semaphore** mutex with an initial value of 1
		- Note
		  collapsed:: true
			- **Different mutually exclusive semaphores** need to be set for **different critical resources.**
			- **P, V operations must appear in pairs**
		- #### Process synchronization
		  collapsed:: true
			- Set **the synchronization semaphore S, initially 0**
			- **Execute V(S) after "previous operation"**
			- **Execute P(S) before "post-operation"**
			- ```
			  Semaphore S = 0;
			  
			  void P1(){
			      code 1;
			      code 2;
			      V(S);
			      code 3;
			  }
			  
			  void P2(){
			      P(S);
			      code 4;
			      code 5;
			      code 6;
			  }
			  ```
		- #### Process precursor relationship
		  collapsed:: true
			- ![前驱关系](https://s2.loli.net/2022/06/17/b6h1E8t52dMs9TF.png)
	- ### Example of semaphore solving synchronization mutual exclusion problem
	  collapsed:: true
		- #### producer-consumer problem
		- #### Multiple producer-consumer problem
		- #### smoker problem
		- #### reader-writer problem
		- #### Philosophers' dining problem
	- ### Pipeline
	  collapsed:: true
		- Semaphores: Difficult to program, error-prone
		- A monitor is a special software module that consists of:
		  collapsed:: true
			- Description of **shared data structures** local to the monitor:
			- A set of procedures that operate on this data structure;
			- Statement that sets initial values ​​for shared data local to the monitor:
			- A monitor has a name.
		- Basic characteristics of tube processes:
			- Data local to the monitor can only be accessed by procedures local to the monitor:
			- A process can enter the monitor to access shared data only by calling a process within the monitor;
			- **Only one process is allowed to execute an internal procedure within the monitor at a time** .
			- Actually similar to OOP
		- code
		  collapsed:: true
			- ```
			  monitor ProducerConsumer{
			      condition full, empty;
			      int count = 0;
			  
			      void insert(Item item) {
			          if (count == N)
			              wait(full);
			          count ++;
			          insert_item(item);
			          if (count == 1)
			              signal(empty);
			      }
			  
			      Item remove() {
			          if (count == 0)
			              wait(empty);
			          count --;
			          if (count == N - 1)
			              signal(full);
			          return remove_item();
			      }
			  }
			  end monitor;
			  
			  void produce() {
			      while (1) {
			          item = 生产一个产品;
			          ProducerConsumer.insert(item);
			      }
			  }
			  
			  void consume() {
			      while (1) {
			          item = ProducerConsumer.remove();
			          消费产品 item;
			      }
			  }
			  ```
		- **Set condition variables and wait/wake up operations in the monitor to solve synchronization issues**
		- **The compiler is responsible for the mutual exclusion process of entering the monitor**
	- deadlock
	- ### The concept of deadlock
	  collapsed:: true
		- #### deadlock definition
		  collapsed:: true
			- **Deadlock** : A phenomenon in which processes wait for each other's resources, causing each process to be blocked and unable to move forward.
			- **Starvation** : A process cannot move forward due to the long-term lack of desired resources. For example: in the short process first (SPF) algorithm, if there is a steady stream of short processes coming, the long process will never be able to get a processor, resulting in long process "starvation"
			- **Infinite loop** : The phenomenon that a process cannot break out of a loop during execution. Sometimes it's caused by program logic bugs, sometimes it's designed intentionally by programmers.
			- ![死锁，饥饿，死循环区别](https://s2.loli.net/2022/06/17/LaxB21AN3IlhZKO.png)
		- #### Necessary conditions for deadlock to occur(must all meet)
		  collapsed:: true
			- **Mutually exclusive conditions** : Only competition for resources that must be used mutually exclusive will lead to deadlock (such as philosopher's chopsticks, printer equipment).
			- **Non-deprivation condition** : The resources obtained by the process cannot be forcibly taken away by other processes before they are used up, and can only be actively released.
			- **Request and hold conditions** : The process has maintained at least one resource, but it makes a new resource request, and the resource is occupied by another process. At this time, the requesting process is blocked, but it keeps its own resources.
			- **Circular waiting condition** : There is a circular waiting chain for process resources, and the resources obtained by each process are simultaneously requested by the next process.
			- **When a deadlock occurs, there must be a loop waiting** However, it is not necessarily deadlock when circular waiting occurs ( **cyclic waiting is a necessary but not sufficient condition for deadlock** ).
		- When does a deadlock occur?
		  collapsed:: true
			- **Competition for system resources** . The competition between processes for **inalienable resources** (such as printers) may cause deadlocks. Source (CPU) competition will not cause deadlock.
			- **The process advancement sequence is illegal. Improper order of requesting and releasing resources can also lead to deadlock** .
			- **Improper use of semaphores can also cause deadlocks.**
			- **In short, irrational allocation of inalienable resources may lead to deadlock** .
		- #### Deadlock handling strategy
		  collapsed:: true
			- Prevent deadlocks. Break one or more of the four necessary conditions for deadlock.
			- Avoid deadlocks. Use some method to prevent the system from entering an unsafe state, thereby avoiding deadlock (Banker's Algorithm)
			- Deadlock detection and relief. Deadlock is allowed to occur, but the operating system will be responsible for detecting the occurrence of deadlock and then taking some measures to relieve the deadlock.
	- ### Deadlock handling strategy---prevention of deadlock
		- #### destroy mutex
			- **SPOOLing technology**
				- Logically transform the exclusive device into a shared device, transfer the output to the output process, and the process side regards the output (printer) as completed
				- Disadvantages: small scope of application
		- #### Destroy the conditions of non-deprivation
			-
-