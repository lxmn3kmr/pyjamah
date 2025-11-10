https://github.com/inpyjama/linker-script-course


gcc -> GNU compiler collection
**steps**:
.c/.h -> compiler -> (.i ->.s -> .o) -> Linker (merges .o to .elf using instructions from .ld file i.e linker script)

**compilation** - preprocessing macros (#defines,includes etc.) creates .i file. usually hidden from user. and then compiles to .S file. 
Assembler generates .o file from .s files.
.o are sent to linker to generate .elf 

let's say b.o when generated assembler does not know where a global variable AP is. It leaves some whole for linker to fill it in. Linker identifies it is in a.o. 
Linker is doing a symbol resolution. **Resolving symbol references**. 

.c to .o -> 
.c will have text and data (global - init & uninit, local, static data)
.o will have .text(code), .data(global initialized, static variables ), .bss(global var not initialized)

Linker will **merge sections** from multiple  .o files, merge into one .text, .data, .bss sections. 
**Placement of Sections**. which section should go at which memory?

**Linkerscript** will aid in merge sections and placement of sections. 

MCU -  code memory (ROM), data memory (SRAM)
.elf will have information that code goes at which address and data at data memory address. 

if we don't provide linkerscript, gcc picks default internal one. 
Do objdump on elf file to generate .s file to understand placement of sections


**C code and Allocation in Memory**
code is in .txt
.data
  initialized global
  static initialized global
  static initialized local

.bss 
  uninit global
  static uninit global
  static uninit local

  local variable is allocated at runtime on the stack when the function is invoked. 

.ELF is final binary that can be loaded to MCU. microcontrollerunit. .ELF has some additional headers/debug
.bin has removed additional headers/debug info from .efl


  utilities --> arm-none-eabi-****


**MAP file**
linker outputs map file from main.ld and main.o
Memory configuration
sections:
.txt information
.data
.bss  (can have all these in same section)


.ld file
/DISCARD/:{          --> This will discard a section/file. can selectively select a few sections by incoporating those in .txt/.data/.bss section
  global.o (*)
}



