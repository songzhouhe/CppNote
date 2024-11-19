> We speak of types related by inheritance as **polymorphic** types, because we can use the "many forms" of these types while ignoring the differences among them --- Quoted from C++ Primer

**多态**意味着我们可以通过统一的流程使用继承体系中的不同类，其能在统一的流程被调用的基础是这些派生类都包含来自基类中的公共元素；而不同派生类之间的差别，则是通过虚函数在派生类中的覆盖，以及派生类自己定义的成员实现的

这就引出了几个关于继承的几个问题
1. 虚函数的定义格式
2. 在运行过程中，如何确定被调用的虚函数的版本，调用不同虚函数版本的具体机制
3. 基类如何控制其成员在派生类中可见性
4. 派生类中的成员变量（包括继承自基类的部分）的初始化顺序和析构顺序
***
## 虚函数的定义格式

- 几个关键字：**virtual (= 0), override, final**
- 可以将基类中的成员函数理解为各派生类中通用的部分，虚函数理解为差异化部分（但并不是必须要差异话，派生类不具体实现的话，还可以使用基类中的默认版本），纯虚函数理解为必须要差异化部分
- 派生类中定义的虚函数应该与基类中的**函数名，参数列表，返回值（除非返回值分别是基类和派生类类型引用/指针）** 一致，如果参数列表不一致的话，实际上发生的是重载(overload)

***
## 在运行过程中，如何确定被调用的虚函数的版本，调用不同虚函数版本的具体机制

- 实际调用的虚函数版本，由运行时 **动态类型(dynamic type)** 决定
- 每个基类/派生类对象内存中会包含一个**虚函数表指针(vitual table pointer)**，指向每个基类/派生类各自的**虚函数表(virtual table)**，运行时通过虚函数表获取到对应版本的虚函数地址之后，再去调用对应的虚函数

***
## 基类如何控制其成员在派生类中可见性

- **访问控制符(access specifier)**，**public**是公开的部分，自然对于派生类也是可见的；**private**是私有部分，只对成员函数和友元可见；希望只对派生类可见，但不公开的部分，被定义为**protected**
- 当派生类继承得到基类的public和protected成员之后，派生类需要决定这些继承得到的成员，对于其后续的派生类是否可见：public，维持这些成员的可见性；proteced，这些成员在后续继承中，只对衍生的派生类可见，继承得到的public成员变为protected；privated，这些成员变为私有的，在后续的继承中对于派生类不可见

***
## 派生类中的成员变量（包括继承自基类的部分）的初始化顺序和析构顺序

- 两个原则：1）派生类是在基类存在的前提上运行的，所以基类先于派生类构造，但晚于派生类析构；2）成员函数体的在成员变量存在的前提上被调用的，所以成员变量在构造函数体被调用前初始化，但在析构函数体被调用后析构
- 继承体系中的初始化/构造顺序：基类成员变量初始化 -> 基类构造函数体被调用 -> 派生类成员变量初始化 -> 派生类构造函数体被调用
- 继承体系中的销毁/析构顺序：派生类析构函数体被调用 -> 派生类成员变量销毁 -> 基类析构函数体被调用 -> 基类成员变量销毁 