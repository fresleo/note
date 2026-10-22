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


标准流程就是 “创建一次，配置一次，绘制时绑定”。


    GLuint vao = 0, vbo = 0, ebo = 0;
    
    // 1) 创建对象
    glGenVertexArrays(1, &vao);
    glGenBuffers(1, &vbo);
    glGenBuffers(1, &ebo);
    
    // 2) 绑定 VAO（后续顶点配置都会记录进它）
    glBindVertexArray(vao);
    
    // 3) 上传顶点数据
    glBindBuffer(GL_ARRAY_BUFFER, vbo);
    glBufferData(GL_ARRAY_BUFFER, vertexBytes, vertices, GL_STATIC_DRAW);
    
    // 4) 上传索引数据（EBO 绑定也会被 VAO 记住）
    glBindBuffer(GL_ELEMENT_ARRAY_BUFFER, ebo);
    glBufferData(GL_ELEMENT_ARRAY_BUFFER, indexBytes, indices, GL_STATIC_DRAW);
    
    // 5) 配置 attribute（例：location=0 是 vec3 位置）
    glEnableVertexAttribArray(0);
    glVertexAttribPointer(
        0,                  // location
        3,                  // 分量数
        GL_FLOAT,           // 类型
        GL_FALSE,           // 是否归一化
        sizeof(Vertex),     // stride
        (void*)offsetof(Vertex, pos) // 偏移
    );
    
    // 6) 解绑 VAO（可选）
    glBindVertexArray(0);

<!--stackedit_data:
eyJoaXN0b3J5IjpbMjQxMzEwMjY0LDE3MzQ2ODIwMDZdfQ==
-->