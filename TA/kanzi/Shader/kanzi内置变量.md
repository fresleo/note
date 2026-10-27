内置变量脚本：
路径：

> Engine\include\kanzi\core.ui\platform\graphics_backend\gl\shader.hpp



gl_FragCoord：GPU 在光栅化阶段自动生成的内建变量


它表示：
•当前正在处理的片元在窗口坐标系中的位置
•x/y 是当前像素的屏幕坐标
•z/w 是深度和齐次坐标
•其中 gl_FragCoord.xy 可以理解为“当前片元对应屏幕上的像素位置”

<!--stackedit_data:
eyJoaXN0b3J5IjpbMTYyMjY3NTk2NiwyOTM3OTI5NzRdfQ==
-->