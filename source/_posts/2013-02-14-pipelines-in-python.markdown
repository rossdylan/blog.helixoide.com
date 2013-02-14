---
layout: post
title: "Pipelines in python"
date: 2013-02-14 16:37
comments: true
categories: [functions, pipelines, python]
---

One of my most recent projects was implementing a easy way to link
functions together in pipelines. This way the output of a function
is piped into the input of another one and so on. Currently I have
been experimenting with different ways of creating pipelines.
Currently I have 3 different types of pipelines defined in
[pipelines.py](https://github.com/rossdylan/utils/blob/master/utils/pipelines.py).
The first two are two different implementations of the same type of pipeline. This type of
pipeline takes in an arbitrary number of functions and then keyword arguments specifying
the other arguments for the functions in the pipeline. The syntax for creating these pipelines looks like this:
``` python
pipeline = PipeLine(
    add1,
    subX,
    stringify,
    subX=(2,),
)
pipeline(10)
```
The first 3 arguments are functions in the pipeline. The last argument is used to add another parameter to a function in the pipeline. This gives the pipeline a bit more flexibility.
To use the pipeline just call it with an initial value to be put through the pipeline.

The first implementation uses a recursive method to generate the pipeline.
``` python Recursive Pipelines https://github.com/rossdylan/utils/blob/master/utils/pipelines.py
def PipeLine(*funcs, **kwargs):
    """
    Given an arbitrary number of functions we create a pipeline where the output
    is piped between functions. you can also specify a tuple of arguments that
    should be passed to functions in the pipeline. The first arg is always the
    output of the previous function.
    """
    def wrapper(*data):
        if len(funcs) == 1:
           combinedArgs = data + kwargs.get(funcs[-1].__name__, tuple())
           return funcs[-1](combinedArgs)
        else:
            combinedArgs = kwargs.get(funcs[-1].__name__, tuple())
            if combinedArgs != ():
               del kwargs[funcs[-1].__name__]
            return funcs[-1](PipeLine(*funcs[:-1], **kwargs)(*data), *combinedArgs)
   return wrapper
```
The second one uses the reduce function in order to link the functions together while passing in their other arguments.
``` python Reduce Based Pipelines https://github.com/rossdylan/utils/blob/master/utils/pipelines.py
def ReducePipeline(*funcs, **kwargs):
    """
    Given an arbitrary number of functions we create a pipeline where the output
    is piped between functions. You can also specify a tuple of arguments that
    should be passed to the functions in the pipeline. The first argument is
    always the output of the previous function. This version uses the reduce builtin
    instead of using recursion.
    """
    def accum(val, func):
        funcArgs = kwargs.get(func.__name__, tuple())
        if hasattr(val, "__call__"):
            return func(val(), *funcArgs)
        else:
            return func(val, *funcArgs)

    def wrapper(*data):
        newFuncs = (partial(funcs[0], *data),) + funcs[1:]
        return reduce(accum, newFuncs)
    return wrapper
```
The final pipeline was an experiment to see if I could link python functions using dot syntax.
So the expected usage would be something like this:
``` python
pipeline = DotPipeline(initialvalue, globals())
result pipeline.someFunction.anotherFunction.finalFunction()
```
The constructor for the pipeline if a little ugly since it needs the globals passed in
however it is super cool that you can just string functions together and the output gets passed along. At some point I want to try and make DotPipeline not need the globals dict passed in.
Here is the code for DotPipeline:
``` python DotPipeline
class DotPipeline(object):
    """
    String together a series of functions using dot syntax.
    give DotPipeline's constructor the starting value, and the globals dict
    and then you can call string functions together
    addOne = lambda x: x+1
    subTwo = lambda x: x-2
    p = DotPipeline(1,globals())
    p.addOne.subTwo() -> 0
    """
    def __init__(self, val, topGlobals):
        self.val = val
        self.topGlobals = topGlobals
    def __getattr__(self, name):
        self.topGlobals.update(globals())
        return DotPipeline(self.topGlobals[name](self.val), self.topGlobals)
    def __call__(self):
        return self.val
```
So those are some pipelines that I have been working on other the last week or so. They are not always super useful but linking functions together using a reusable pipeline is a cool way to transform data.
