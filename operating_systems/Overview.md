- [[W2N1 - OS Structure|Types of OS]]
	- [[W2N1 - OS Structure#Monolithic OS|Monolithic]]: all OS operations occur within one kernel. Pros: low cost as no overhead from switching between components. Cons: difficult to maintain or modify. Used widely. Can be extended using kernel modules, which are compiled in or loaded at execution time.
	- [[W2N1 - OS Structure#Layered OS|Layered]]: many layers, each presenting an interface to the layer above. Pros: each layer can be independently verified. Cons: The hierarchical structure doesn't hold, and crossing layers has a cost. Most OSes uses a hardware abstraction layer, otherwise sees little use.
	- [[W2N1 - OS Structure#Microkernel OS|Microkernel]]: kernel contains only the very basics: virtual memory, security, communication. Everything else is run in userspace. Pros: components are isolated, easy to extend or customise. Cons: worse performance as kernel boundary is crossed more frequently.
- [[W2N2 - Devices|Devices]] are every peripheral that isn't the CPU or main memory
	- Can use **IO ports** which need privileged instructions to interact
	- Or **memory mapping**, where each register or buffer on that device has its own memory address that is not backed by RAM
	- **Direct memory access (DMA) controllers** allow the CPU to transfer a large amount of data from a device directly into memory without passing through the CPU, which wastes cycles
- [[W3N1 - Processes|Processes]] are an abstraction for execution and contain all the information required to resume a program that is executing
	- Linux stores that information in the [[W3N1 - Processes#The PCB in Linux|process control block (PCB)]].
	- Processes execute until they are stopped by an interrupt or they issue a system call. At this point a **context switch** occurs, and the OS takes over execution, performs an action, and returns control to a program.
	- A process is either running, ready to run, or blocked waiting for an event to occur. The OS stores the list of processes in each of these states in state queues, one for each state, and one for each type of wait.
	- [[W3N1 - Processes|UNIX creates processes]] when a parent calls the `fork` syscall, which makes a copy of the parent address space. The fork will usually then [[W3N1 - Processes#Fork and exec|exec]] another program, replacing the current process with the new program.
	- [[W3N2 - Interprocess communication|Processes communicate]] between each other in one of two ways:
		- **Shared memory**: a section of both processes address spaces are shared. Pros: fast access as the OS isn't involved past setting up the shared space. Cons: requires the application programmer to fully manage data transfer.
		- **Message passing**: the processes use syscalls to send messages via OS provided syscalls. Pros: simpler. Cons: slower, less flexibility.
			- Pipes allow a parent and child process to communicate using a pair of file descriptors. Writing to one can be read from the other.
			- Sockets are endpoints for network/internal communications.
			- Signals are one way notifications that come from the OS from errors or timers, or from other processes.
	- [[W4N1 - Threads|Threads]] exist within processes, and have a private stack and registers, but share an address space and OS resources. This allows for easier communication between related threads.
		- A process may contain 1 or more threads, but a thread is part of exactly one process. Threads are the unit of scheduling.
		- [[W4N2 - Managing threads#Kernel-level threading|Kernel-level threading]] gives the kernel control over individual threads, with thread information is stored in the [[W4N1 - Threads#Thread control block (TCB)|Thread control block (TCB)]]. Pros: the OS can schedule threads individually, and not schedule them when they are blocked for IO. Cons: expensive due to context switching.
		- [[W4N2 - Managing threads#User-level threading|User-level threading]] uses a userspace library to manage threads. Pros: can be 10-100x faster than kernel-level threading, gives precise control to the application program. Cons: The kernel doesn't know that multiple threads are running, and may block the whole process if any of those threads are waiting for IO.
	- [[W4N3 - Scheduling basics|Scheduling]] is the process of deciding which thread runs next. Threads tend to consist of cycles of CPU bursts and IO bursts, so a thread doesn't need to be scheduled when its waiting for IO.
		- There are 3 classes of schedulers: batch schedulers aim to maximise throughput or utilisation, interactive schedulers aim to minimise response times, real time schedulers must guarantee processes have below maximum latencies or response times.
		- **Preemptive scheduling** can interrupt processes involuntarily, **non-preemptive scheduling** waits for a process to complete, voluntarily yield, or make a blocking call.
		- There are many scheduling algorithms, including:
			- [[W4N4 - Scheduling algorithms#First-come first-served (FCFS)|FCFS]]: processes run in the order they arrive
			- [[W4N4 - Scheduling algorithms#Shortest job first (SJF)|SJF]]: the shortest program is run first, this can be either preemptive - if a short job arrives while the running process has longer time remaining the new job gets swapped in (also known as SRTN) - or non-preemptive - once a job is scheduled it can't be interrupted.
			- [[W4N4 - Scheduling algorithms#Round robin (RR)|RR]]: each job runs for a preset amount of time called a quantum. After the quantum elapses the job is preempted, and the next process gets to run.
			- [[W4N4 - Scheduling algorithms#Priority (PRIO)|PRIO]]: each process has a priority, and the highest priority runs first. This can be static (each process is assigned a priority by the developer or OS when it starts) or dynamic (calculated at runtime, e.g. based on the ratio of execution to waiting that process does, so IO bound processes take priority)
			- [[W4N4 - Scheduling algorithms#Multiple queues (MQ)|MQ]]: a separate ready queue is maintained for each priority, with each priority queue being run in RR fashion.
			- [[W4N4 - Scheduling algorithms#Multilevel feedback queue (MLFQ)|MLFQ]]: like MQ except each queue has a different quantum (lower priority get higher quantas) and processes start with the highest priority, and get moved to lower queues when they exceed their quantum. A process is moved to a higher priority when it becomes interactive
- [[W8N2 - Memory management|Memory management]] is the process of assigning specific parts of memory to different processes
	- Memory is accessed by processes with a [[W6N2 - Virtual memory|virtual address]], which is translated to the corresponding physical address on RAM.
	- Virtual memory is divided into equal size [[W6N1 - Non-contiguous memory allocation#Paging|pages]], and physical memory is divided into equal size frames.
	- A virtual address consists of a page number and an offset, and to translate virtual to physical only the page number must be mapped to a frame number using a page table in the MMU.
	- The page table contains an invalid bit, when it is set it means that the page is not in memory. In this case, the MMU raises a page fault exception, which prompts the OS to check if the page is [[W6N2 - Virtual memory#Demand paging|swapped to disk]], and if so swaps it into memory and potentially evicts another page to disk.
	- There are a number of page replacement algorithms to decide which page gets evicted to disk:
		- [[W6N2 - Virtual memory#First-in-first-out (FIFO)|FIFO]]: the oldest page in memory is replaced
		- [[W6N2 - Virtual memory#Least recently used (LRU)|LRU]]: the least recently accessed page is replace
		- [[W6N2 - Virtual memory#Second chance|Second chance]]: similar to FIFO, but if a page is used it is sent to the back of the queue
	- Each process has a [[W6N3 - Multiprocess page management#Working sets|working set]], the number of pages that a process has accessed in the last $n$ memory accesses.
	- The number of pages a program can have swapped into memory can be determined by choosing a [[W6N3 - Multiprocess page management#Page-fault frequency allocation|PFF]], then measuring the actual rate of page faults and increase the number of pages when the process exceeds that rate, and decrease it when it falls below it
	- [[W6N3 - Multiprocess page management#Thrashing|Thrashing]] occurs when a system is spending most of its tie servicing page faults instead of doing work. This can be due to insufficient memory, or a badly suited page replacement algorithm.
- [[W7N1 - Secondary storage|Secondary storage]] is slow, non-volatile memory such as HDDs or SSDs used for long-term storage of data
	- [[W7N1 - Secondary storage#Hard disk drives|HDDs]] consist of spinning platters divided into tracks, which are divided into sectors. Sectors are addresses using a logical block address, which is mapped onto each sector sequentially across all tracks and platters.
		- [[W7N1 - Secondary storage#Disk performance|HDD performance]] is dependent on seek time, rotational latency, and transfer time.
		- The order of reads and writes to an HDD is scheduled using [[W7N1 - Secondary storage#Block scheduling|block scheduling algorithms]]:
			- FCFS: same as in process scheduling
			- SSTF: the next operation is the one with the shortest seek time from the current head location
			- SCAN: travel right to left, then left to right, serving requests as they come under the head.
			- C-SCAN: same as SCAN, but only service requests when travelling rightwards.
	- [[W7N1 - Secondary storage#Solid state drives|SSDs]] consist of an array of NAND flash, and flash controllers to read and write to them.
		- SSDs are significantly faster than HDDs.
		- SSDs are written a page at a time from the OS, but the physical flash cells are written at a block (64-256 pages) at a time. A cell can only be erased a certain number of times before it fails, so the SSD will use an onboard controller to cache writes and balance the wear between blocks.
	- Data in secondary storage is organised in files according to a [[W8N1 - Filesystems|filesystem]].
		- A file consists of metadata, about the owner/protection/access times etc, and the desired stored data.
		- There are many types of files, some for filesystem organisation, such as symlinks and directories, and some for alphanumeric or binary data that is used by programs or the OS.
		- Files usually have [[W8N1 - Filesystems#File protection|protection information]], which describes who can access a file and what they can do to it.
		- The OS keeps a number of [[W8N1 - Filesystems#File system data structures|datastructures]] in various places to maintain the filesystem
			- [[W8N1 - Filesystems#Inodes|Inodes]] are index nodes, which contain metadata along with the physical location of a file's blocks on disk. The number of inodes on a disk is allocated when the disk is formatted, meaning there is a maximum number of files that can be stored on a disk
			- An inode contains a number of [[W8N1 - Filesystems#Inode block lists|block pointers]], along with single, double, and triple indirect pointers.
- When multiple processes/threads try to access the same resources, access must be [[W9N1 - Synchronisation|synchronised]], to ensure data consistency.
	- A [[W9N1 - Synchronisation#Critical regions|critical region]] is an area of a program where shared data is accessed, and only one thread/process may run within any given critical region at any one time to ensure no race conditions occur.
	- Critical regions are protected using [[W9N1 - Synchronisation#Mutual exclusion (Mutexes)|mutexes]]
		- [[W9N1 - Synchronisation#Locks|Spinlocks]] use atomic instructions to enter a critical region, and simply spins until the region becomes available. Spinlocks waste CPU time, so are used to build more complex synchronisation methods.
		- [[W10N1 - Semaphores#Semaphores|Semaphores]] maintain a list of sleeping threads, and allow threads to yield their running time until they can enter the critical section.
		- [[W10N2 - Monitors|Monitors]] are a programming language construct which automatically manage access to an object.
- [[W11N1 - Virtualisation|Virtualisation]] is the process of creating a virtual version of real hardware, which can then run a full OS and its applications.
	- A virtual system has a hypervisor/VMM which manages the host resources and distributes them to guest machines.
	- Virtualisation allows multiple OSes to share the same hardware at the same time, isolates different guests from each other, and can [[W11N1 - Virtualisation#Migration|migrate]] running guests between hosts with no down time
	- There are [[W11N1 - Virtualisation#Methods of virtualisation|two approaches to virtualisation]]:
		- Binary translation: the VMM examines each instruction for the guest, and determines the result
		- Trap-and-emulate: the VMM configures the host CPU to trap to it when privileged instructions are executed, then directly runs the guest code on the host machine.
	- [[W11N1 - Virtualisation#Paravirtualisation|Paravirtualisation]] uses modified guest OSes that know they are being virtualised, and allows them to directly request privileged operations from the VMM. It is faster than the other options, but requires modified guest OSes.