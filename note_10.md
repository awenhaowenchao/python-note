# 对象初始化
    
    Python对象初始化，准确的说这一章应该叫Python对象生命周期，描述Python对象从诞生到消亡的过程。

> * 概念 
    __new__创建对象时调用该方法

    __new__通常用于控制生成一个新实例的过程（用于给这个对象分配内存的方法），它是类级别的方法
    __new__至少要有一个参数cls，代表要实例化的类，此参数在实例化时由Python 解释器自动提供
    __new__必须要有返回值，返回实例化出来的实例
    return父类__new__出来的实例，或者直接是object的__new__出来的实例
    通过拦截这个方法, 可以修改对象的创建过程
    da比如:单例设计模式
    
    __init__对象初始化
    
    每个对象实例化的时候，都会自动执行这个方法，__new__生成实例后才执行初始化，它是实例级别方法。
    可以在这个方法里面，初始化一些实例属性
    __del__对象销毁
    
    当对象被释放的时候调用这个方法，如执行`del 实例`，同样会调用该方法
    可在这个方法中清理资源
    
> * 示例
```python
    class Test():
    
        def __new__(cls, *args, **kwargs):
            print("1. 创建实例")
            ins = super().__new__(cls)
    
            return ins
        def __init__(self, *args, **kwargs):
            print("2. 实例赋值")
            print(args)
            print(kwargs)
            pass
        def __del__(self):
            print("3. 再见")
    
    test = Test(12, 234, a=1, b=2)
    print(test)
    print("-----")
    del test
```
    结果如下：

```python
    1. 创建实例
    2. 实例赋值
    (12, 234)
    {'a': 1, 'b': 2}
    <__main__.Test object at 0x10317e7b8>
    -----
    3. 再见
```
> * 注意事项
    
    由于 __new__() 不限于返回同一个类的实例，所以很容易被滥用，不负责任地使用这种方法可能会对代码有害，所以要谨慎使用。一般来说，对于特定问题，最好搜索其他可用的解决方案，最好不要影响对象的创建过程，使其违背程序员的预期。比如说，前面提到的覆写不可变类型初始化的例子，完全可以用工厂方法（一种设计模式）来替代。

## 参考文献
> * [PEP 3107 -- Function Annotations](https://www.python.org/dev/peps/pep-3107/)