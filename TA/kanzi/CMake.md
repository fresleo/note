CMake 自带/标准语法/标准变量

> CMAKE_CURRENT_LIST_DIR

CMake 内置变量，当前 CMakeLists 文件目录

> PROJECT_SOURCE_DIR

CMake 内置变量，当前 project 的源码根目录

> CMAKE_COMMAND

CMake 内置变量，指向当前 CMake 可执行文件


> BUILD_SHARED_LIBS

CMake 标准变量/选项，控制默认构建动态库还是静态库

> MSVC

CMake 内置布尔变量，表示当前编译器是否为 MSVC

`include(kanzi-common)`
不是变量，是 CMake 的 include() 命令，加载一个模块文件


> install_target_to_output_directory(...)

不是 CMake 语法，而是 Kanzi 自定义函数

> PLATFORM_TARGET / MSVC_TAG

Kanzi 自定义变量

> KANZI_ENGINE_BUILD

Kanzi 自定义/可选变量，通常由上一层构建系统或缓存传入；本文件里并不定义它

> declspec(dllexport)

不是 CMake 语法，是 MSVC 的 C/C++ 导出声明语法
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTkyOTg0MDYzN119
-->