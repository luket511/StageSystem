Stage functions
===============

Splitting the program into stages that can be stopped and started again can be useful

One requirement for this system however is to allow for functions to call other prerequisite functions which would need to run first

The implementation has a Metadata class running along side of it that tracks which stages/ functions have been completed and which ones have not. This progress is saved to a metadata json which saves the information

How to define a stage function
==============================

```python
@stage(prerequisite_function, prerequisite_function, ...)
def some_stage():
    #Add anything the stage needs to do here
    functionality()
```

The @stage decorator (see decorators.py) marks the function as a stage to be tracked. It can be passed prerequisite functions as parameters which will run before the program if they haven't run already

How to call a stage function
============================
```python
some_stage(metadata)
```
A stage can be called as a normal function but must be passed the metadata class that runs throughout the program

```python
some_state(metadata, True)
```
The option to force a specific stage to run even if it has been run before is also possible in this implementation by passing "True" as a second parameter

Conditional stage
=================
Sometime a stage should only be run if a certain condition is met. In this case @conditionalStage decorator can be used instead

```python
@conditionalStage(condition_function, prerequisite_function, prerequisite_function, ...)
def another_stage():
    #Add anything the stage needs to do here
    functionality()
```

A `condition_function` must be a function that returns a boolean value and so the stage will only run if it returns True

Check Stage
===========

Sometimes you might need to check if a stage that has been previously processed is still up to date. To to this you can pass a checker_function to the stage which it will run. 
```python
@checkerState(checker_function, prerequisite_function, prerequisite_function, ...)
def another_stage():
    #Add anything the stage needs to do here
    functionality()
```

The `checker_function` should return a boolean value and the stage will run again if the checker_function returns True
