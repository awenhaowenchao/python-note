# __call__
    
    python中，__call__的存在重载了 () 运算符，既可以让类实例对象像调用普通函数那样以“对象名()”的形式使用，又可以
    让可调用对象（bound method 或 普通fuction）以"instance.method.__call__() 或 func.__call__()"调用。
    
        注意：实例属性不是可调用对象，通过hasattr(obj.xxx, "__call__")是否为Ture可以判断obj.xxx是方法还是属性。
    

    
> * 示例
```python
    class OBJ():
        # __metaclass__ = Metaclass
        def say(self):
            print("hello world!")
            pass
        def __call__(self, *args, **kwargs):
            print(args)
        pass
    
    print(hasattr(OBJ().say, "__call__"))
    print(OBJ().say)
    print(dir(OBJ().say))
    print(OBJ().say.__repr__.__call__)
    
    obj = OBJ()
    obj("hi susan")
    obj.__call__("hi susan")
    
    def say():
        print("hello world!")
    say()
    say.__call__()
    
    print(say)
```

```shell
    True
    <bound method OBJ.say of <__main__.OBJ object at 0x101f61518>>
    ['__call__', '__class__', '__delattr__', '__dir__', '__doc__', '__eq__', '__format__', '__func__', '__ge__', '__get__', '__getattribute__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__le__', '__lt__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__self__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__']
    <method-wrapper '__call__' of method-wrapper object at 0x101f61550>
    ('hi susan',)
    ('hi susan',)
    hello world!
    hello world!
    <function say at 0x101ed9ea0>
```    
