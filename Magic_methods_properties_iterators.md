## Magic Methods, Properties, and Iterators

#### Class: new style vs old style

- new style: *Class* NewStyle(object):
- old style: *Class* OldeStyle:
- use \_\_metaclass\_\_ = type to convert old style to new one

 ```python
 __metaclass__ = type
 class NewStyleNow:
  more_code_here
 ```

- \_\_metaclass\_\_ = type locates:
  - at the beginning of the file --> available to all classes
  - in the class scope --> set the metaclass of only that class
- all classes in Python3 are new-style

#### Constructor: \_\_init\_\_(self, args, kwargs)

- called automatically after an instance is created
- used to define the attributes that objects of the same class share

#### Destructor: \_\_del\_\_(self)

- called just before the object is destroyed

#### Overriding constructor

1. rewrite constructor to specify its unique attributes
2. call superclass constructor to retrieve general attributes

- unbound fashion: SuperClass.\_\_init\_\_(self)

 ```python
 class SubClass(SuperClass):
  def __init__(self):
   # by supplying the current instance as the self argument to the unbound method
   # the current instance get all of its superclasses' attributes
   SuperClass.__init__(self)
   self.attr1 = some_value
 ```

- bound fashion: super(SubClass, instance)

 ```python
 class SubClass(SuperClass):
  def __init__(self):
   super().__init__()
   self.attr1 = some_value
 ```

- super function is the one-call solution to multiple superclasses

#### The Basic Sequence and Mapping Protocol

- \_\_len\_\_(self)
  - return the number of items in a collection
  - if \_\_len\_\_ returns 0, the object is treated as false in Boolean context (like empty string, list, etc.)
- \_\_getitem\_\_(self, key)
  - return the value corresponding to the given key
- \_\_setitem\_\_(self, key, value)
  - store value in a manner associated with key
  - mutable objects exclusive
- \_\_delitem\_\_(self, key)
  - delete the element associated with key
  - called when \_\_del\_\_ statement is used on a part of the object
  - mutable objects exclusive

#### Extra requirements on key

- for a sequence, key should be an integer ranging from 0 to n-1
- or a negative integer, x[-n] is the same as len(x)-n
- if key is of an inappropriate type (such as a string key used on a sequence), a TypeError may be raised
- if key is of the right type but outside the allowed range, an IndexError should be raised.

#### Practice: Arithmetic Sequence

create a sequence which is characterized by:

- infinite length
- the value of a item can be modified
- item cannot be delete

#### Subclassing list, dict, and str

- reason: inherit many other useful magic and ordinary methods (such as \_\_iter\_\_, revers, etc)

#### Other magic methods

- see section "Special method names" in Python Reference Manual

#### Properties

- those attributes that are defined through their accessors are often called *properties*
- an alternative to accessors
- implementation:
  - builtin function (actually class) *property*(fget, fset, fdel, doc)

 ```python
 class C(object):
  def getx(self): return self._x
  def setx(self, value): self._x = value
  def delx(self): del self._x
  # a managed attribute is then created as:
  x = property(getx, setx, delx, "I'm the x property")
 # Decorator style
 class C(object):
  def __init__(self):
   self._x
  @property
  def x(self):
   "I am the 'x' property"
   return self._x
  @x.setter
  def x(self, value):
   self._x = value
  @x.deleter
  def x(self):
   del x
 ```

#### Descriptor Protocol

- a descriptor is an **attribute** that has one of the methods in the descriptor protocol:
  - \_\_get\_\_()
  - \_\_set\_\_()
  - \_\_del\_\_()

#### Static Methods and Class Methods

- they are created by *staticmethod* and *classmethod* classes
- static methods are defined without *self* argument
- they can be called directly on the class itself
- class methods are defined with *cls*
- they can be called directly on the class object too
- *cls* is then automatically bound to the class
- two ways to define:

 ```python
 # wrapping and replacing version
 class MyClass(object):

  def smeth():
   print('This is a static method')
  smeth = staticmethod(smeth)

  def cmeth(cls):
   print('This is a class method of', cls)
  cmeth = classmethod(smeth)

 # decorator version
 class MyClass(object):

  @staticmethod
  def smeth():
   print('This is a static method')

  @classmethod
  def cmeth(cls):
   print('This is a class method of', cls)
 ```

#### Iterator Protocol

- an object that implements the \_\_iter\_\_ method is an iterable
- an object that implements the \_\_next\_\_ method is an iterator
- \_\_iter\_\_() returns an iterator, usually the object itself
- \_\_next\_\_() returns an iterator's next value
- If the iterator has no more value to return, it should raise a _StopIteration_ exception
- built-in function next() is equivalent to \_\_next\_\_()
- built-in function iter() can convert an iterable object into an iterator

#### Generator

- generators are iterators that produce the values of the expressions passed to *yield*
- any function that contains a yield statement is called a *generator*
- when a generator is called, the code in the function body is not executed
- instead, a iterator is returned
- how a generator works:

```python
def gen_123():
   yield 1
   yield 2
   yield 3
print(gen_123) # <function gen_123 at 0x...>
print(gen_123()) # <generator object gen_123 at 0x...>
for i in gen_123():
   print(i) # 1, 2, 3
g = gen_123()
print(next(g)) # 1
print(next(g)) # 2
print(next(g)) # 3
print(next(g))
# Traceback (most recent call last):
# ...
# StopIteration
```

#### Practice: Flatten an arbitary tree structure

```python
def flatten(nested):
    """
    flatten a list with arbitary depth
    """
    try:
        try:
            # Don't iterate over string-like objects
            nested + ''
        except TypeError:
            for sublist in nested:
                for element in flatten(sublist):
                    yield element
        else:
            raise TypeError

    except TypeError:
        yield nested


def main():
    l = [1, [2, 3], [4, [5, 6]]]
    print(list(flatten(l)))
    l = ['cyeext', ['wanglingchun', 'mafeng', ['bob']]]
    print(list(flatten(l)))


if __name__ == "__main__":
    main()
```

#### Generator Methods

- send
- throw
- close

#### The Eight Queens

