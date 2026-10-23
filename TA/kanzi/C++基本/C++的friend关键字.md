C++ 的 `friend` 关键字叫做友元。它最核心的作用是：允许一个类或函数访问另一个类的 `private`（私有）和 `protected`（受保护）成员。

正常情况下，类外部不能直接访问 `private` 成员，而 `friend` 可以开辟一个特殊的访问权限。

## 1. 基本用法

先看一个例子：

```
class A {
private:
    int value = 100;

    friend void Show(A& obj);
};

void Show(A& obj) {
    cout << obj.value;  // 合法，输出 100
}
```
这里有三个关键点：

-   `value` 是私有成员，普通外部函数不能访问。
    
-   `friend void Show(A& obj);` 声明 `Show` 是类 `A` 的友元函数。
    
-   `Show` 因此可以访问 `A` 的私有成员。
<!--stackedit_data:
eyJoaXN0b3J5IjpbNzEyNTU2MTMyXX0=
-->