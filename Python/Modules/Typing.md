## Description
---
> The [typing](https://docs.python.org/3/library/typing.html) module adds support for type hints. It contains some of the types you will use most often: List, Dict, and Tuple.[^1]


## Union and Optional
---
> Pretty often, you want to accept multiple types. Then you use Union:

```python
from typing import Union


def upcase(s: Union[str, bytes]) -> Union[str, bytes]:
	if isinstance(s, str):
		return s.upper()
	elif isinstance(s, bytes):
		return bytes(x - 0x20 if 0x61 <= x <= 0x7A else x for x in s)
	else:
		raise TypeError("need str or bytes")
```


## List vs Sequence
---
> The type `typing.List` represents `list` . A `typing.Sequence` is “an iterable with random access” as [Jochen Ritzel](https://stackoverflow.com/a/2921465/562769) put it so nicely. For example, a string is a `Sequence[Any]` , but not a `List[Any]` .

```python
from typing import List


def fib_list(n: int = 0) -> List[int]:
	fib_numbers: List[int] = [0, 1]
	for _ in range(n):
		fib_numbers.append(fib_numbers[-1] + fib_numbers[-2])
	return fib_numbers


print(f"fib_list(10) = {fib_list(10)}")
```


## Dict vs Mapping
---
> Similarly to the example List vs Sequence, the `typing.Dict` is meant mainly to represent a `dict` whereas `typing.Mapping` is more general. [Stacksonstacks](https://stackoverflow.com/a/52487800/562769) gives a good answer.

```python
from typing import Dict, Any, List


def get_student_names(students: Dict[str, Dict[str, str]]) -> List[str]:
	final_result: List[str] = []
	
	for _student in students.values():
		student_name = _student.get("name")
		
		if student_name:
			final_result.append()
		
	return final_result


print(f"get_student_names() = {get_student_names({ ... })}")
```


## TypedDict
---
> TypedDict was introduced in [PEP-589](https://www.python.org/dev/peps/pep-0589/) and provides the possibility to denote which keys a dictionary should have and which type the values for those keys have. The example in the PEP shows this well:

```python
from typing import TypedDictclass

Movie(TypedDict):  
    name: str  
    year: int

movie: Movie = {
	'name': 'Blade Runner',  
	'year': 1982
}
```
By default, it must have all of the keys. You can make the keys optional by setting the [totality](https://www.python.org/dev/peps/pep-0589/#totality): `class Movie(TypedDict, total=False)`


---
Tags:
#python/typing

Links:
[^1]: https://medium.com/analytics-vidhya/type-annotations-in-python-3-8-3b401384403d