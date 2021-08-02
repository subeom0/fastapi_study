path는 rootlink(127.0.0.1:8000)뒤에 **/~** 를 의미한다
~~~
127.0.0.1:8000/su/beom
~~~
위 링크에서 path는 /su/beom이 된다
이러한 path를 만드는 fastapi의 메소드는 **app.메소드명()** 형태로 사용된다
- post - 데이터 만들기
- get - 데이터 읽기
~~~python
@app.get(path명)
def 함수명(인자):
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
이렇게 await를 쓰지 않으면 코루틴 객체가 반환되고 실제 함수는 실행되지 않는다. *https://sjquant.tistory.com/14
