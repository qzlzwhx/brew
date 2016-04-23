# python
##单例模式
```

class Singleton(type):
  def __init__(self, *args, **kargs):
    self.__instance = None
    super.__init(*args, **kargs)
  def __call__(self, *args, **kargs):
    if self.instance is None:
      # python3
      self.__instance = super().__call__(*args, **kargs)
      # python2
      # self.__instance = super(Singleton, cls).__call__(*args, **kargs)
      return self.__instance
    else:
      return self.__instance
python2
class Spam:
  __metaclass__ = Singleton
pyton3
class Spam(metaclass=Singleton):
  pass
```

## python 私有变量
```
python并没有私有变量，只是我们通俗上用_来表示私有变量，实际上他并不是静态语言上的私有变量；
__表示不可被子类继承的变量（比如上边的单例模式）
```
## python2 和python3的super
```
python2的super使用的时候必须需要2个参数（继承父类， self)
python3的super已经默认这么处理，直接super()即可
```

## python 的__prepare__ __new__ __init__ 
python的__prepare__是python3新加的python2并没有这个方法，这个方法在__new__之前执行，返回的是存放类属性的字典，也就是说返回的必须是字典。
__new__在class初始化之前被调用，参数（cls, name, parent, attrs)从字面上理解是调用__new__方法的类，




