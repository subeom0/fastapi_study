## https://docs.python.org/3/library/enum.html#enum.Enum

enum을 사용하는 이유는 코드를 클래스단위로 묶어서 관리함으로써 가독성을 높여준다.

enum클래스를 상속시켜 사용
~~~python
from enum import Enum 
class a(Enum):
  a = 3
  
print(a.a.name, print(a.a.value)
~~~
~~~
a 3 
~~~

auto()를 이용해 값 할당가능
~~~python
class a(str, Enum):
    a = enum.auto()
    b = enum.auto()

print(a.a.value, a.b.value)
~~~
~~~
1 2
~~~

순회가 가능해서 반복문으로 출력이 가능
~~~python
class a(str, Enum):
    a = enum.auto()
    b = enum.auto()

for i in a:
    print(i)
    print(i.name)
    print(i.value)
~~~
~~~
a.a
a
1
a.b
b
2
~~~

enum으 상속받아 클래스를 만들 때 자료형을 파라미터(metaclass라고 원서에 적혀있었다)로 줄 수 있다. 그러면 클래스내의 변수값이 해당자료형으로 바뀌고, 바꿀 수 없을 때 오류가 발생한다.
*어디쓰는건진 모르겠다*
~~~python
class a(float, Enum):
    a = "234"
    b = "23"

for i in a:
    print(i)
    print(i.name, type(i.name))
    print(i.value, type(i.value))
~~~
~~~
a.a
a <class 'str'>
234.0 <class 'float'>
a.b
b <class 'str'>
23.0 <class 'float'>
~~~
~~~python
class a(list, Enum):
    a = "234"
    b = "23"

for i in a:
    print(i)
    print(i.name, type(i.name))
    print(i.value, type(i.value))
~~~
~~~
a.a
a <class 'str'>
['2', '3', '4'] <class 'list'>
a.b
b <class 'str'>
['2', '3'] <class 'list'>
~~~
