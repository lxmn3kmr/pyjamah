foo.s (assembly code) -> assembler(ARM) -> foo.o
map.ld is a linker script. It guides on how to place stuff in memory. 
foo.o + map.ld to linker -> generate ELF file. feed this ELF to QEMU. 

QEMU will work like cortex M3. Open socket at QEMU for connecting gdb to debug/look at programmer view SP, PC etc.

check foo.s
   directives,
   vector_table:
    SP, PC(reset_handler address), Mem init, 
   .align 1


map.ld -> MEMORY , SECTIONS - .text section


foo.elf -> foo.lst generate obj dump. lst contains - lot of debug info about assembly code.
  reset_handler - address, instructions, opcodes.

bl - branch and link.

Instructions: 
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
  




