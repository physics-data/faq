## python3 中 "is" 和 "==" 的区别

进行如下尝试：

```python
# (1) 数据类型：列表/元组/浮点数等
a = []
b = []
a is b
# False
a == b
# True

a = 0.1
b = 0.1
a is b
# False
a == b
# True

# (2) 数据类型：[-5, 256] 区间的整数
a = 233
b = 233
a is b
# True
a == b
# True
```

尝试 (1) 进行 `is` 判断时返回为 `False` 的原因：

> **`is` 用于判断两个变量引用对象是否为同一个，`==` 用于判断引用变量的值是否相等.**

尝试 (2) 对于[-5, 256] 区间的整数进行 `is` 判断时返回为 `True` 的原因：

> 在 CPython 的实现中，为了性能考虑，把 [-5, 256] 区间的数字进行了缓存，称为 **small integer caching**, 所以这些范围的相同数字实质上都是同一个对象。**虽然有这样的特性，依旧不可以使用 `is` 判断对象的逻辑相等性.**

具体说明可见下面的网页：

https://stackoverflow.com/questions/15171695/whats-with-the-integer-cache-maintained-by-the-interpreter

https://medium.com/techtofreedom/3-facts-of-the-integer-caching-in-python-20ce587f09bb

p.s. 在两个网页中都有提到，在交互式中把两个赋值放在一行时，会进行更多优化，例：

```python
a = 257; b = 257
a is b
# True
```

但当我在 python 3.9.2 (IPython 7.26.0) 下运行上述代码，输出仍为 `False`. 这符合了第二个网页中给出的结论--python 版本/实现不同, 对同一特性可能有不同机制。

第二个网页中的结论不错，摘录如下作为本问题的总结：

> **Conclusion**

> Python has many optimization mechanisms internally, we should understand how it works and affects our code. The integer caching feature is useful and necessary to reduce time and memory costs. There are three facts under the hood:

> 1. The integers in the range of [-5, 256] are singletons in Python and can’t be recreated again.

> 2. If the Python compiler can see the whole code, more optimisations may apply to the code.

> 3. Different implementations and versions of Python may have different mechanisms for the same feature.


注：尝试 (2) 的解释 (及参考网页) 参照了助教陈晟祺在 issue 上提供的解答，非常感谢！



