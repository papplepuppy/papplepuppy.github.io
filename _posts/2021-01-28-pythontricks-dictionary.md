---
title: 일반 자료구조-딕셔너리
published: true
category: pythontricks
tags:
  - python
  - dict
---

# 딕셔너리(=맵 or 해시테이블)

* 딕셔너리(dictionary)는 임의의 수의 객체를 저장한다.
* 각 객체는 unique key로 식별(identify)한다.
* 딕셔너리는 map 혹은 hash table로 구성된 자료형(data type)이다.

```
# 기본적인 dict의 선언
>>> test_dict = {
...     'carl': 86,
...     'bob': 65,
...     'zz': 41
... }
>>> type(test_dict)
<class 'dict'>
>>> test_dict
{'carl': 86, 'bob': 65, 'zz': 41}
>>> 

# 이렇게 하는 것도 가능하다.
>>> from_comp = {x: x**2 for x in range(5)}
>>> type(from_comp)
<class 'dict'>
>>> from_comp
{0: 0, 1: 1, 2: 4, 3: 9, 4: 16}
>>> 

# key로 값을 가져올 땐 `dict_variable['key']` 형식을 사용할 수 있다.
>>> test_dict['carl']
86

# dict_variable['key'] 형식으로 업데이트도 가능하다
>>> test_dict['bob'] = 48
>>> test_dict['bob']
48
```

## 아무 값이나 key가 될 수 있을까?
* dict는 hash table이다.
* hash table lookup에는 key가 필수이고, python은 이 key를 hash해서 table을 유지한다.
* 따라서 key는 만드시 **해시 가능해야(hashable)** 한다.
* 해시 가능한 객체는 다음의 특성이 있다.
    * 객체의 수명동안 해시값이 변경되지 않음(`__hash__`)
    * 다른 해시 객체와 비교 가능(`__eq__`)
    * 따라서 immutable 객체는 key로 사용 가능
    * tuple 객체도 해시 가능 타입만 포함하고 있다면 사용 가능

## 특수한 딕셔너리
### `collections.OrderedDict`: 키 삽입 순서 기억
* 일반 dict는 키의 순서를 보장하지 않는다.(하지만 보장하는 것처럼 보일 수 있다)
* 키의 순서(객체 추가 순서)가 중요하다면 `collections.OrderedDict`를 사용하면 좋다.

```
# import 한다
>>> from collections import OrderedDict

# 원형: class collections.OrderedDict([items])
# 각 item을 key=val 형식으로 입력하면 tuple로 구성되는 듯
>>> ordered_dict = OrderedDict(carl=82, bob=66, zz=45)
>>> ordered_dict
OrderedDict([('carl', 82), ('bob', 66), ('zz', 45)])
>>> ordered_dict['asdf'] = 30
>>> ordered_dict
OrderedDict([('carl', 82), ('bob', 66), ('zz', 45), ('asdf', 30)])
>>> 

# OrderedDict는 dict의 subclass이기 때문에 dict에서 쓸 수 있는 메서드는 다 사용 가능
>>> ordered_dict.items()
odict_items([('carl', 82), ('bob', 66), ('zz', 45), ('asdf', 30)])
>>> ordered_dict.keys()
odict_keys(['carl', 'bob', 'zz', 'asdf'])
>>> for key,val in ordered_dict.items():
...     print("key {} value {}".format(key, val))
... 
key carl value 82
key bob value 66
key zz value 45
key asdf value 30
>>> 
```

### `collections.defaultdict`: 누락된 키의 기본값 반환
* 역시 dictk의 subclass이다.
* 접근한 키가 초기화되지 않은 키라면 초기화 시 넘긴 함수(=factory function)를 호출해서 초기화한 후 진행

```
# import 먼저 한다
>>> from collections import defaultdict
>>> 

# defaultdict 객체를 생성하면서 list를 넘겼다.
# 빈 객체에 접근할 때 list()를 호출해 초기화하게 된다.
>>> dd = defaultdict(list)
>>> type(dd)
<class 'collections.defaultdict'>
>>> dd['newone'].append("hi")
>>> dd['newone'].append("there")
>>> dd['newone'].append("everyone")
>>> dd['newone']
['hi', 'there', 'everyone']
>>> 

# 새로 접근하면 바로 list가 된다.
>>> type(dd['new2'])
<class 'list'>
>>> dd['new2']
[]
>>> dd['new3']
[]
>>> 

# 그러나 .get()을 사용하면 None을 리턴한다.
>>> dd.get('absnew')
>>> if dd.get('absnew') is None:
...     print("oops")
... 
oops
>>> 
```

그리고 docs.python.org를 찾아보니 좋은 예제가 있어 옮겨본다.([링크](https://docs.python.org/3/library/collections.html#defaultdict-examples))
```
# 같은 키를 갖는 튜플들을 돌면서 리스트화한다.
>>> s = [('yellow', 1), ('blue', 2), ('yellow', 3), ('blue', 4), ('red', 1)]
>>> d = defaultdict(list)
>>> for k, v in s:
...     d[k].append(v)
... 
>>> sorted(d.items())
[('blue', [2, 4]), ('red', [1]), ('yellow', [1, 3])]
>>> 
```

위의 예제는 일반 dict의 setdefault()를 이용해 아래와 같이 구현할 수 있다.
```
>>> s = [('yellow', 1), ('blue', 2), ('yellow', 3), ('blue', 4), ('red', 1)]
>>> d = {}
>>> for k, v in s:
...     d.setdefault(k, []).append(v)
... 
>>> sorted(d.items())
[('blue', [2, 4]), ('red', [1]), ('yellow', [1, 3])]
>>>
```


### collections.ChainMap: 여러 딕셔너리를 단일 매핑으로 검색
TBD

### types.MappingProxyType: 읽기 전용 딕셔너리를 만들기 위한 래퍼
TBD
