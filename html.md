~~~python
from fastapi import FastAPI, Request
from fastapi.responses import HTMLResponse
from fastapi.templating import Jinja2Templates
~~~

~~~python
template = Jinja2Templates(directory='homepage')
app.mount("/homepage", StaticFiles(directory="homepage"), name="homepage")
~~~
경로가 포함된 페이지로 접근할 때 마운트시킴 (마운트는 정적파일을 사용할 수 있게 해주는 것) StaticFiles의 directory이하의 파일을 마운트 name은 html에서 url_for에서 이용

~~~python
@app.get("/homepage", response_class=HTMLResponse)
async def read_item(request: Request):
    return template.TemplateResponse("home.html", {"request": request})
~~~
html을 띄우는 코드

~~~html
<head>
<link href="{{ url_for('homepage', path='/style.css') }}" rel="stylesheet">
</head>

<img src="{{url_for('homepage',path = 'image/icon0.svg')}}">
~~~
url_for을 이용해 이미지, css로 정적파일 불러옴 http://127.0.0.1:8000/homepage/style.css 같이 서버링크로 불러오게 됨
path앞의 'homepage'는 app.mount에서 name = ''에 들어가는 변수명을 사용해야한다. 
변수명에 해당하는 경로의 파일을 불러올 수 있게 되는듯하다.
