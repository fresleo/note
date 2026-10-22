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
    kzsGlBindVertexArray(0);  
    SAFE_FREE_VBO(wfEbo); SAFE_FREE_VBO(wfVbo); SAFE_FREE_VAO(wfVao);
  




宏参数说明：
1)
KZS_GL_ARRAY_BUFFER_BINDING 和KZS_GL_ELEMENT_ARRAY_BUFFER_BINDING
它们是两种“不同用途”的缓冲区绑定对象。

 • ARRAY_BUFFER：存顶点属性数据 ◦ 例如 position, normal, uv, color 等 ◦ 绑定后，后续glVertexAttribPointer 读的就是这里的数据 ◦ 典型用法：glBindBuffer(GL_ARRAY_BUFFER, vbo); 
 
• ELEMENT_ARRAY_BUFFER：存索引数据 ◦ 例如 0,1,2, 0,2,3 绑定后，glDrawElements 会从这里读取顶点索引 ◦
   典型用法：glBindBuffer(GL_ELEMENT_ARRAY_BUFFER, ebo);


2)
KZS_GL_STREAM_DRAW, KZS_GL_STATIC_DRAW, KZS_GL_DYNAMIC_DRAW
这几个是“缓冲区数据更新频率”的 hint，告诉 GPU/驱动这个 buffer 以后大概如何使用。它不决定正确性，但对性能有帮助。

•GL_STREAM_DRAW

 - ◦ 数据几乎每帧都改 
 - ◦ 例如：粒子系统、动画骨骼、动态顶点
 -  ◦ 例子：每帧更新一组粒子位置


•GL_STATIC_DRAW

 - ◦ 数据只创建一次，几乎不变
 - ◦ 例如：静态网格、静态 quad、UI 固定顶点 
 - ◦ 例子：屏幕四边形、贴图固定几何体


•GL_DYNAMIC_DRAW

 - ◦ 数据会多次更新，但不是每帧都无脑重写 
 - ◦ 例如：骨骼动画、频繁变换的 transform / UI 动画 
 - ◦例子：每次变换一个物体的位置，但不一定每帧都完全刷新


配置attribute函数介绍

kzsGlEnableVertexAttribArray(0);  
kzsGlVertexAttribPointer(0, 3,KZS_GL_FLOAT,KZS_GL_FALSE, 0, (void*)0);

 -  0：attribute location，表示把数据绑定到顶点着色器的 layout(location = 0) 或 attribute vec3 ... 这个位置 
 -  3：每个顶点属性包含 3 个分量 
 -  KZS_GL_FLOAT：每个分量类型是float 
 -  KZS_GL_FALSE：不归一化
     0：stride = 0 这是“紧凑连续”模式，表示数组里每个顶点属性紧挨着下一	    个顶点，没空隙 
  -  (void)0：offset = 0 ◦ 从该 VBO的起始位置开始读取

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE0MzIyNDg1MzYsLTExNDExMDU0MTQsLT
MyMDYxMDgwMCw1MzU5ODU1MTcsLTEwODEwMTk2MDMsMjQxMzEw
MjY0LDE3MzQ2ODIwMDZdfQ==
-->