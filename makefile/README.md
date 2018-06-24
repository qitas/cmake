# makefile

对C、C++语言程序、需要先把源文件编译成中间代码文件，在Linux下中间代码是 .o 文件，在Windows下是 .obj 文件，这个动作叫做编译（compile）。把大量的.o文件合成执行文件叫作链接（link），对于编译和链接过程需要有相应文件的路径。

 make命令执行时，需要一个 Makefile 文件，以告诉make命令需要怎么样的去编译和链接程序。

 Makefile里主要包含了五个东西：显式规则、隐晦规则、变量定义、文件指示和注释。

* 显式规则：由Makefile指出要生成的文件，文件的依赖文件，生成的命令
* 隐晦规则：由make有自动推导的功能，所以隐晦的规则可以让我们比较粗糙地简略地书写Makefile
* 变量的定义：变量一般都是字符串，这个有点你C语言中的宏，当Makefile被执行时，其中的变量都会被扩展到相应的引用位置上。
* 文件指示：其包括了三个部分，一个是在一个Makefile中引用另一个Makefile，就像C语言中的include一样；另一个是指根据某些情况指定Makefile中的有效部分，就像C语言中的预编译#if一样；还有就是定义一个多行的命令。
* 注释：只有行注释，和UNIX的Shell脚本一样，其注释是用“#”字符，在Makefile中使用“#”字符，可以用反斜框进行转义“\#”。


```
make，又指GNU Make，指一条可执行指令，该指令是读入一个名为makefile的文件然后执行这个文件中指定的指令。
Make从makefile文件中获得构建程序的依赖关系，Makefile列出了每个目标文件以及如何由其他文件来生成它。
编写一个程序时，为它编写一个makefile文件，就可以使用Make来编译和安装这个程序。
```

 Makefile文件就是告诉make命令怎么样地去编译和链接程序。Makefile就像一个Shell脚本一样，其中也可以执行操作系统的命令。

* make命令会在执行的目录下找名字叫“Makefile”或“makefile”的文件

```
obj = main.o utils.o
edit : $(obj)
        cc -o edit $(obj)
main.o : main.c defs.h
        cc -c main.c
```
GNU make可以自动推导文件以及文件依赖关系后面的命令，没必要在每一个[.o]文件后都写上类似的命令。

只要make看到一个[.o]文件，它就会自动的把[.c]文件加在依赖关系中，如果make找到一个whatever.o，那么whatever.c，就会是whatever.o的依赖文件。

```
obj = main.o utils.o
edit : $(obj)
        cc -o edit $(obj)
main.o : defs.h
```

### 依赖库链接

GCC命令行-L 指定库的路径 -l 指定需连接的库名

-l(小写的L)参数就是用来指定程序要链接的库，-l参数紧接着就是库名。-lm 表示程序指定的链接库名是m  (math数学库)


放在系统的/lib，/usr/lib，/usr/local/lib里的库直接用-I参数就能链接

链接程序ld在上述3个目录里找不到libxxx.so，这时另外一个参数-L就派上用场了，我们编译时就要用-L /usr/X11R6/lib -lX11参数，-L参数跟着的是库文件所在的目录名。


Makefile有三个非常有用的变量：$@，$^，$<

$@--目标文件，$^--所有的依赖文件，$<--第一个依赖文件。



```
.PHONY : clean
clean :
        -rm edit $(objects)
```
.PHONY意思表示clean是一个“伪目标”，rm命令前面加小减号的表示某些文件出现问题，但不要管，继续做后面的事。

