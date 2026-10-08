创建shader
1：第一步
m_id = kzsGlCreateShader(type);
glShaderSource(vertexShader, 1, &vsSource, NULL);
//type种类有：
KZS_GL_VERTEX_SHADER
KZS_GL_FRAGMENT_SHADER
KZS_GL_TESS_CONTROL_SHADER
KZS_GL_TESS_EVALUATION_SHADER
KZS_GL_GEOMETRY_SHADER
KZS_GL_COMPUTE_SHADER



<!--stackedit_data:
eyJoaXN0b3J5IjpbLTIxMzA5MzQ4NDksLTIwODg3NDY2MTJdfQ
==
-->