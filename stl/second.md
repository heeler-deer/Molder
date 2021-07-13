# iterators
## Value Type
&emsp;&emsp;C++包含一个type inference机制。假设一个泛型函数f()需要一个Ⅰ的引数，声明一个Ⅰ的value type,虽然typeof(Ⅰ*)不允许，但我们可以写一个令f()成为转递函数，委托给f_impl(I iter,T t),在f()中调用f_impl(iter,*iter)(其中iter是Ⅰ型别的)   
&emsp;&emsp;C++允许template的全特化以及偏特化，在此，我们需要偏特化：
```
template<class T>
struct iterator_traits<T*>{
    typedef T value_type;
};
```
## Difference Type
&emsp;&emsp;
## Reference/Pointer Type
&emsp;&emsp;
## 处理与tags
&emsp;&emsp;
## 我们联合！
&emsp;&emsp;
## 新组件
# function object
## 一般化
&emsp;&emsp;
## concepts
&emsp;&emsp;
## adapters
&emsp;&emsp;
## 预定义的objects
&emsp;&emsp;