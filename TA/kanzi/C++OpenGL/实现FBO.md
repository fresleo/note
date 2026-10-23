流程：

1.创建颜色纹理（Render Target）  
glGenTextures / glBindTexture(GL_TEXTURE_2D, tex)  

◦glTexImage2D(..., width, height, ..., nullptr) 分配存储  
◦设置 GL_TEXTURE_MIN_FILTER / MAG_FILTER / WRAP
2.创建 FBO  
◦glGenFramebuffers(1, &fbo)
3.绑定 FBO  
◦glBindFramebuffer(GL_FRAMEBUFFER, fbo)
4.附加颜色纹理到 FBO  
◦glFramebufferTexture2D(GL_FRAMEBUFFER, GL_COLOR_ATTACHMENT0, GL_TEXTURE_2D, tex, 0)
5.（可选）附加深度/模板缓冲  
◦需要深度测试时，创建 renderbuffer 并 glFramebufferRenderbuffer(...)
6.检查 FBO 完整性  
◦glCheckFramebufferStatus(GL_FRAMEBUFFER)  
◦必须是 GL_FRAMEBUFFER_COMPLETE
7.在该 FBO 上绘制  
◦glViewport(0,0,width,height)  
◦glClear(...)  
◦program->apply() + DrawQuad()（或你的 mesh draw）
8.恢复默认/上级 framebuffer 并保存输出句柄  
◦glBindFramebuffer(GL_FRAMEBUFFER, prevFbo)  
◦把这张纹理写回属性：setProperty(OutPutTextureProperty, textureResource)


一个最小伪代码骨架：


    int previousFramebuffer = 0;  
    //查询某个 OpenGL 状态变量的整数值  
kzsGlGetIntegerv(KZS_GL_FRAMEBUFFER_BINDING, &previousFramebuffer);
kzsGlBindFramebuffer(KZS_GL_FRAMEBUFFER, outputFramebuffer);
kzsGlViewport(0, 0, static_cast<int>(m_RT_Width), static_cast<int>(m_RT_Height));  
kzsGlClearColor(0.0f, 0.0f, 0.0f, 0.0f);  
kzsGlClear(KZS_GL_COLOR_BUFFER_BIT);



参数解释：
glGetIntegerv：查询某个 OpenGL 状态变量的整数值，把结果写到你给的变量里

KZS_GL_COLOR_BUFFER_BIT ：
•“当前绑定 framebuffer 的颜色附件”
•也就是当前 FBO 里，所有 GL_COLOR_ATTACHMENT0...N 的颜色输出目标

kzsGlBindFramebuffer 的作用是： 把“当前渲染目标”切到某个 framebuffer。

也就是告诉 OpenGL，后续 DrawElements / Clear / ReadPixels 都输出到这个 FBO

kzsGlViewport 就是 OpenGL 的 glViewport，作用是：

•设置当前渲染目标的视口大小和位置
•决定后续绘制时，NDC 坐标会映射到哪个矩形区域
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTg0OTU4Mjg3MCwtMTA3NTg0MDk4NCwtMT
k0NTM5NDkyNCwyNDQyNjYwNDAsMjExMTQ2MDY4NCwxMjcxMzE3
MTA2LDgwNzUyMzYyNF19
-->