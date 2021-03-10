# super()
    
    super() 函数是用于调用父类(超类)的一个方法。
    super() 是用来解决多重继承问题的，直接用类名调用父类方法在使用单继承的时候没问题，但是如果使用多继承，会涉及到查找顺序（MRO）、重复调用（钻石继承）等种种问题。
    MRO 就是类的方法解析顺序表, 其实也就是继承父类方法时的顺序表。

    
> * 示例
```python
    #!/usr/bin/python
    # -*- coding: UTF-8 -*-
    
    class FooParent(object):
        def __init__(self):
            self.parent = 'I\'m the parent.'
            print('Parent')
    
        def bar(self, message):
            print("%s from Parent" % message)
    
    
    class FooChild(FooParent):
        def __init__(self):
            # super(FooChild,self) 首先找到 FooChild 的父类（就是类 FooParent），然后把类 FooChild 的对象转换为类 FooParent 的对象
            # super(FooChild, self).__init__() #2.x
            super().__init__()
            print('Child')
    
        def bar(self, message):
            # super(FooChild, self).bar(message) #2.x
            super().bar(message)
            print('Child bar fuction')
            print(self.parent)
    
    
    if __name__ == '__main__':
        fooChild = FooChild()
        fooChild.bar('HelloWorld')
        print(FooChild.mro())
```

```shell script
    Parent
    Child
    HelloWorld from Parent
    Child bar fuction
    I'm the parent.
    [<class '__main__.FooChild'>, <class '__main__.FooParent'>, <class 'object'>]
```
> * 注意事项
    
    

## 参考文献
> * [PEP 3107 -- Function Annotations](https://www.python.org/dev/peps/pep-3107/)