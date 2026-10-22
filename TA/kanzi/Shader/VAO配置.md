VAO（Vertex Array Object） 是“顶点输入状态对象”，用来记录顶点数据如何喂给 vertex shader。


它通常保存：

 - • 启用哪些顶点属性（glEnableVertexAttribArray） 
 - • 每个属性从哪个 VBO 读、格式是什么(glVertexAttribPointer)
   • GL_ELEMENT_ARRAY_BUFFER（索引缓冲）绑定


常见流程：

1.glGenVertexArrays 创建  
2. glBindVertexArray 绑定  
3. 绑定 VBO/EBO + 设置 glVertexAttribPointer  
4. 绘制时只需 glBindVertexArray + glDraw




<!--stackedit_data:
eyJoaXN0b3J5IjpbNDUwMDc2OCwxNzM0NjgyMDA2XX0=
-->