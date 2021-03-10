# 函数注解
    
    Python3.0，引入了用于python函数元注解的 Function Annotations 特性。

> * 语法

. 定义函数的时候对参数和返回值添加注解：
```python
    def foobar(a: int, b: "it's b", c: str = 5) -> tuple:
        return a, b, c
```
    a: int 这种是注解参数
    c: str = 5 是注解有默认值的参数
    -> tuple 是注解返回值。
    
    
. 注解的内容既可以是个类型也可以是个字符串，甚至表达式：
```python
    def foobar(a: 1+1) -> 2 * 2:
        return a
```
. 那么如何获取我们定义的函数注解呢？至少有两种办法：

    __annotations__:
```shell
    >>> foobar.__annotations__
    {'a': int, 'b': "it's b", 'c': str, 'return': tuple}
```

    inspect.signature:

```shell
    >>> import inspect
    >>> sig = inspect.signature(foobar)
    >>> # 获取函数参数
    >>> sig.paraments
    mappingproxy(OrderedDict([('a', <Parameter "a:int">), ('b', <Parameter "b:"it's b"">), ('c', <Parameter "c:str=5">)]))
    >>> # 获取函数参数注解
    >>> for k, v in sig.parameters.items():
            print('{k}: {a!r}'.format(k=k, a=v.annotation))     
    a: <class 'int'>
    b: "it's b"
    c: <class 'str'>
    >>> # 返回值注解
    >> sig.return_annotation
    tuple
```   
> * 示例
```python
    # coding: utf8
    import collections
    import functools
    import inspect
    
    
    def check(func):
        msg = ('Expected type {expected!r} for argument {argument}, '
               'but got type {got!r} with value {value!r}')
        # 获取函数定义的参数
        sig = inspect.signature(func)
        parameters = sig.parameters          # 参数有序字典
        arg_keys = tuple(parameters.keys())   # 参数名称
    
        @functools.wraps(func)
        def wrapper(*args, **kwargs):
            CheckItem = collections.namedtuple('CheckItem', ('anno', 'arg_name', 'value'))
            check_list = []
    
            # collect args   *args 传入的参数以及对应的函数参数注解
            for i, value in enumerate(args):
                arg_name = arg_keys[i]
                anno = parameters[arg_name].annotation
                check_list.append(CheckItem(anno, arg_name, value))
    
            # collect kwargs  **kwargs 传入的参数以及对应的函数参数注解
            for arg_name, value in kwargs.items():
               anno = parameters[arg_name].annotation
               check_list.append(CheckItem(anno, arg_name, value))
    
            # check type
            for item in check_list:
                if not isinstance(item.value, item.anno):
                    error = msg.format(expected=item.anno, argument=item.arg_name,
                                       got=type(item.value), value=item.value)
                    raise TypeError(error)
    
            return func(*args, **kwargs)
    
        return wrapper
    
    @check
    def foobar(a: int, b: str, c: float = 3.2) -> tuple:
        return a, b, c

```

> * 场景


## 参考文献
> * [PEP 3107 -- Function Annotations](https://www.python.org/dev/peps/pep-3107/)