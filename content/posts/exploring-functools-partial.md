---
title: "Exploring functools.partial"
date: 2025-08-11
draft: false
ShowToc: false
---

## Introduction

The goal of today's post is to dig into the Python's `functools.partial` object.

## Partial Basics

Let's start by importing `partial` from Python's built-in `functools`.

```python
>>> from functools import partial
```

Next, we define a simplistic `multiply` method and a `times_two` `partial` object which fixes the multiplier to always be `2`.

```python
>>> def multiply(a: int, b: int) -> int:
...     return a * b

>>> times_two = partial(multiply, b=2)

>>> times_two(3)
6
```

When inspecting the `__name__` and `__doc__` fields that regular Python methods always provide, we notice that for partial objects `__name__` does not exists and `__doc__` is set to some generic `partial` object default.
So to have sensible values for the two fields, we have to set those ourselves.

```python
>>> times_two.__name__
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'functools.partial' object has no attribute '__name__'. Did you mean: '__ne__'?

>>> times_two.__name__ = 'times_two'
>>> times_two.__name__
'times_two'

>>> times_two.__doc__
'partial(func, *args, **keywords) - new function with partial application\n    of the given arguments and keywords.\n'

>>> times_two.__doc__ = 'Multiply the given value times two.'
>>> times_two.__doc__
'Multiply the given value times two.'
```

When printing our `times_two` partial we get the name of the class together with a reference to the original method and the arguments that we've fixed for our `partial`.

```python
>>> times_two
functools.partial(<function multiply at 0x101358900>, b=2)
```

 We can also get those latter two explicitly, using the `.func` and `keywords` fields of `partial`.

```python
>>> times_two.func
<function multiply at 0x101358900>

>>> times_two.keywords
{'b': 2}
```

If we had used `partial` with non-keyword arguments (like `partial(multiply, 2)`, which would've set `a` to `2`), those values would show up in the `.args` field.
Since we've only used keyword arguments however, `args` will be an empty tuple in our case.

```python
>>> times_two.args
()
```

## Partial With Types

If we ask Python for the type of the partial, it will simply tell us it's a `functools.partial` object.

```python
>>> type(times_two)
<class 'functools.partial'>
```

And while we can get the argument names with their types as well the return type from a regular Python method using `<method_name>.__annotations__`, this dunder method is not provided for `partial` objects.

```python
>>> times_two.__annotations__
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'functools.partial' object has no attribute '__annotations__'
```

However, we can retrieve the same output using the simple dict comprehension below.

```python
>>> {k: v for k, v in times_two.func.__annotations__.items() if k not in times_two.keywords}
{'a': <class 'int'>, 'return': <class 'int'>}
```
