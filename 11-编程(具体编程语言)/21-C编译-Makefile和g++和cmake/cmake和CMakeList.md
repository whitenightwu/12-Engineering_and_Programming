# cmake
在Linux下进行开发很多人选择编写makefile文件进行项目环境搭建，而makefile文件依赖关系复杂，工作量很大。
采用自动化的项目构建工具CMake可以将程序员从复杂的makefile文件中解脱出来，与此对应的就是编写CMakeList.txt。
cmake能根据内置的规则和语法来自动生成相关的makefile文件进行编译，同时还支持静态库和动态库的构建。
cmake的使用很简单，更高级的应用好比版本信息，打包，安装等相关基本的应用后面会一一介绍。



## cmake的使用
完成对应的CMakeLists.txt 文件编写后，便可以进行编译了。由于编译中出现许多中间的文件，因此最好新建一个独立的目录build，在该目录下进行编译
1）mkdir build
2）cd build
3）执行“cmake ..  ”，会生成很多文件
4）再执行"make"


打开cmake的图形界面：cmake-gui
查看版本：cmake --version

CMakeListserv.txt的写法
CMakeLists.txt文件中不区分大小写

2）PROJECT(project_name)
定义工程名称
语法：project(projectname [cxx] [c] [java])
可以指定工程采用的语言，选项分别表示：C++, C, java， 如不指定默认支持所有语言。

3）MESSAGE（STATUS, "Content")
打印相关消息
输出消息，供调试CMakeLists.txt 文件使用。

4）SET(CMAKE_BUILE_TYPE DEBUG)
设置编译类型debug或者release。
debug版会生成相关调试信息，可以使用GDB；release不会生成调试信息。
当无法进行调试时查看此处是否设置为debug。

SET(SOURCE_FILES .....)
例如：
SET(SOURCE_FILES ConfigParser.cpp StrUtility.cpp) # 设置变量，表示所有的源文件。
表示要编译的源文件，所有的源文件都要罗列到此处。set 设置变量，变量名SOURCE_FILES自定义。


5）SET(CMAKE_C_FLAGS_DEBUG "-g -Wall")
设置编译器的类型CMAKE_C_FLAGS_DEBUG    ----  C 编译器
CMAKE_CXX_FLAGS_DEBUG  ----  C++ 编译器

6）ADD_SUBDIRECTORY(utility) 添加要编译的子目录
为工程主目录下的存放源代码的子目录使用该命令，各子目录出现的顺序随意。

7）INCLUDE_DIRECTORIES()
相关头文件的目录 INCLUDE_DIRECTORIES(/usr/local/include ${PROJET_SOURCE_DIR}/utility) 
include头文件时搜索的所有目录,变量PROJECT_SOURCE_DIR 表示工程所在的路径，系统默认的变量

8）LINK_DIRECTORIES(/usr/local/lib)
库文件存放的目录，在程序连接库文件的时候要再这些目录下寻找对应的库文件。

9）ADD_LIBRARY(association ${SOURCE_FILES})
表示生成静态链接库libassociaiton.a，由${PROJECT_SOURCE_DIR}代表的文件生成。
语法：ADD_LIBRARY(libname [SHARED|STATIC]
SHARED表示生成动态库，STATIC表示生成静态库。

10）TARGET_LINK_LIBRARY(association core）
表示库association，依赖core库文件

11)SET_TARGET_PROPERTIES
设置编译的库文件存放的目录，还可用于其他属性的设置。如不指定，
生成的执行文件在当前编译目录下的各子目录下的build目录下，好拗口！简单一点：
如指定在： ./src/lib 下
不指定在： ./src/build/utility/build 目录下
生成的中间文件在./src/build/utilty/build 目录下，不受该命令额影响

