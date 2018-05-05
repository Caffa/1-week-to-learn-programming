# Stop Writing Classes
Reduce code and increase features!

When you are only instantiating the class once, and throwing it away... Don't write a class, use a function instead.

If you are always passing the same argument to the function, standard library has: functools

'''python
def greet (greeting, targer):
  return '%s! %s' % (greeting, target)

import functools
greet = functools.parital(greet, 'hola')
greet('bob')
'''
