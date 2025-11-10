input files
file formats
output files
placement of common blocks
address of sections

if not obj|archive|linker script then prints error. Linker will first identify files or obj or archive. then linker script. if none  prints error. 

LinkerScript is a collection of statements. 
   set a particular option
   select/group input files, name output files

  two important statements:   sections and memory

Expressions : 
  all expressions are evaluated as unsigned long integers. 
   all constants are integers
   arithmetic operators are provided
   you may reference define and create global variables
   can call special built in functions

Symbol Names:
   unless quoted start with Letter, underscore, point
   unquoted symbols should not conflict with keywords like symbols, 

Location Counter  .


Expression evaluation 
   Lazy expression  -> calculated an expression only when absolutely necessary
   ex:  memory regions' start and end is needed to do any linking at all. so computed as soon as command file is read.
   "symbol values" are not known until after storage allocation

Assignment operation 

Placement of assignment statements 
   can place anywhere in linker script. address will be allocated relative to section or absolute if not placed in sections. 

Relocatable (fixed offset from base location of section) and absolute (value is same in output file) type of Symbols
    
   
   
