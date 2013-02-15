---
layout: post
title: "Function-Currying"
date: 2013-02-15 09:35
comments: true
categories: [functions, python, currying, partials, functools]
---

Recently I have been looking into functional languages such as Haskell and one of the things
I really like about Haskell is function currying / partial application. Python has this as
well.
``` python
from functools import partial
def func(x, y, z):
    return (x, y, z)

part = partial(func, 1)
```
However I find this syntax to be a bit ugly. I would prefer to just be able to call a function with
only a portion of its arguments and have it generate a new function that takes in the other functions.
``` python
def func(x, y, z):
    return (x, y, z)

part = func(1)
```
That syntax just seems a lot cleaner to me. In order to implement this syntax I wrote a small
decorator which can be placed on a function and adds the built in currying functionality.
``` python curry decorator
class curry(object):
    """
    Decorator which takes a int which is the maximum number of args the decorated function can take
    """
    def __init__(self, numArgs):
        self.numArgs = numArgs

    def __call__(self, func):
        if self.numArgs > 0:
            @curry(self.numArgs-1)
            def wrapper(*args, **kwargs):
                if len(args) < self.numArgs:
                    return partial(func, *args)
                else:
                    return partial(func, *args)(**kwargs)
            return wrapper
        else:
            return func
```

This decorator takes in the number of arguments for the function and then recursively wraps the function.
Every time an argument is added to the function it recurses down another layer until all arguments have been added the the function.
It then calls the original function with all its arguments. The final syntax looks like this.
``` python
@curry(3)
def func(x, y, z):
    return (x, y, z)

part1 = func(1)
part2 = part1(2)
result = part2(3)
```
This code and the code for the pipelines mentioned in the previous post are hosted on my [github page](https://github.com/rossdylan/utils).
