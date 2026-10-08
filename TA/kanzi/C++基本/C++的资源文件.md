“资源文件”方案，本质上是把 .vert/.frag 这种文本当成 DLL 里的“资源块”嵌进去，而不是当成磁盘文件。

常用 API：
•FindResourceA / FindResourceW
	根据资源 ID 和资源类型，定位资源
•LoadResource
	把资源从 DLL 中加载到内存
•LockResource
	获取资源真正的数据指针
•SizeofResource
	获取资源大小
•MAKEINTRESOURCEA
	把整数 ID 转成 Windows 资源名
<!--stackedit_data:
eyJoaXN0b3J5IjpbNDIzMzE2MTAzXX0=
-->