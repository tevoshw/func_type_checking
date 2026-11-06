# Python's `isinstance()`

## What it is

`isinstance(obj, classinfo)` is a **built-in function** in Python. It is a **predicate** (it returns `True` or `False`) used for **type introspection**: examining the type of an object at runtime.

Because it is built in, it requires no `import`. Other functions in the same group include `len`, `print`, `type`, `range`, and `sorted`.

## Syntax

```python
isinstance(obj, classinfo)
```

- `obj`: the object to check.
- `classinfo`: a class, a tuple of classes, or a union type (e.g. `int | str`).

Returns `True` if `obj` is an instance of `classinfo` or of any subclass of it. Otherwise returns `False`.

## Basic usage

```python
x = 42

isinstance(x, int)    # True
isinstance(x, str)    # False
isinstance(x, float)  # False
```

## Checking multiple types

Pass a tuple (or, in Python 3.10+, a union type):

```python
isinstance(x, (int, float))  # True if x is int OR float
isinstance(x, int | float)   # equivalent, Python 3.10+
```

## Common usage in conditionals

```python
def process(x):
    if isinstance(x, str):
        return x.upper()
    elif isinstance(x, (list, tuple)):
        return len(x)
    else:
        raise TypeError("Unsupported type")
```

## `isinstance()` vs. `type()`

`isinstance()` respects inheritance. `type() ==` does not.

```python
class Animal: pass
class Dog(Animal): pass

d = Dog()

isinstance(d, Animal)  # True  (Dog inherits from Animal)
type(d) == Animal      # False (the exact type is Dog)
```

This is why `isinstance()` is the recommended approach in most cases.

## Gotchas

- **`bool` is a subclass of `int`**, so `isinstance(True, int)` returns `True`.
- **Abstract base classes** let you check behavior instead of concrete types:

  ```python
  from collections.abc import Iterable

  isinstance([1, 2, 3], Iterable)  # True
  isinstance(42, Iterable)         # False
  ```

- **Overuse can hurt design.** Python favors *duck typing* ("if it walks like a duck..."). Excessive type checks can make code rigid. Prefer them at API boundaries, input validation, and when behavior genuinely differs per type.

## Common uses in machine learning code

```python
import numpy as np
import torch.nn as nn

if isinstance(X, np.ndarray):
    X = torch.from_numpy(X)

if isinstance(model, nn.Module):
    model.eval()
```

## How it differs from other Python concepts

| Concept | What it is | Example |
|---|---|---|
| **Built-in function** | A function that ships with Python | `isinstance(x, int)` |
| **Iterator** | An object that yields values one at a time (`__iter__` / `__next__`) | `iter([1, 2, 3])` |
| **Decorator** | A function that wraps and modifies another function, applied with `@` | `@property` |

## Related functions

- `type(obj)`: returns the exact type of an object.
- `issubclass(cls, classinfo)`: checks class relationships instead of instances.
- `hasattr(obj, name)`: checks whether an object has an attribute.
- `callable(obj)`: checks whether an object can be called.

## Equivalents in other languages

- **Java:** `instanceof`
- **C#:** `is`
- **JavaScript:** `instanceof` (for objects), `typeof` (for primitives)