foo.s (assembly code) -> assembler(ARM) -> foo.o
map.ld is a linker script. It guides on how to place stuff in memory. 
foo.o + map.ld to linker -> generate ELF file. feed this ELF to QEMU. 

QEMU will work like cortex M3. Open socket at QEMU for connecting gdb to debug/look at programmer view SP, PC etc.

check foo.s
   directives,
   vector_table:
    SP, PC(reset_handler address), Mem init, 
   .align 1


**map.ld** -> MEMORY , SECTIONS - .text section
MEMORY
{
  MEM: ORIGIN = 0x0, LENGTH = 0x4000
  RAM: ORIGIN = 0x2000000, LENGTH = 0x800
}

SECTIONs
{
  .text : {
    *(.vectors*)
    *(.txt*)
  }>MEM
  .data : {
    *(.data*)
  }>RAM
}

**foo.elf** -> foo.lst generate obj dump. lst contains - lot of debug info about assembly code.
  reset_handler - address, instructions, opcodes.

bl - branch and link.

**Instructions**: 
16 bit instruction -> ADD R1 R2 R3 -> let's say 4 bits for each. operation, destination, source1, source2.
4 bits for operation indicates only 16 operations can be done. 4 bits for register -> 16 registers.
32 bit instructions -> more operations. 
Instructions encoding. Format, opcode, etc. bits to instructions. 

objcopy -O binary foo.elf foo.bin
ELF - instructions, data, debug related info, .data, .txt, etc.
.bin will have direct memory dump. instructions, data at exact mem locations.

Anatomy of assembly file
  instructions, hints for assembler. 
  .s -> preprocessor is not run; .S -> preprocessor is run; .asm
  Directives, labels, instructions, comments
  starting with . is directive. These are the hints for assembler.
  ending with : is a label. Label is a readable name given to a location. 

Instructions
  calculation, 
  data movement related
  condition related
  branching - condition/nonconditional jump, 
  stack manipulation - push/pop. 
  

**Systick timer**:
Memory mappe I/O. modify registers to control Systick. registers have dedicated memory address.
Systick ->NVIC -> CPU -> systick_handler

.equ systick_csr 0XE000E010 //address of CSR reg
.equ timeout 0xFFFF 

reset_handler:
 ldr r0,=systick_csr
 ldr r1,=systick_rvr
    r2 = cvr
 ldr r3,=timeout

 str r3, [r1] //storing timeout value to RVR register

 mov r3, #0x0
 str r3, [r2] 

 mov r3, #0x7
 str r3, [r0]
  b. //stuck here waiting for ISR
  
systick_handler: //CPU automatically saves context (in this case only 8 registers are saved by CPU. Rest 8 needs to be done by user)
   add r5, r5, #1 //just increment r5 to track timer expiry

   push registers r4-r11 // of Task1()
   update SP //to SP2
   pop registers   //of Task2()
   bx lr

#Tasks
  .section .text
  .p2align 4
  .globl main1
  .type main1, %function
main1:
   nop
   add r0 r0 0x1;
   b main1

  .section .text
  .p2align 4
  .globl main2
  .type main2, %function

main2:
   nop
   add r1 r1 0x2;
   b main1


  .section .text
  .p2align 4
  .globl main3
  .type main3, %function
main3:


   .data
   .align 4
stack_1: 
  .word 0x18
  .word 0x19
  ...
  .word 0x309 ->
  .word main1 -> PC
  .word 0x01000000 -> xPSR
  .zero 100
  .align 4
  
stack_2:


stack_3:


break here:
   bx lr //exit from ISR


#SP was given address in code memory. so writing into stakc gave hardfault error. 
check for read write memory starting location. SRAM address 
full stack is at lower address. As you POP SP add increases
we kept stack on RAM. on powercycle we lose RAM data.
on HW, there is .data section on ROM/code memory and every power cycle reset handler will have instr to copy .data section to RAM. 
when binary is created it ensures that when code access data section, code will use RAM address to access data. LInker script will separate load (Physical address) and virtual address (of RAM) of content.




   

