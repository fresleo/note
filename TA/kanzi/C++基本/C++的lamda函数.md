结构：
auto f = []() -> int {
    return 42;
};


说明：
•   []
 - ◦捕获列表。
 - ◦表示这个 lambda 能不能访问外部作用域的变量。 	
 - ◦这里是空的 []，意思是“不捕获任何外部变量”。

•()
 - ◦ 参数列表。
 -  ◦ 这里为空，表示这个 lambda 没有参数。

•-> int
 - ◦ 返回类型。 
 - ◦ 表示这个 lambda 返回 int 类型。

•{ return 42; }
 - ◦ 函数体。
 -  ◦ 这里返回 42。



<!--stackedit_data:
eyJoaXN0b3J5IjpbLTIwNDg5OTYwMDRdfQ==
-->