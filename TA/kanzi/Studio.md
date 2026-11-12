表达式中加颜色
Color4(1,0,0,1)



在 Kanzi 绑定表达式中，Transform 类属性（如 LayoutTransformation、RenderTransformation）是复合类型，读取时需要按字段访问，写入时用构造函数。
读取字段
在属性引用后面直接追加字段名：
○	{@./LayoutTransformation}.TranslationX
○	{@../Node/RenderTransformation}.TranslationY
○	{@./LayoutTransformation}.ScaleX
○	{@./LayoutTransformation}.RotationZ
可用字段：TranslationX/Y/Z、ScaleX/Y/Z、RotationX/Y/Z、Rotation。
构造整个 Transform
用构造函数生成完整值（常用于整体赋值）：
○	Srt2D(scaleX, scaleY, rotation, translationX, translationY)
○	Srt3D(scaleX, scaleY, scaleZ, rotX, rotY, rotZ, transX, transY, transZ)
○	Matrix4(...) — 需要矩阵时使用
示例：
○	Srt2D(1, 1, 0, {@./SliderValue}, 0)
○	Srt3D(1, 1, 1, 0, 0, {@./Angle}, 0, 0, 0)
常见用法
○	只绑定某个分量（推荐）：在绑定里把 property_field 设为 TRANSLATION_X、SCALE_Y 等，表达式只写数值：{@./LayoutTransformation}.TranslationX * 5
○	整体绑定 Transform：property_field 用 WHOLE_PROPERTY，表达式返回 Srt2D(...) / Srt3D(...)
○	取当前值参与计算：getCurrentValue() 可用于读取被绑定属性自身的当前值
注意 2D 节点用 Node2D.LayoutTransformation，3D 节点用 Node3D.LayoutTransformation / Node3D.RenderTransformation，命名空间不能省略。



只改旋转分量，有两种做法：
方案一：绑定旋转字段（推荐）
把绑定的 property_field 设为 **ROTATION**（或 ROTATION_Z 等单轴），表达式只返回旋转值，其他分量保持不变：
○	整体旋转：ROTATION → Vector3(0, 180, 0)（3D）或单值（2D 用 RotationZ）
○	单轴：ROTATION_Y → 180
⚠️ 注意：可用字段里只有 ROTATION 和 ROTATION_Z，Y 轴单独绑定需在表达式内构造完整旋转。
方案二：整体写回 Transform，保留其余分量
如果必须用 WHOLE_PROPERTY，用 getCurrentValue() 读取当前值再只替换旋转：
○	2D：Srt2D({@./LayoutTransformation}.ScaleX, {@./LayoutTransformation}.ScaleY, 180, {@./LayoutTransformation}.TranslationX, {@./LayoutTransformation}.TranslationY)
○	3D：Srt3D({@./LayoutTransformation}.ScaleX, {@./LayoutTransformation}.ScaleY, {@./LayoutTransformation}.ScaleZ, 0, 180, 0, {@./LayoutTransformation}.TranslationX, {@./LayoutTransformation}.TranslationY, {@./LayoutTransformation}.TranslationZ)
如果只是想静态改值（不做绑定）
直接设 Node3D.LayoutTransformation 属性为 0, 180, 0 对应字段即可，不需要写表达式。
需要我直接给哪个节点设置这个旋转吗？告诉我节点名（或者你确认用当前选中的 Screen / 某个子节点）。

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEzOTI0MjM1LC0xMTUzMjIxNTQsMjY0MT
U3MzQyXX0=
-->