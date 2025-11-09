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
 
 
