# Operating-System-Bytes


### Operating System
Its a system software which works as a interface between user and hardware.
Hardware - I/O Devices, CPU

### Why Operating System?
If no Operating System, the user needs to write program to access every hardware, ex - To Print a Doc, the user need to invoke the printer by writing program.

### Major Goals of Operating System
1. Convinience - Easiest Access (Windows accquired 95% market, once beacuse of convinnece)
2. Throughput - No of task executed per unit time (Linux) it gave us most efficiency.

### Functionality of Operting System
1. Resource Manger - When multiple user access, what to allocated to who is managed by Resource Manager.
2. Process Management - How to execute multiple process, (CPU Schedulling).
3. Storage Management - Harddisk (FileSystem - NFS, CIFS - How we store data in hardware, how its stored in disk architecture)
4. Memory Management - (RAM) Limitation of Size, Whenever a process executes it comes first to RAM, (Alloc & DeAlloc) occurs here. 
5. Security and Privacy - Authenticated Person can only access windows. (If any process trys to interfare other process, it gets blocked).

### Types of Operating System
1. Batch - Similar type of jobs batched - 1960 - NASA - used Punchcards to load jobs offline - They go to company - Operator - Operator Makes B1 B2 B3 - B1 will be added to CPU for execution, when CPU need to perform I/O, It goes to IDLE, then after finishin the current job, Then its goes to B2 (next job). No fix date for result. IBM made Monitor System which replaced Operators - IBSYS709X 1960 (User can directly go and execute the job)
2. Multiprogrammed - Non Preemptive - When a program is to be executed, CPU will executed it completely untill the program itself says it wants to access I/O or other operation, when the program goes to perform I/O, CPU will jump to next program execution. Less IDLENESS
3. Multitasking - Preemptive - It will be executed in a certian amount to time, if its completes well and good, if it doesn't, it will be scheduled later. So it will touch every program (Responsiveness)
4. RealTime OS - The output should be almost with minimum time difference, NO DELAYS
      * Hard - Missile System, Flight System
      * Soft - Computer Games, Youtube Live Streaming
5. Distributed - Systems are distributed (Geographically Distributed) losly copuled, If one system fails other system can executed the work.
6. Clustered - Same work as distributed, but connected thorugh LAN (kind of super computer) increased computation power. Advantages - Load Balancing, Power.
7. Embedded - It can perform only a specific task which was set once it was made. Eg - AC, Washing Machine

### Process States

![alt text](https://www.gatevidyalay.com/wp-content/uploads/2018/10/Process-State-Diagram.png)

Figure - New - Ready - (Wait/Block)(Suspend) - Running - Terminated --Its in iPAD photos now, make and paste it here).
1. New - Process Created not runned only stored.
2. Ready - Ready Queue, now its in Ram. Only few process can be in ready state, Long Term Schedular bring the processes in RAM (Multiprogramming) 
    * Few process dispatched to Running State (No. of dispatched depends on power of CPU). after completed - Terminated- then DeAllocate.
    * When RAM throws a priority task to CPU, then CPU will send the previous incomplete task to READY again (Priority), Time Quantum is one other factor which send the task to ready state, (Time Quantum - Specific amount of time allocated to perform the task, if its complete then terminates, if not sends back to ready and reschedule) It is done by STS (Short Term Schedular - has resonsibilty to schedule).
3. (Wait/Block) - When the is in Running state and wants to perform IO Operations then CPU will send the running program to Wait/Block state, once the operation is completed, then It will go to Ready state and then the CPU will executed once it is in Running state again.
     * When the WAIT/BLOCK queue gets filled, then the programs will be send to secondary memory (SUSPEND/WAIT), and it can do its required task there. It is done by MTS (Medium Term Schedular) when, Suspend Wait also gets filled, then extra task will go to SUSPEND READY ( which is Suspend for Ready Block - It works when the Ready Queue is filled, or a VVIP task is to be performed, the extra task are sent to SUSPEND READY), and the task which are in SUSPEND WAIT of WAIT/BLOCK are also sent to the SUSPEND READY once needed in worst cases, so that it can again go into Ready State then Running State.
     
### Linux Commands
     1. Only read condition to all - Chmod ugo = r note
     2. Chmod ugo+rw note in octal notation = 666
     3. lseek = its a system call (random acess of data, read write head) eg lseek(n,10,SEEK_CUR); --> lseek(n,5,SEEK_SET); so read write head in in 5.
     
          u r 4 ---- here ugo, means user group others, rwx means read write execute, 421 are numbers which are later used in octal representation.
          g w 2
          O x 1
     

 ### System Call 
     * To access kernal mode, eg If we are write a C prog, and want to print on screen, then we need to access kernal mode.
     (FIGURE)
     
     1. File Related --> Open(), Read(), Write(), Close(), Create file etc.
     2. Device Related --> Read, Write, Reposition, ioctl, fcnte
     3. Information --> getPid, attributes, get System time and data
     4. Process Control --> Load, Execute, abort, **Fork, nbit, Signal, Allocate etc
     5. Communication --> Pipe(), Create/Delete connections, Shinget ()
     
 ### Fork System Call
     * Used to create a child process
     * It genreally returns 0(Child), +1(Parent), -1 (Child X) X 
       eg -
          main()
          fork();
          printf("hello");
          }
          
          Output - Hello Hello. 
          
        *Explanation - 
                P -- Fork Called then C1 and P, if it is called again then P Fork Called C1, P Fork Called Again - C2,C1,C3,P.
        [Add Figure] 
        How to find  : no of output - n^2 , and no of child = n^2 -1.
        
### Understanding Fork using Program

     #include<stdio.h>
     #include<unistd.h>
     int main()
     {
          if(fork()&&forks()) {
               fork();
          printf("hello");
          return 0;
     }
     
Output : If the condition is true hello will be printed 4 times. [add figure for clarification]

Note : Child returns 0 and Parent returns +ve no;

### User Mode and Kernal Mode

![alt text](https://miro.medium.com/max/3616/1*J3LbfnG88ysmltH48VhU6w.png)

Drivers : Works in Kernal Mode
Processors : Switches between User & Kernal Mode

Working : Suppose you write a program in notepad, You access the notepad usinga API call, then the program which you wrote requires access of Hardware, such as IO devices, So The process starts which executing, once it starts to execute then it it will use Get System Call we will used Read() sys call, to access the kernal mode and its MODE BIT = 1 will change to MODE BIT = 0, its called trap, once it completed its work in the kernal mode, it will again go back to user mode and perform the rest of the execution RETURN FROM KERNAL MODE and its MODE BIT will again shift to 1. 

### Process v/s Threads

| Process | Threads |
| --- | ----------- |
| System Calls  involved in Process | There is no system call |
| OS Treats different process differently | All user level threads as singel task for OS |
| Different process have different copies of Data, Files, Code | Threads share same copy of code and data |
| Context switching is slow | Context switching is faster |
| Blocking a process will not block entire process| Blocking thread will block entire process |
| Independant | Interdependant |


### User Level Thread v/s Kernal Level Thread


| User Level Thread | Kernal Level Threads |
| --- | ----------- |
| User level threads are managed by User Level Library | Kernal Level Threads are managed by OS System Calls |
| User level threads are typically fast | Kernal level threads are slower than user level |
| Context Switiching is Faster | Context Switching is Slower |
| If one user level threads perform blocking operation then entire process gets blocked. | If one kernal level thread blocked, No affect on others |

Note : Process/CT > KLT > ULT

Hybrid Threads :
![alt text](http://www.it.uu.se/education/course/homepage/os/vt18/images/module-4/scheduler-activations-1-2.png)

# CH2

### Process Scheduling Algorithms

Scheduling Algorithm : It is a way to pick a process and keep it in ready queue. then to send it to CPU.

Pre-emptive : Stop in middle and execute in future. :: Ready Queue --> Running --> Ready Queue :: Uses time quantum (specific amount of time is alloted for a task, if it gets complete good, if not then rescheduled.) and another factor is Priority of the task or process.
Pre-emptive : Complete Execution. :: Ready Queue --> Running --> Terminate ::

### Algorithms in Scheduling.

| Pre-Emptive | Non Pre-Emptive |
| --- | ----------- |
| SRTF (Short Remaining Time First)| FCFS (First Come First Serve) |
| LRTF (Longest Remaning Time First | SJF (Shortest Job First) |
| Rand Robin | LJF (Longest Job First) |
| Priority Based | HRRN (Highest Response Ratio Next) |
| | Multilevel Queue |
| | Priority |


### CPU Scheduling 

     1. Arival Time : The time at which process enter the Ready Queue or State
     2. Brust Time : Time required by a process to get execute on CPU (Duration)
     3. Completion Time : The time at which process complete its execution, (Not Duration)
     4. Turn around Time : {Completion time - Arrival Time}
     5. Waiting Time : {Turn around time - Brust Time}
     6. Response Time : [(The time at which process get CPU First Time) - (Arrival Time)]
     
 Types of Time :
 1. CPU Bound : Time which is used while performing this process by CPU
 2. I/O Bound : Time when the process goes to perform IO 
 
 ### FCFS : First Come First Serve Algorithm Example 
 
 Criteria : Arrival Time
 Mode : Non Preemptive
 
| Process No | Arrival Time | Brust Time | Completion Time | TAT | WT | RT |
| --- | ----------- |----------- |----------- |----------- |----------- |----------- |
| P1 | 0 | 2 | 2 | 2 | | 0 |
| P2 | 1 | 2 | 4 | 3 | | 1 |
| P3 | 5 | 3 | 8 | 3 | | 0 |
| P4 | 6 | 4 | 12 | 6 | | 2 |

ADD GANT CHART : Remember it is solved using GANT Chart.
XX -- IDLE TIME
Look at the subscript values to know the running time.
Once the process is in ready state, it is the next process to be performed.

Gant Chart : P1   P2  XX   P3  P4
Time       : 0 2   4   5    8   12

Formulas :
TAT = CT-AT
WT = TAT-BT


### Algorithms Discuessed 
   PDF of the numericals are attached .
     1. FCFS : Criteria Arrival Time
     2. SJF : Criteria Brust Time
     3. SRTF : Criteria Brust Time, Preemptive
     4. Round Robin : Criteria Time Quantum, Preemptive
     5. Priority Scheduling : Criteria Priority, Preemptive Note, higher the number higher the priority.
     6. 
     
### Multi Level Queue Scheduling 

![alt text](https://2.bp.blogspot.com/-YxSVj0AATSA/W6K13EPH7jI/AAAAAAAAAB8/UFbljrzj5EYOrKb08NXQpp1hBR_ngWQlwCLcBGAs/s1600/cpu-scheduling-algorithm.png)

     * Every process has different algorithms.
     * Here we are categorizing the process.
     * Starvation may occurs due to mismanagement of priority.
     * To solve starvation we use, feedback queue.
     
### Multilevel Feedback Queue
     

    
     
     
    

