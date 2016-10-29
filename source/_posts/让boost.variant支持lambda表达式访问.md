---
title: 让boost.variant支持lambda表达式访问
date: 2016-10-29 21:13:43
categories: boost
tags: 
    - boost
    - variant
    - C++11
    - lambda
---

![此处输入图片的描述][1]

前言
-----

之前写个过一篇博客叫[《浅谈boost.variant的几种访问方式》][2]，里面讲到了可以通过访问者方式来获取variant的值，但是在重载函数`operator()`里面只能够获取variant的值，如果要捕获外部变量或调用外部函数比较麻烦，那么有没有一种方法来简化variant的访问呢？当然有，下面我们让variant支持lambda表达式访问。

<!--more-->

代码
-----

```cpp
#include <iostream>
#include <string>
#include <utility>
#include <type_traits>
#include <boost/variant.hpp>

template<typename Function, typename... Args>
struct make_overload_impl : make_overload_impl<Function>::type, 
                            make_overload_impl<Args...>::type
{
    using type = make_overload_impl;
    using make_overload_impl<Function>::type::operator();
    using make_overload_impl<Args...>::type::operator();
    constexpr explicit make_overload_impl(Function&& func, Args&&... args)
        : make_overload_impl<Function>::type(std::forward<Function>(func)),
        make_overload_impl<Args...>::type(std::forward<Args>(args)...)
    {}
};

template<typename Function>
struct make_overload_impl<Function>
{
    using type = Function;
};

template<typename Return, typename... Args>
struct make_overload_impl<Return(*)(Args...)>
{
    using type = make_overload_impl;
    using Function = Return(*)(Args...);
    constexpr explicit make_overload_impl(const Function&& func) : _func(func) {}
    constexpr Return operator()(Args&&... args) const
    {
        return _func(std::forward<Args>(args)...);
    }

private:
    Function _func;
};

struct make_overload
{
    template<typename... Function, typename Overload = 
        typename make_overload_impl<typename std::decay<Function>::type...>::type>
    constexpr Overload operator()(Function&&... func) const
    {
        return Overload(std::forward<Function>(func)...);
    }
};

template<typename... Args>
auto make_visitor(Args&&... args)
{
    return make_overload()(std::forward<Args>(args)...);
}

int main()
{
    auto visitor = make_visitor
    (
        [](int& i) { std::cout << i << std::endl; },
        [](std::string& i) { std::cout << i << std::endl; }
    );
    boost::variant<int, std::string> v;
    v = "Hello world";
    boost::apply_visitor(visitor, v);
    v = 100;
    boost::apply_visitor(visitor, v);

    return 0;
}
```

该代码也上传到了我的[github][3]。

参考资料
-----

[让BOOST.VARIANT的VISIT支持LAMBDA][4]


  [1]: https://raw.githubusercontent.com/chxuan/images/master/blog/2016/10/variant2.jpg
  [2]: http://chengxuan.me/2016/10/26/%E6%B5%85%E8%B0%88boost.variant%E7%9A%84%E5%87%A0%E7%A7%8D%E8%AE%BF%E9%97%AE%E6%96%B9%E5%BC%8F/
  [3]: https://github.com/chxuan/samples/tree/master/variant_visitor
  [4]: http://purecpp.org/?p=982
