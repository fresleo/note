***创建shader***

1：第一步
m_id = kzsGlCreateShader(type);
2：第二步
glShaderSource(m_id , 1, &vsSource, NULL);
4：第三步：
glCompileShader(vertexShader);

//type种类有：
KZS_GL_VERTEX_SHADER
KZS_GL_FRAGMENT_SHADER
KZS_GL_TESS_CONTROL_SHADER
KZS_GL_TESS_EVALUATION_SHADER
KZS_GL_GEOMETRY_SHADER
KZS_GL_COMPUTE_SHADER


***创建Program***



<!--stackedit_data:
eyJoaXN0b3J5IjpbMjk3NDI5NjIzLC0yMDg4NzQ2NjEyXX0=
-->