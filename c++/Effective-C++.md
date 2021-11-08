## 

- 构造函数使用explicit来避免隐式类型转换
- 声明新对象时使用“=”会调用拷贝构造函数



## Item 1 : View c++ as a federation of languages

- C++ = "C" + "Object-Oriented C++" + "Template C++" + "STL"



## Item 2 : Prefer const, enum, inline to \#define

- 常量使用const, enum
- 函数使用inline



## Item 3 : Use const whenever possible

- pass by reference-to-const
- 编译器只考虑bitwise constness，我们需要考虑conceptual constness（如避免改变指针指向的值）



## Item 4 : Make sure that objects are initialized before they're used

- 在参数列表处初始化，初始化次序和声明次序一致
- 避免静态变量互相初始化



## Item 5 : Know what functions C++ silently writes and calls

- ```c++
  class A{
  public:
      A();
      A(const A&);
      ~A();
      
      A& operator=(const A&); //shallow copy
  }
  ```

- 有reference/const成员变量时，默认赋值运算是非法的



## Item 6 ： Explictly disallow the use of compiler-generated function you do not want

- 使用privte
- 使用=delete



## Item 7 : Declare destructors virtual in polymorphic base classes

- 多态的基函数析构函数必须是虚函数，否则可能会有内存泄漏（局部销毁)
- 如果不用做基类就没必要，可以避免生成虚函数表



## Item 8 : Prevent exceptions from leaving destructors

- 不要用析构函数吐出异常，可能在销毁数组时报出多个异常



## Item 9 : Never call virtual functions during construction or destruction

- base class构造时还没有虚函数表，此时没有多态
- 进入base class析构函数时对象变为base class对象



## Item 10 : Have assignment operators return a reference to *this

- 方便连锁赋值`x = y = z;`



## Item 11 : Handle assign to self in operator=

- copy and swap
- move



## Item 12 : Copy all parts of an object

- 拷贝构造时在参数列表调用基类的拷贝构造函数
- 用`init()`实现拷贝构造和赋值构造中重复的功能



## Item 13 : Use objects to manage resources

- RAII，多使用智能指针
- 在析构时释放资源



## Item 14 : Think carefully ablout copying buhavior in resource-managing classes

- 抑制copying、计数



## Item 15 : Provide access to raw resources in resource-managing class

- 提供取得原始资源的方法
- 考虑显式和隐式访问，前者安全后者方便



## Item 16 : Use the same form in corresponding uses of new and delete

- delete []



## Item 17 : Store newed objects in smart pointers in standalone statments

- 和new语句在同一个序列点内的语句执行顺序是未定义的，可能会因为异常中断导致资源泄漏



## Item 18 ： Make interfaces easy to use correctly and hard to use incorrectly

- 建立防止误用的方法，如限制类型



## Item 19 : Treat class design as type design

- 对象怎么被创建和销毁
- 初始化和赋值的区别
- pass-by-value的实现
- 变量的合法值
- 继承图系
- 隐式/显式转换
- 操作符和函数
- 需要的标准函数
- 成员的public、protected、private
- 未声明接口
- 一般化（模板）
- 真的需要定义type吗



## Item 20 ：Prefer pass-by-reference-to-const to pass-by-value

- 可以有效减少类内对象的拷贝



## Item 21 : Don't try to return a reference when you must return an object

- 返回的指针或引用通常指向一篇被销毁的内存



## Item 22 ：Declare data members private

- 便于数据访问可控以及封装性



## Item 23 ： Prefer non-member non-friend fuction to member functions

- 有时候非成员函数更能保证封装性



## Item 24 : Declare non-member functions when type conversions should apply to all parameters

- 当被this指针指向的参数也需要隐式类型转换时，最好声明非成员函数



## Item 25 : Consider support for a non-throwing swap

- public swap/namespace::non-member swap
- 使用using std::swap方便使用最佳的swap版本
- 可以对std template全特化，但不要额外添加



## Item 26 : Postpone vairable definitions as long as possible

- 中途有异常时可以减少构造析构的成本
- 方便直接调用构造函数而不是使用默认值



## Item 27 : Minimize casting

- 尽量避免转型，多使用`C++style`转型



## Item 28 : Avoid returning "handles" to obeject internals

- 破坏封装性



## Item 29 : Strive for exception-safe code

- 当异常抛出时，要求：不泄漏资源、数据一致性
- 异常安全函数提供以下三个保证之一：
  - 基本承诺：数据一致，但状态不确定
  - 强烈保证：异常后回滚
  - no throw保证



## Item 30 : Understanding the ins and outs of inlining

- 可能导致代码膨胀
- 虚函数、函数指针时不会inline
- inline函数修改时，程序需要重新编译



## Item 31 : Minimize compilation dependencies between files

- 声明而不定义



## Item 32 : Make sure public inheritance models "is-a"

- public 继承即是 is-a 模式
- 不是所有现实中的is-a都能被程序准确描述



## Item 33 : Avoid hiding inherited names

- 派生类的同名函数会直接覆盖基类函数（即使参数列表不同），可以通过using来解决这一问题



## Item 34 : Differentiate between inheritance of interface and inheritance of implementation

- 纯虚函数也可以实例化



## Item 35 : Consider alternatives to virtual functions

- **NVI**(non-virtual interface)：基类的实现中定义非虚函数调用虚函数
- 使用函数指针/伪函数来实现运行时可变的效果



## Item 36 : Never redefine an inherited non-virtual function

- 非虚函数是静态绑定的



## Item 37 : Never redefine a function's inherited default parameter value

- 默认参数值是静态绑定的（提高运行期效率）



## Item 38 : Model "has-a" or "is-implemented-in-terms-of" throught composition

- 考虑应该继承还是复合



## Item 39 : Use private inheritance judiciously

- 私有继承时不会默认将派生类转为基类类型
- 私有继承相当于继承实现部分，不考虑接口
- **EBO**(empty base optimization)：继承空白类，此时空白类不需要额外占位空间



## Item 40 : Use multiple inheritance judiciously

