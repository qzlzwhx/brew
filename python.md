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


## python的__new__
python2和python3在重载写__new__方法的时候调用super方式：
经典列子：
```
*args和**kwargs的强制参数签名
可以通过使用自定义元类来创建签名对象
from inspect import Signature, Parameter

def make_sig(*names):
    parms = [Parameter(name, Parameter.POSITIONAL_OR_KEYWORD)
            for name in names]
    # 抓住这些参数
    return Signature(parms)

class StructureMeta(type):
    def __new__(cls, clsname, bases, clsdict):
        # 元类重载__new__方法，clsdict即当前类的属性，
        # clsdict['__signature__']给当前类增加__signature__属性(通过元类默认添加)， 属性值是什么？
        # *clsdict.get('_fields',[])从clsdict拿_fields属性的只，_fields是以StructureMeta为元类的类的属性
        # 
        clsdict['__signature__'] = make_sig(*clsdict.get('_fields',[]))
        return super().__new__(cls, clsname, bases, clsdict)

class Structure(metaclass=StructureMeta):
    _fields = []
    def __init__(self, *args, **kwargs):
        bound_values = self.__signature__.bind(*args, **kwargs)
        for name, value in bound_values.arguments.items():
            setattr(self, name, value)

# Example
class Stock(Structure):
    _fields = ['name', 'shares', 'price']

class Point(Structure):
    _fields = ['x', 'y']
```


## python的 super
python2 和python3的区别有：
调用方式，python2
super(当前类名， cls[一般是self,但是在__new__方法内调用super的时候就是需要__new__的第一个参数当前类对象])
python3
不需要任何参数，直接super()即可。

## python 避免重复定义属性方法
此方法用到了python的property装饰器，如果想将一个方法变成类对象的属性，那么需要是新式类，python3默认都是新式类。
```
如果出现很多如下重复的代码，那么我可能需要重构一下代码了
class Person:
    def __init__(self, name ,age):
        self.name = name
        self.age = age

    @property
    def name(self):
        return self._name

    @name.setter
    def name(self, value):
        if not isinstance(value, str):
            raise TypeError('name must be a string')
        self._name = value

    @property
    def age(self):
        return self._age

    @age.setter
    def age(self, value):
        if not isinstance(value, int):
            raise TypeError('age must be an int')
        self._age = value
        
```
写一个方法：
```
def type_property(name, expected_type):
  storage_name = '_' + name
  @property
  def prop(self):
    return self.getattr(storage_name)
  @prop.setter
  def prop(self, value):
    if not isinstance(value, expected_type):
      raise TypeError('Argument{} must be {} type'.format(storage_name, expected_type))
    setattr(self, storage_name, value)
  return prop
# python2必须是新式类才可以
class Person(object):
  name = type_property('name', str)
  age = type_property('age', int)
  def __init__(self, name, age):
    self.name = name
    self.age = age
# 还可以进一步简略代码，使用functools的partial
String = functools.partial(type_property, expected_type=str)
Integer = functools.partial(type_property, expected_type=int)
class Person(object):
  name = String('name')
  age = Integer('age')
  def __init__(self, name, age):
    self.name = name
    self.age = age
```
## python urllib于world wide web 通信的模块
python3已经将python2的urllib拆成urllib.request, urllib.parse,urllib.error另外python3删除了urllib中的urlopen，
而是等价于urllib2中的urlopen.





