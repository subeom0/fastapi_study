## http://127.0.0.1:8000/docs 는 코드상에 구현한 함수를 시각화하여 보여주는 사이트이다.

path는 rootlink(127.0.0.1:8000)뒤에 **/~** 를 의미한다.
~~~
127.0.0.1:8000/su/beom
~~~
위 링크에서 path는 /su/beom이 된다
이러한 path를 만드는 fastapi의 메소드는 **app.메소드명()** 형태로 사용된다
- post - 데이터 만들기
~~~python
#프론트에서 json형식으로 path에 정보를 보내면 받을 수 있다.
#테스트시에 postman이라는 프로그램을 사용할 수 있다. 이때 body에서 raw를 체크하고 형식을 json으로 전송해야한다.
@app.post(path)
def 함수명(파라미터):
  #내용~
~~~
- get - 데이터 읽기
~~~python
@app.get(path명)
def 함수명(파라미터):
  #---내용
  return 출력시킬 내용 #"su" "beom" 처럼 문자열을 띄어쓰기 형태로 리턴시 둘이 합쳐진다 -> "subeom"
                   # 1+2처럼 연산식을 리턴하면 연산이 된다. 하지만 타입을 지켜주지 않으면 에러가 난다. ex) 1+'2'
~~~
- put - 데이터 업데이트
- delete - 데이터 삭제
## 색다른 것들
- options
- head
- patch
- trace


## async
def로 정의된 함수는 페이지를 넘길때마다 로딩을 처리한다.
하지만 async def로 정의된 함수는 모든 페이지를 로딩시켜둔 후 보여준다.
그렇기에 async def는 def에 비해 속도가 빠를 때도 있다.
~~~python
@app.get("/")
async def rasd():
    return await asd()

async def asd():
    return 0
~~~
이처럼 async로 선언된 함수는 await를 이용해 호출해야 코드가 돌아간다.
await은 async def 함수에서만 사용가능하다.

~~~python
@app.get("/")
async def rasd():
    print(asd())
    return await asd()

async def asd():
    return 0
~~~
~~~
<coroutine object asd at 0x101eb8540>
~~~
이렇게 await를 쓰지 않으면 코루틴 객체가 반환되고 실제 함수는 실행되지 않는다. * https://sjquant.tistory.com/14 *

## path parameter
경로중 일부를 함수의 인자로 넘길 수 있다. print에서 포매팅과 유사하게 {넘겨줄 인자명}을 path에 포함시켜준다
~~~python
@app.get("/{a}")
def test(a):
  return a
~~~

**경로에 포함되지 않은 변수를 파라미터로 정의할 겅우 이는 쿼리파라미터로 인식된다.
쿼리문은 패스뒤에 파라미터명?파라미터명=값 형태로 온다.**
~~~
http://127.0.0.1:8000/asd?asd=adds
~~~

ex) http://127.0.0.1:8000/items
~~~
"items"
~~~

이렇게 넘기는 인자의 자료형을 정해줄 수 있다. 
경로상에서 123은 문자로 취급되는데, 우리는 인자로 정수르 받아야할 때가 있을 것이다.
이때 def 함수명(인자: 자료형) 형태로 선언하며 자료형이 변한다.
** 해당자료형으로 변환이 불가능하면 에러발생 **

ex)http://127.0.0.1:8000/12
~~~python
@app.get("/{a}")
def test(a: float): # 이렇게 자료형 지정가능 str, int, bool 등등 다 된다 ! 
  return a
~~~
~~~
12.0
~~~

error ex) http://127.0.0.1:8000/asdasdasd
~~~
{"detail":[{"loc":["path","a"],"msg":"value is not a valid float","type":"type_error.float"}]}
~~~
이렇게 error발생시 어디서 에러가 났는지를 알려준다 
*int -> float는 가능하지만 float ->int는 불가능하다*

### fastapi는 타입을 https://pydantic-docs.helpmanual.io 에 근거하고 았다. 타입 고를 때 참고

## enum상속 클래스를 자료형으로 지정
path를 통해 받아올 수 있는 값은 상속class내에 선언된 변수(들)의 값이다.
~~~python
class a(str, Enum):
    a = 234
    b = "23"
    
@app.get("/items/{item}")
def read_item(item: a, q: Optional[str] = None):
    print(item,a("234"))
    if item == a.a:
        item == a.a.name
        return item
    else:
        item = {"asd":123}
~~~
http://127.0.0.1:8000/items/234?q=1
~~~
print >> a.a a.a
"234"
~~~
path에서 파라미터로 값을 넘겨줄 때 기본값이 str로 정의되어서인지 enum상속 클래스에서 변수의 값이 문자열 이외의 다른 것으로 정의되면 오류가 발생했다.
~~~python
class a(Enum):
    a = 234
    b = "23"
    
def read_item(item: a, q: Optional[str] = None):
    print(item,a("234"))
    if item == a.a:
        item == a.a.name
        return item
    else:
        item = {"asd":123}
~~~
~~~
{"detail":[{"loc":["path","item"],"msg":"value is not a valid enumeration member; permitted: 234, '23'","type":"type_error.enum","ctx":{"enum_values":[234,"23"]}}]}
~~~
이를 해결하기 위해 enum상속클래스를 클래스명(str, Enum)형태로 선언하여주거나, 변수값들을 모두 문자로 정의해주면 된다.

path를 통해 받은 값은 클래스명(value)형태와 같은 값을 갖고 있다. <br>
이는 상속클래스 내에서 해당 값을 갖는 변수명과 동일하다.(클래스명.변수명 형태) 이는 아래 코드에서 print값이 a.a, a.a로 동일하게 나옴을 통해 알 수 있다. <br>
하지만 print로 출력할 때와 달리 return을 해주게 되면 변수명이 아니라 변수의 값, 즉 path에서 받아온 값(변수명.value)을 리턴한다.
~~~python
class a(str, Enum):
    a = 234
    b = "23"
    
@app.get("/items/{item}")
def read_item(item: a, q: Optional[str] = None):
    print(item,a("234"))
    if item == a.a:
        item == a.a.name
        return item # == return item.value
    else:
        item = {"asd":123}
~~~
~~~
print >> a.a a.a
"234"
~~~
path에서 파라미터로 값을 넘길 때 파일경로임을 명시할 수 있다.
~~~python
@app.get("/files/{file_path:path}")
async def read_file(file_path: str):
    return {"file_path": file_path}
~~~
files뒤에 /까지 파라미터에서 필요할 수 있다. 
이떄는 //를 입력해주면 된다.
