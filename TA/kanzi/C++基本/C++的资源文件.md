“资源文件”方案，本质上是把 .vert/.frag 这种文本当成 DLL 里的“资源块”嵌进去，而不是当成磁盘文件。

## ***常用 API：***

 - FindResourceA / FindResourceW 	**根据资源 ID 和资源类型，定位资源**
 - LoadResource 	**把资源从 DLL 中加载到内存**
 - LockResource **获取资源真正的数据指针**
 - SizeofResource 	**获取资源大小**
 - MAKEINTRESOURCEA 	**把整数 ID 转成 Windows 资源名**

## 资源类型：

 - RT_RCDATA 最常见，表示任意二进制/文本数据
 - •也可自定义类型，但 RT_RCDATA 最简单


resource.h：定义资源 ID
.rc：声明“这个资源对应哪个文件”
shader 文件：实际内容，放在 .rc 中的路径里


例如，通常写法是：
1)
resource.h

    #pragma once
    
    #define IDR_SHADER_VERT 101
    #define IDR_SHADER_FRAG 102

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTIwNjIwMzE3MzcsLTIwMjI4OTAyMTRdfQ
==
-->