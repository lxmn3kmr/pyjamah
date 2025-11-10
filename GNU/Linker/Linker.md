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
  const variable?  .rodata

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
.rodata (read only data/variables)

.ld file
/DISCARD/:{          --> This will discard a section/file. can selectively select a few sections by incoporating those in .txt/.data/.bss section
  global.o (*)
}


Need to include .rodata in ld file to include const variables. 


initialized global saved in RAM when program is loaded. but if we remove power, content of RAM is gone. **how to retain content of data section**?
Keep a copy of .data and .bss in ROM. Transfer this content to RAM every power cycle/reboot in predefined/reserved memory space. and code uses address of RAM to modify the variable. Virtual Memory Address(RAM) and Load Memory Address (ROM)

.data : {
  *(.data)
} > RAM AT > ROM      --> RAM and ROM. 

map file will now have both VMA and LMA captured. 

**Copy from LMA to VMA**
src, dst, size. 
Current Location counter - pointing to VMA

.rodata : {
 *(.rodata)
 _src_start_data = .;
} > ROM AT > ROM --> This will be loaded last in ROM after which data section is loaded. hence get end address of this section

.data : {
  _dst_start_data = .;  -> start of VMA in RAM. 
  *(.data)
  _dst_end_data = .;  -> end of VMA in RAM 
} > RAM AT > ROM      --> RAM and ROM.   -> size = end - start;

.bss : {
  _dst_start_bss = .;  -> start of VMA in RAM. 
 *(.bss)
  _dst_end_bss = .;
}

**add copy function in main.c **

extern char _dst_start_data;
extern char _dst_end_data;
extern char _src_start_data;

void copy(){
char *src = &_src_start_data; --> variables from linker scripts are address. 
char *dst = &_dst_start_data; 
char *dst_end =&_dst_end_data;

while(dst!=dst_end)
*dst++ = *src++;

while(_dst_start_bss != _dst_end_bss){
  *dst++ = 0; 
  src++; 
  }

}

we find constructors like this in static code of diff programs.
even in linux kernel - loader also does this kind of copy and only after above copy main.c is called. 

we need this code to be executed first. so should be placed on top of .text section. and then initialize bss memory in RAM to 0. similar code like above. 
so **create a special section** . GCC attribute section. mark a function in custom section. can mark variable also in custom section. 
void __attribute__((section (*new_section_name))) copy()

.text : {
  *(new_section_name) //placing before other code
  *(.text)
}



