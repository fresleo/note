***创建shader***

1：第一步
m_id = kzsGlCreateShader(type);
2：第二步
glShaderSource(m_id , 1, &vsSource, NULL);
3：第三步：
glCompileShader(vertexShader);
4：验证
int success = 0;  
kzsGlGetShaderiv(m_id,KZS_GL_COMPILE_STATUS, &success);

//shader的type种类有：
KZS_GL_VERTEX_SHADER
KZS_GL_FRAGMENT_SHADER
KZS_GL_TESS_CONTROL_SHADER
KZS_GL_TESS_EVALUATION_SHADER
KZS_GL_GEOMETRY_SHADER
KZS_GL_COMPUTE_SHADER


***创建Program***

1：第一步
m_id = kzsGlCreateProgram();
2：第二步
kzsGlAttachShader(m_id, shader->getId());
3：第三步：
kzsGlLinkProgram(m_id);
4：验证：
int success = 0;  
kzsGlGetProgramiv(m_id, KZS_GL_LINK_STATUS, &success);

<!--stackedit_data:
eyJoaXN0b3J5IjpbMzI0NjE3MTUyLDI5NzQyOTYyMywtMjA4OD
c0NjYxMl19
-->