TODO
# Working sets
The **working set (WS)** of a process is the set of pages that the process currently "needs". We can define it as
$$
w(k,t)=\left\{\text{pages referenced in the time interval }(t-k,t)\right\}
$$
- $t$: time
- $k$: working set window, measured in page references
- A page is in the WS only if it was referenced in the last k references

![[w6n3workingSetOverTime.png]]
The working set size varies over the lifetime of a program.

![[w6n3workingSetSizeGraph.png]]
The working set must all be in memory, otherwise heavy faulting occurs. 
# Page-fault frequency allocation
We can determine the number of frames to be allocated to a program by establishing an acceptable **page-fault frequency (PFF)** rate. We then measure the actual PFF rate of the process, and if it is too low then we decrease the number of frames allocated, and if it is too high then the process gains frames.
# Thrashing
Thrashing occurs when the system is spending most of its time servicing page faults instead of doing useful work. This could be that there is enough memory but a poor replacement algorithm which is incompatible with program behaviour, or it could be that memory is over-committed, leading to the OS seeing the CPU is being poorly utilised and starts more programs, resulting in many active processes requesting memory.
![[w6n3thrashing.png]]
# In Linux
TODO