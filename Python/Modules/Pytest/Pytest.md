## Console Usage

```shell
    $ pytest [file or directory] [-v (verbose)] [-k expression] [-x (exits on first failure)]
    $ pytest --markers | list all the markers
```

## Fixtures

- Uses a function result to use on others functions has arguments

### Usage:

```python
import pytest

@pytest.fixture
def numbers():
    a = 10
    b = 20
    c = 30
    return [a, b, c]


def test_method1(numbers):
    assert numbers[0] == 10


def test_method2(numbers):
    assert numbers[1] == 25


def test_method3(numbers):
    assert numbers[2] == 30

```

```shell
    $ pytest -v
```

### Result:

```shell
=================================== test session starts ==================================
platform linux -- Python 3.9.7, pytest-6.2.5, py-1.10.0, pluggy-1.0.0 -- /usr/bin/python
cachedir: .pytest_cache
rootdir: /mnt/c/Users/NélioMarcelino/Documents/projects/vantage-towers/tasks/Discover Learn PyTest/fixtures
plugins: pythonpath-0.7.3
collected 3 items

test_fixture.py::test_method1 PASSED                                                [ 33%]
test_fixture.py::test_method2 FAILED                                                [ 66%]
test_fixture.py::test_method3 PASSED                                                [100%]

======================================== FAILURES ========================================
______________________________________ test_method2 ______________________________________

numbers = [10, 20, 30]

    def test_method2(numbers):
        y = 25
>       assert numbers[1] == y
E       assert 20 == 25
E         +20
E         -25

test_fixture.py:19: AssertionError
================================= short test summary info ================================
FAILED test_fixture.py::test_method2 - assert 20 == 25
============================== 1 failed, 2 passed in 0.18s ===============================
```

#

## Test with marks

- Functions starts with a `test_` and whatever comes after, it's used has an alias
- Use `@pytest.mark.` to give an alias to a function
- In `pytests.ini` file, configure all the custom marks with the respective description after `':'`

### Usage

```python
    import pytest

    @pytest.mark.one
    def test_method1(x=5):
        assert x == 5

    @pytest.mark.two
    def test_method2(a=5, b=5):
        assert a == 5 and b == 5
```

### pytests.ini

```ini
    [pytest]
    markers =
        slow: marks tests as slow (deselect with '-m "not slow"')
        one: see if argument 'a' is a 5
        two: see if both arguments are a 5
```

```shell
  $ pytest -k method1 -v
  $ pytest -k two -v
```

#

## Marks special functions

- Use `mark` to use special functions:
  - `@pytest.mark.filterwarnings(warning)`: add a warning filter to the given test
  - `@pytest.mark.skip(reason=None)`: skip the given test function with an optional reason. Example: skip(reason="no way of currently testing this") skips the test
  - `@pytest.mark.xfail(condition, ..., *, reason=..., run=True, raises=None, strict=xfail_strict)`: mark the test function as an expected failure if any of the conditions evaluate to True. Optionally specify a reason for better reporting and run=False if you don't even want to execute the test function. If only specific exception(s) are expected, you can list them in raises, and if the test fails in other ways, it will be reported as a true failure
  - `@pytest.mark.usefixtures(fixturename1, fixturename2, ...)`: mark tests as needing all of the specified fixtures
  - `@pytest.mark.tryfirst`: mark a hook implementation function such that the plugin machinery will try to call it first/as early as possible
  - `@pytest.mark.trylast`: mark a hook implementation function such that the plugin machinery will try to call it last/as late as possible

## Usage

```python
    import pytest

    @pytest.mark.xfail(run=False, reason="test xfail")
    def test_method1(x=5):
        assert x == 5


    @pytest.mark.skip("test skip")
    def test_method2(a=5, b=5):
        assert a == b
```

```shell
    $ pytest -v
```

### Result:

```shell
=============================== test session starts ===============================
platform linux -- Python 3.9.7, pytest-6.2.5, py-1.10.0, pluggy-1.0.0 -- /usr/bin/python
cachedir: .pytest_cache
rootdir: /mnt/c/Users/NélioMarcelino/Documents/projects/vantage-towers/tasks/Discover Learn PyTest/multiple_tests
plugins: pythonpath-0.7.3
collected 2 items

main_test.py::test_method1 XFAIL ([NOTRUN] test xfail)                          [ 50%]
main_test.py::test_method2 SKIPPED (test skip)                                  [100%]

=========================== 1 skipped, 1 xfailed in 0.14s ===========================
```

#

## Parametrize

- Define the parameters keys and values for the function
- `@pytest.mark.parametrize(argnames, argvalues)`: call a test function multiple times passing in different arguments in turn. argvalues generally needs to be a list of values if argnames specifies only one name or a list of tuples of values if argnames specifies multiple names. Example: @parametrize('arg1', [1,2]) would lead to two calls of the decorated test function, one with arg1=1 and another with arg1=2

### Usage

```python
    import pytest

    @pytest.mark.parametrize("x,y,z", [(10, 20, 200), (15, 25, 45)])
    def test_method(x, y, z):
        assert x*y == z


    @pytest.mark.parametrize("x,y", [(10, 20), (15, 15)])
    def test_method2(x, y):
        assert x == y
```

```shell
    $ pytest -v
```

### Result:

```shell
================================== test session starts ===================================
platform linux -- Python 3.9.7, pytest-6.2.5, py-1.10.0, pluggy-1.0.0 -- /usr/bin/python
cachedir: .pytest_cache
rootdir: /mnt/c/Users/NélioMarcelino/Documents/projects/vantage-towers/tasks/Discover Learn PyTest/paramterized
plugins: pythonpath-0.7.3
collected 4 items

test_paramterized.py::test_method[10-20-200] PASSED                                [ 25%]
test_paramterized.py::test_method[15-25-45] FAILED                                 [ 50%]
test_paramterized.py::test_method2[10-20] PASSED                                   [ 75%]
test_paramterized.py::test_method2[15-15] FAILED                                   [100%]

======================================= FAILURES ========================================
_________________________________ test_method[15-25-45] _________________________________

x = 15, y = 25, z = 45

    @pytest.mark.parametrize("x,y,z", [(10, 20, 200), (15, 25, 45)])
    def test_method(x, y, z):
>       assert x*y == z
E       assert 375 == 45
E         +375
E         -45

test_paramterized.py:6: AssertionError
__________________________________ test_method2[15-15] __________________________________

x = 15, y = 15

    @pytest.mark.parametrize("x,y", [(10, 20), (15, 15)])
    def test_method2(x, y):
>       assert x != y
E       assert 15 != 15

test_paramterized.py:11: AssertionError
================================ short test summary info ================================
FAILED test_paramterized.py::test_method[15-25-45] - assert 375 == 45
FAILED test_paramterized.py::test_method2[15-15] - assert 15 != 15
============================== 2 failed, 2 passed in 0.12s ==============================
```
%% Begin Waypoint %%
- **[[Pytest]]**
	- [[Pytest]]
	- [[MyDocs/Python/Modules/Pytest/usage]]

%% End Waypoint %%