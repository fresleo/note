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


    GLint prevFbo = 0;
    kzsGlGetIntegerv(KZS_GL_FRAMEBUFFER_BINDING, &prevFbo);
    
    ensureOutputTexture(width, height); // 创建/复用纹理
    ensureFbo();                        // 创建/复用FBO
    
    kzsGlBindFramebuffer(KZS_GL_FRAMEBUFFER, m_fbo);
    kzsGlFramebufferTexture2D(KZS_GL_FRAMEBUFFER, KZS_GL_COLOR_ATTACHMENT0, KZS_GL_TEXTURE_2D, m_texId, 0);
    
    if (kzsGlCheckFramebufferStatus(KZS_GL_FRAMEBUFFER) != KZS_GL_FRAMEBUFFER_COMPLETE)
    {
        kzsGlBindFramebuffer(KZS_GL_FRAMEBUFFER, prevFbo);
        return;
    }
    
    kzsGlViewport(0, 0, width, height);
    kzsGlClearColor(0,0,0,0);
    kzsGlClear(KZS_GL_COLOR_BUFFER_BIT);
    
    m_program->apply();
    DrawQuad();
    m_program->unapply();
    
    kzsGlBindFramebuffer(KZS_GL_FRAMEBUFFER, prevFbo);
    
    // 关键：把 render target 对应的 Kanzi Texture 资源写到属性
    setProperty(OutPutTextureProperty, static_pointer_cast<Resource>(m_outputTexture));

<!--stackedit_data:
eyJoaXN0b3J5IjpbODA3NTIzNjI0XX0=
-->