---
title: 浅谈boost.variant的几种访问方式
date: 2016-10-26 22:56:23
categories: boost
tags: 
    - boost
    - variant
    - C++11
---

![此处输入图片的描述][1]

前言
-----

variant类型在C++14并没有加入，在[cppreference][2]网站上可以看到该类型将会在C++17加入，若想在不支持C++17的编译器上使用variant类型，我们可以通过boost的variant类型，variant类型可以表示任意一种类型和any类型有些相似，但还是有些区别，比如说variant支持的类型需提前定义，而any类型不需要，获取any类型的值需要给出原始类型，然而variant类型支持多种方式访问，其中一种就是通过访问者模式来访问，是不需要给出原始类型的，下面将浅谈variant的几种访问方式(个人博客也发表了[《浅谈boost.variant的几种访问方式》][3])。

<!--more-->

使用boost::get
-----

```cpp
boost::variant<int, std::string> v;
v = "Hello world";
std::cout << boost::get<std::string>(v) << std::endl;
```

使用boost::get来访问，需要给出原始类型，并且这样做不安全，若类型错误，程序将会抛出异常。

使用RTTI
-----

```cpp
void var_print(boost::variant<int, std::string>& v)  
{  
    if (v.type() == typeid(int))  
    {  
        std::cout << boost::get<int>(v) << std::endl;  
    }
    else if (v.type() == typeid(std::string))  
    {  
        std::cout << boost::get<std::string>(v) << std::endl;  
    }  
    // Else do nothing
}  
int main()  
{  
    boost::variant<int, std::string> v;
    v = "Hello world";  
    var_print(v);
    return 0;
}  
```

使用RTTI技术可以避免类型访问错误而程序异常的情况，但是这样做有点不优雅，每增加一个类型，都需要修改if-else结构，并且使用RTTI会对程序性能有一定影响。

使用访问者模式
-----

```cpp
class var_visitor : public boost::static_visitor<void>
{
public:
    void operator()(int& i) const
    {
        std::cout << i << std::endl;
    }

    void operator()(std::string& str) const
    {
        std::cout << str << std::endl;
    }
};
int main()  
{  
    boost::variant<int, std::string> v;
    v = "Hello world";  
    boost::apply_visitor(var_visitor(), v);
    return 0;
} 
```

使用该模式，需要定义一个类并继承于boost::static_visitor，在类里面需要重载`()`操作符，通过boost::apply_visitor来访问原始类型的值，这样做还是有些繁琐，每增加一个类型，都需要在var_visitor里面增加一个函数，但比使用RTTI里面的修改if-else结构好得多，因为使用访问者模式至少是遵循开放-封闭原则的，即对写开放，对修改封闭。

使用模板函数
-----

```cpp
class var_visitor : public boost::static_visitor<void>
{
public:
    template<typename T>
    void operator()(T& i) const
    {
        std::cout << i << std::endl;
    }
};
int main()  
{  
    boost::variant<int, std::string> v;
    v = "Hello world";  
    boost::apply_visitor(var_visitor(), v);
    return 0;
} 
```

将`operator()`改成了模板函数的好处就是不用关心variant支持多少类型。

参考资料
-----

[boost官网][4]


  [1]: https://raw.githubusercontent.com/chxuan/images/master/blog/2016/10/variant.jpg
  [2]: http://en.cppreference.com/w/
  [3]: http://chengxuan.me/2016/10/26/%E6%B5%85%E8%B0%88boost.variant%E7%9A%84%E5%87%A0%E7%A7%8D%E8%AE%BF%E9%97%AE%E6%96%B9%E5%BC%8F/
  [4]: http://www.boost.org/doc/libs/1_62_0/doc/html/variant/tutorial.html
