https://github.com/inpyjama/Makefile-Tutorial

Make is a utility - automation. -> commands in a file. 
execute shell commands/ cmd line. terminal. usually used in building source code. i.e Compilation.

Makefile -> name exactly same.
 should contain targets and specifics/recipes
xyz is a target, and 5 commands below form recipe for target
xyz:
  ls (must have one tab character before commands)
  touch a.txt
  echo "hello world" > a.txt
  ls
  cat a.txt

abc: 
  echo "this is abc target"
 command: make abc
    looks for Makefile and looks for targets and executes recipes on it.  If target is not mentioned, it picks the first target. 


xyz: a.txt -> Dependency on a target xyz. runs xyz target commands only when a.txt is changed.

eg: main.o:main.c
    gcc main.c -o main.o 
    make main.o //should generate main.o only when main.c is modified. If not it will skip running recipes. looks for timestamp file modified. If target is newer than all dependencies.
    

 
