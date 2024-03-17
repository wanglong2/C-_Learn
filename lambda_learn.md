# lambad 表达式学习



### 1. 基本格式

 []()->return type {};

[] 为捕获对象列表       

​			=  捕获所有对象 且为值传递

​			&  捕获所有对象 且为引用   所有包括 lambda所在范围内全部可见的局部变量 包含Lambda 					所在类的this

​			this 函数体{}内可以使用Lambda所在类的成员变量

​			例外  如果有 大部分为值传递 小部分为引用传递的需求  则 [=,&a,&b]

​			注意事项

​					 1. 若为空，则不使用所在函数中的变量

​					 2. 局部变量 与全局变量相对应 简单来说 局部变量-> 函数体中的变量   全局变量->函						数体外   

() 为形参列表  

{} 为函数体实现 

 返回类型看 返回值类型

### 2. 说明符

	#### （1）multable

​			允许函数体修改复制捕获的对象，以及调用其非const成员函数

#### （2）constexpr 

​			显式指定函数调用运算符为 constexpr 函数。此说明符不存在时，若函数调用运算符恰好满足针对 constexpr 函数的所有要求，则它也会是 constexpr；  (C++17 起)

#### (3) consteval

​		指定函数调用运算符为立即函数。不能同时使用 consteval 和 constexpr。(C++20 起)

(对于（2） （3）) 用法暂不了解

```Cpp
void test()
{
    int x = 20;
    int y = 30;
    auto f = [&x,&y]()mutable
            {
        //不加 mutable 则不能修改 值传递的变量
                x = 200;
                y = 300;
            };
    x = y = 0;
    f();
    cout << x << "  " << y << endl;

    int z = 100;
    auto f1 = [z](){return z;};  //此处为 值传递，为复制品 ，故 z = 100;与外边的z无关，外边的z值改变与否与其无关   若为 引用，则捕获的变量与外边的 就是一个地址的内容，外边修改，里边的跟着一起修改
    z = 0;
    int j = f1();
    cout << j<< endl;

}
```

一些学习stl算法时与 lambda相关的应用

```C++
vector<int> v;
void sor_test()
{
    for (int i = 0; i < 30; ++i)
    {
        v.push_back(rand()%30);
    }
    for_each(v.begin(),v.end(),[](int x)->void{
        cout << x << "  ";
    });
    cout << endl;
    //降序排序
    sort(v.begin(),v.end(),[](int x1,int x2)->bool{
        return x1>x2;
    });
    //遍历
    for_each(v.begin(),v.end(),[](int x){
        cout << x << "  ";
    });
    // 升序排序   默认为升序排序
    sort(v.begin(),v.end(),[](int x1,int x2)->bool{
        return x1<x2;
    });
    cout << endl;
    for_each(v.begin(),v.end(),[](int x){
        cout << x << "  ";
    });
    cout << endl;
}
```