A-Class - Application Grade processors - capable to run Windows, Linux
R-Class - Real Time System CPUs - Engine control/
M-Class - Micro Controller based CPUs for embedded applications - windows in car, remote controls, wifi.

MicroArchitecture vs Architecture

Architecture is simply a specification of what CPU should do.
Micro-Arch is implementation of architecture. 

**CPU memory interactions**
Memory - instructions mem, data memory. 
ARM CPUs are based on RISC architecture - Load/Store architecture. Any computation on data -> fetch data, compute, store data. 
CPU is made up of GPR (general purpose registers), Status Register and configuration Registers, ALU, 
  Eg: Status Registers captures if a computation value is negative, zero, carry 
  Config Registers - run in specific mode

CPU -
  Programmers Model - mode of operation of CPU and internal Registers
  Exception model  - how to handle interrupts
  Memory model - CPU interactions with memory. Eg: how much data is stored at CPU before sending it out etc.
  Debug model - gdb. external circuitry in CPU which can connect external HW. external HW can control CPU. give instructions to CPU.
  

  CPU will have buses. Ex: 2 pairs of buses. 
     Instruction Bus - contains adress link where to fetch instructions from and Bus to fetch, 
     Address Bus - contains adress link where to fetch data from and Bus to fetch. 
  CPU have Interrupts - External to CPU. Exception is internal eg: when address is invalid.

**Programmers Model:**
  Arm Cortex M Programmers Guide
  General Registers - R0-R15; Special Registers - Interrupt, control, Program Status
  two R13 registers - MSP (Main Stack Ptr) and PSP (Process Stack Ptr). Why 2 SP?
  R14 - Link register - points to the address of next instruction where control returns back to after function call. 
  R15 - Program Counter -   points to the address where instruction needs to be fetched

  xPSR - Program Status Register - combination of 3 registers. Execution PSR, Application PSR, InterruptPSR. 
      Union of these 3 is xPSR (32 bit). 
      0-8 is is IPSR (contains exception number i.e which interrupt happened), 
      9-24 EPSR (contains if it is conditional instruction etc, 
      25-31 is APSR (contains Negative, Zero, Carry).
  Control Register - controlling the previlige and which stack pointer to use.  
  Interrupt registers
    PRIMASK - priority mask - CPU can have multiple interrupts- each interrupt is assigned a priority. 
    BASEPRI - Base priority.  Using Basepri and primask we can control which interrupts are allowed. 
    FAULTMASK 

  CPU can have FPU Floating point unit for computations - Status, Data registers. FPRSCR (FP status and control Register), D0-D15

  **Modes and Priviliges**
   CPU can operate in two modes - 
   Handler mode - interrupt handling . Always privileged. 
   Thread mode - computation related, actual application code. Can be privileged and non priv. Application related runs on                     unpriviliged thread mode. safeguard control/status registers
 
   Priviliges - 
    Privilege -  code can access control and status registers
     non privilege - code cannot access control and status registers

 
  CPU starts in Privileged thread mode. Switches to unpriv thread mode and then excpetion handler to priv or unpriv thread mode. 

  When CPU starts stack ptr used is MSP. We can change to PSP in Priv mode. 
  UnPriv mode PSP is used. 
  Handler mode always use MSP.
  Why 2 SP?
   eg: In unpriv mode, application is pushing data to stack. Stack grows downwards. If stack is full, CPU moves to handler mode. Now in handler mode, CPU needs stack to run its operations henc MSP has room to run those exceptions. 


**Vector Table**
  In class M CPU, when interrupt happens, CPU need to know where to go to/how to handle. Instructions on handling are in vector table. 
  Interrupts are called vectors. 
  Table - usually starts at address 0. at every 4 bytes, we have a number which tells where CPU should jump. So many numbers-? because of multiple interrupts supported. 
  0th memory location of table - contains MSP initial value. it is where stack starts.
  1st location - 0x4 Reset vector - location where CPU should start fetching instructions on a reset. (Boot part)
  2nd loc - 0x8 - NMI vector - location where instructions of handling NMI are present
  3rd - 0xC - Hard fault vector
  4th - 0x10 - Mem Manage vector
  5 - Bus fault vector - Bus responds error. CPU goes to address in this memory and fetches instructions. 
  ..
  Sys tick vector 
  interrup0
  inter1
  ...
  interupt 240 vector. Different peripherals (UART, DMA) are mapped to diff interrupt numbers. 

  **When CPU boots up -** 
   0th entry in vector table - which has MSP is put into R13 register.
   loads 1st entry - 0x4 - into Program Counter and starts executing instructions. 

**QEMU - emulator**
  Emulator is a SW that acts as a CPU. Has Binary for cortexM.  attach a debugger to QEMU to debug CPU. 
  QEMU has debug interface and exposes ports to external hw .

  Make file - instructions, elf file
  
  
  
  

  

  
  
  
