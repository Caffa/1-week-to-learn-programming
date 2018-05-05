# Stop Writing Classes
[Video](http://pyvideo.org/pycon-us-2012/stop-writing-classes.html)
*Reduce code and increase features!*

1. When you are only instantiating the class once, and throwing it away... Don't write a class, use a function instead.

**When you are writing a class with only 2 methods, and one of them is init... Don't write a class, use a function instead.**

```python
def greet (greeting, targer):
  return '%s! %s' % (greeting, target)

#If you are always passing the same argument to the function, standard library has: functools
import functools
greet = functools.parital(greet, 'hola')
greet('bob')
```
2. Namespaces are for preventing name collisions not taxonomies

- Can use standard exceptions instead of giving them unique names.
- You generally don't need to write that many exceptions for python.

3. Sometimes you do want classes (for Containers), like when you are always operating on the same thing (e.g.  Heap which has many functions operating on the same object)
