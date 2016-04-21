# python
##单例模式
```

class Singleton(type):
  def __init__(self, *args, **kargs):
    self.__instance = None
    super.__init(*args, **kargs)
  def __call__(self, *args, **kargs):
    if self.instance is None:
      return super.__call__(*args, **kargs)
    else:
      return self.__instance
python2
class Spam:
  __metaclass__ = Singleton
pyton3
class Spam(metaclass=Singleton):
  pass
```

