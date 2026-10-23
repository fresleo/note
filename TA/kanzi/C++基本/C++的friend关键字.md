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


注意：`friend` 只是赋予访问权限，并不会让这个函数自动成为类的成员函数。 `Show` 仍然是一个普通的全局函数。

## 2. `friend` 的四个重要特性

① 可以访问私有成员

友元函数可以访问 `private` 和 `protected` 成员，不受普通类外访问限制。

② 友元关系不具有传递性

如果 A 是 B 的友元，B 是 C 的友元，不代表 A 是 C 的友元。

③ 友元关系不具有继承性

如果 A 是 B 的友元，那么 A 不会自动成为 B 的派生类的友元，反过来也一样。

④ 可以声明函数或整个类为友元

-   `friend void Show(A& obj);`：指定一个函数。
    
-   `friend class B;`：允许类 B 的所有成员函数访问类 A 的私有成员。
    

## 3. 友元类的示例

```
class A {
private:
    int value = 100;

    friend class B;
};

class B {
public:
    void Show(A& obj) {
        cout << obj.value;  // 合法
    }
};
```

由于 B 是 A 的友元，B 的成员函数可以直接访问 `A::value`。

但这并不意味着 A 也能访问 B 的私有成员。友元关系是单向授权的。

## 4. 为什么 C++ 需要 `friend`？

假设你正在开发一个汽车 HMI 系统：

-   `Engine` 类管理发动机数据。
    
-   `DebugTool` 类负责调试和输出内部状态。
    
-   你不希望所有外部代码都能访问 `Engine` 的私有数据。
    
-   但你希望 `DebugTool` 可以读取这些数据。
    

这时可以声明：

```
class Engine {
private:
    float temperature = 90.0f;

    friend class DebugTool;
};
```

这样就不必把 `temperature` 改成 `public`，同时又允许特定的调试类访问它。

不过要注意：实际项目中，优先考虑公开的查询接口，例如 `GetTemperature()`。如果随意授予过多友元权限，会削弱类的封装性。

<!--stackedit_data:
eyJoaXN0b3J5IjpbOTEyNjc3ODY0XX0=
-->