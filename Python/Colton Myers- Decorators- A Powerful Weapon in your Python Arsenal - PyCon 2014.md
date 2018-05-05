# [Colton Myers: Decorators: A Powerful Weapon in your Python Arsenal - PyCon 2014](https://www.youtube.com/watch?v=9oyr0mocZTg)

## What is a decorator?

Decorators wrap function
- Add functionality
- Modify behaviour
- Perform setup and tear down
- Diagnostics (timing, etc)

## How are decorators constructed?

Everything in Python is an object so functions are also objects (not true for some other languages)

So python can pass a function to a variable and call that variable:

```python
def myfunc():
  print('hi')

myfunc() #prints 'hi'
f = myfunc
f() #prints 'hi'
```

**Functions can create other functions**

This is a closure, the inner function closes over the value of word which retains the value of 'word' even after we are done executing make_printer.

```python
def make_printer(word):
  def inner():
    print(word)
  return inner

p = make_printer('wow')
p() #prints 'wow'
```
## How do they work?

**Decorators are usually closures**

Decorators are created with classes

```python
#This doesn't do anything, just an example
def my_decorator(wrapped):
  def inner(*args, **kwargs):
    return wrapped(*args, **kwargs)
  return inner

@my_decorator #basically does this : myfunc = my_decorator(myfunc)
def myfunc():
  pass
```

A working example:

```python
#This doesn't do anything, just an example
def shout(wrapped):
  def inner(*args, **kwargs):
    print('B4')
    return wrapped(*args, **kwargs)
    print('After')
  inner.__name__ = wrapped.__name__ # so that when myfunc.__name__ is called, returns myfunc (instead of returning 'inner')
  return inner

@shout #basically does this : myfunc = shout(myfunc)
def myfunc():
  print('such wow!')
```
calling my func will print out:

```python
>>> myfunc()
B4
such wow!
After
```
## The 'right' way to make decorators

**Decorators are versatile** (so should be reusable regardless of how the function being wrapped is like.)
So we use (\*args, \*\*kwargs) - \*args : list of arguments, \*\*kwargs : dictionary of keyword argumetns -   allows us to take any number of positional and/or keyword arguments.

**There is a library called wrapt** (use wrapt to create decorators) that deals with issues surrounding decorators (like the __name__ of the function and allowing for inspection of arguments.)

```python
#Use wrapt like so
import wrapt

@wrapt.decorator #just need this instead of nested variables.
def pass_thr(wrapped, instance, args, kwargs):
  return wrapped(*args, **kwargs)

@pass_thr
def function():
  pass
```
As decorators are not limited to functions, and can be used for classes and objects, the instance passed in allows you to figure out what kind of object is being deocrated and act accordingly.

**Decorators with arguments**
```python
def skipIf(conditional, msg):
  def dec(wrapped):
    def inner(*args, **kwargs):
      if not conditional:
        return wrapped(*args, **kwargs)
      else: #skipping
        print(msg)
      return inner
    return dec


@skipIf(True, 'I hate dogs') #how it is used
def myfunc():
  print('very print')
```

```python
import wrapt

def with_arg(myarg1, myarg2):
  @wrapt.decorator
  def wrapper(wrapped, instance, args, kwargs):
    return wrapped(*args, **kwargs)
  return wrapper


@with_arg(1, 2) #how it is used
def myfunc():
  pass
```

## Usage of Decorators
- Count how many times a function is called
```python
def count(wrapped):
  def inner(*args, **kwargs):
    inner.counter += 1
    return wrapped(*args, **kwargs)
  inner.counter = 0 #start at 0
  return inner


@count
def myfunc():
  pass
#check with myfunc.counter
```

- Time your code
```python
def timer(wrapped):
  def inner(*args, **kwargs):
    t = time.time()
    ret = wrapped(*args, **kwargs)
    print(time.time()-t)
    return ret
  return inner


@count
def myfunc():
  print('so eg')
# >>> myfunc()
#so eg
#4.05311e-06
```
## Qs
- If you were to apply a decorator to a decorated function, it would be like this:

```python
@One
@Two
def myfunc():
  pass
# myfunct = One(Two(myfunc))
```

- Your decorator should return a function.
