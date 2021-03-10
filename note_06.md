# 如何合理使用assert
    assert 的合理使用，可以增加代码的健壮度，同时也方便了程序出错时开发人员的定位排查。

> * 定义
    
    assert 语句，可以说是一个 debug 的好工具，主要用于测试一个条件是否满足。如果测试的条件满足，则什么也不做，相当于执行了 pass 语句；如果测试条件不满足，便会抛出异常 AssertionError，并返回具体的错误信息（optional）。它的具体语法是下面这样的：

```python
    assert 1 == 2,  'assertion is wrong'
```    

    它就相当于下面这两行代码：

```python
    if __debug__:
        if not expression1: raise AssertionError(expression2)
```    
    __debug__是一个常数，解释器开始运行时就已经决定了__debug__是True，当运行时加上-O这个选项，__debug__是False，导致所有的 assert 语句都失效。需要注意的是，直接对常数__debug__赋值是非法的。

> * 示例
    
    示例 1，用来检查折后价格，这个值必须大于等于 0、小于等于原来的价格，否则就抛出异常
```python
    def apply_discount(price, discount):
        updated_price = price * (1 - discount)
        assert 0 <= updated_price <= price, 'price should be greater or equal to 0 and less or equal to original price'
        return updated_price
```

    示例 2，规定销售数目必须大于 0，这样就可以防止后台计算那些还未开卖的专栏的价格
```python
    def calculate_average_price(total_sales, num_sales):
        assert num_sales > 0, 'number of sales should be greater than 0'
        return total_sales / num_sales
```
    示例 3，input必须是list
```python
    def func(input):
        assert isinstance(input, list), 'input must be type of list'
        # 下面的操作都是基于前提：input必须是list
        if len(input) == 1:
            ...
        elif len(input) == 2:
            ...
        else:
            ... 
```

> * 错误示例
    示例 1，如果你想打开一个文件，进行数据读取、处理等一系列操作，那么下面这样的写法，显然也是不正确的。
```python    
    def read_and_process(path):
        assert file_exist(path), 'file must exist'
        with open(path) as f:
          ...
```

> * 总结
    
    assert 并不适用 run-time error 的检查。比如你试图打开一个文件，但文件不存在；再或者是你试图从网上下载一个东西，但中途断网了了等等，这些情况下，还是应该参照我们前面所讲的错误与异常的内容，进行正确处理。
    
    
## 参考文献

> * [python 规范篇 如何合理使用 assert](https://www.cnblogs.com/hiyang/p/12634744.html)