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
