~~~
.
└── sql_app
    ├── __init__.py
    ├── crud.py
    ├── database.py
    ├── main.py
    ├── models.py
    └── schemas.py
~~~

## __init__.py
__init__.py는 패키지임을 나타내기 위해 넣어둠.( 빈문서임 )

## database.py 
database에 연결하기 위한 파일

~~~ 
$ pip install pymysql
$ pip install sqlalchemy
~~~

~~~python
# 예제코드
from sqlalchemy import create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker

user, pw, host, port, db = 0, 0, 0, 0, 0
DB_URL = "mysql+pymysql://"+user+":"+pw+"@"+host+":"+port+"/"+db+"?charset=utf8"

engine = create_engine(
    DB_URL, connect_args={"check_same_thread": False}
)
SessionLocal = sessionmaker(autocommit=False, autoflush=False, bind=engine)

Base = declarative_base()
~~~

~~~
General formula
dialect+driver://username:password@host:port/database

PostgreSQL
# import psycopg2
engine = create_engine('postgresql+psycopg2://username:password@host:5432/database')

MySQL
# import pymysql
engine = create_engine('mysql+pymysql://username:password@host:3306/database')

Oracle
# import cx_Oracle
engine = create_engine('oracle://username:password@host:1521/database')
engine = create_engine('oracle+cx_oracle://username:password@host:1521/database')
~~~
sqlalchemy db별 사용법

~~~python
user, pw, host, port, db = 0, 0, 0, 0, 0
DB_URL = "mysql+pymysql://"+user+":"+pw+"@"+host+":"+port+"/"+db+"?charset=utf8"
~~~
DB 경로는 "mysql+pymysql://[유저이름]:[비밀번호]@[호스트주소]:[포트번호]/[스키마이름]?charset=utf8"로 구성 mysql 기준

~~~python
engine = create_engine(
    DB_URL, connect_args={"check_same_thread": False}
)
~~~
engine은 DB엔진을 만든다.
connect_args={"check_same_thread": False}는 SQLite사용시에만 필요하다. 다른 DB이용시 필요없다.

~~~python
SessionLocal = sessionmaker(autocommit=False, autoflush=False, bind=engine)
~~~
DB세션

~~~python
Base = declarative_base()
~~~
DB모델 만들기 위한 클래스

## models.py
database.py를 통해 접근한 DB(스키마)에 테이블과 컬럼을 추가하는 클래스를 만든다.
DB구조 : 스키마(= DB) - table - column ( in mysqldb ) 

~~~python
from sqlalchemy import Boolean, Column, ForeignKey, Integer, String # 컬럼 자료형
from sqlalchemy.orm import relationship
from .database import Base
~~~

~~~python
# fastapi 문서에서는 db와 상호작용하는 인스턴스와 클래스를 model이라 칭함
class 클래스명(Base): #database.py의 Base 상속
    __tablename__ = "테이블명" #만들 테이블명
    
    컬럼명 = Column(속성) 

~~~
