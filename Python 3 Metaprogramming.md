# Python 3 Metaprogramming

Metaprogramming is code that manipulates code. E.g. Decorators, Metaclasses, Descriptors.
Used in frameworks, libraries.
Prevents highly repetitive code (DRY)

## Definitions
- Statements (perform the actual work of your program)
  - Always execute in 2 scope (globals & locals)
```python
statement1
statement2
statement3
```
- Functions (fundamental unit of code in most programs)
```python
def func(x,y,z):
  statement1
  statement2
  statement3
```
  - 2 forms (Module level functions & Methods of classes)
  - 2 calling conventions
    - Positional arguments : func(1,2,3)
    - Keyword arguments : func(x=1, z=3, y=2)
  - Default arguments
    - Default values set at definition time
    - Only use immutable values (e.g. None)
  ```python
  def func(x, debug=False, names = None):
    if names is None:
      name = []
    ...
  func(1)
  func(1, names = ['x', 'y'])
  ```

  - \*args is tuple of position Args
  - \*\*kwargs is dict of keyword args
```python
def func(*args, **kwargs):
...

func(1,2,x=3,y=4,z=5)
# args = (1,2)
# kwargs = {'x': 3, 'y' : 4, 'z': 5}
```
**Python 3 has keyword-only args [5:57](https://youtu.be/sPiWg5jSoZI?t=5m57s)**
- Closure (You can make and return functions)
- Classes
```python
class Spam:
  a = 1
  def __init__(self, b):
    self.b = b
  def imethod(self):
    pass

  @classmethod
  def cmethod(cls):
    pass

  @staticmethod
  def smethod():
    pass

# Spam.a is a Class variable (will return 1)
# s = Spam(2)
# s.b is an Instance variable
# s.imethod() is an Instance method (executed on instance)
# Spam.cmethod() is a class method (executed on class)
# Spam.smethod() is a static method (a function put in a class)
# Also have special methods to customise :
def __getitem__(self, index):
  ...
```

For inheritance, you take a class, inherit from it then customise it.
```python
class Base:
  def spam(self):
    ...
class Foo(Base):
  def spam(self):
    ...
    #Call method in base class
    r = super().spam()
```
Objects are layered on dictionaries.

## Metaprogramming Basics
### Debugging
The print statement is the one true way to debug! -> you use a decorator (a function that creates a wrapper around another function.)

Emacs :heart:

To do decorators properly:
```python
from functools import wraps

def debug(func):
  #func is function to be wrapped
  @wraps(func) #copies meta data
  def wrapper(*args, **kwargs):
    print(func.__qualname__) #qualname is new in python 3 - starts with classname...
    return func(*args, **kwargs)
  return wrapper
```

Decorators : don't have to care about implementation, can change decorator and not mess with the code.

Can have a decorator that takes prefix - 2 levels of nested functions

To decorate all methods of a class, use a class decorator (walks through the class dictionary, if the value is callable...)

```python
from debugly import debug
  def debugmethods(cls):
    #cls is a class
    for key, val in vars(cls).items():
      if calllable(val):
        setattr(cls, key, debug(val))
    return cls

@debugmethods #only works with instance methods, not class methods and static methods.
class Spam:
  def a(self):
    pass
  def b(self):
    pass
# in the command line
import example
s = example.Spam()
s.a()
#prints Spam.a #the result of the qualname
```

Outermost wrapper output first.

**How to debug all the classes?**
You can define a metaclass. (create the class normally then immediately wrapped by class decorators)

```python
class debugmeta(type):
  def __new__(cls, clsname, bases, clsdict):
    clsobj = super().__new__(cls, clsname, bases, clsdict)
    clsobj = debugmethods(clsobj)
    return clsobj
```

All values in Python have a type, classes define new types. The class is the type of instances created, it is a callable that creates instances.
Classes are instances of typers (classes are types)
Types are their own class (class type, creates new "type" objects and are used when defining classes)

### Deconstruction of classes
Example Class:
```python
class Spam(Base):
  def __init__(self, name):
    self.name = name
  def bar(self):
    print "I'm Spam.bar"
```

Components of Example Class:
- Name = "Spam"
- Base classes = (Base)
- Functions = \_\_init\_\_ , bar

Class definition process:
1. Body of the class is isolated
2. The class dictionary is created, the dictionary serves as local namespace for the statements in the class body.
3. Body is executed in returned dictionary.
4.  class dictionary is populated.
5. Class is contructed from its name, base classes and the dictionary.

You can change the metaclass like so:
```python
class Spam(metaclass=type):
  def __init__(self, name):
    self.name = name
  def bar(self):
    print "I'm Spam.bar"
```
By default it is set to 'type' but you can change it to something else.

### Using a Metaclass
Metaclasses get info about class definitions at the time of definition and they can inspect & modify that data. (essentially like a class decorator.)

Why use a metaclass instead of a class decorator?
- Inheritance
  - Metaclasses propagate down hierarchies

## Overview
Wrapping:
- Decorators : Functions
- Class Decorators : Classes
- Metaclasses : Class hierarchies

Metaclasses capture things about the class before it even gets created while class decorators are after the class has been fully formed.

[Stopped here](https://youtu.be/sPiWg5jSoZI?t=38m27s)
