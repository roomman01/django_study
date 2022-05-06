



# Django Study

## 장고 프로젝트

### 1 장고 프로젝트 생성
	django-admin startproject 이름입력 (.) .입력시 현재경로에 생성
 
#### 1.1 프로젝트 설정파일 수정 (setting.py)
##### 1.1.1 
  프로젝트 관련 설정값들은 settings.py 파일에서 관리됨
  (루트 디렉토리, 기타 디렉토리 위치, 로그형식, 앱 명칭 등)
  
##### 1.1.2 ALLOWED_HOSTS
  - DEBUG=TRUE -> 개발모드 /FALSE -> 운영모드로 인식
  - 운영모드일 경우 ALLOWD_HOST에 반드시 서버 IP 또는 도메인 지정해야함
  - 개발모드일 경우 기본값으로 ['localhost', '127.0.0.1']로 지정됨

##### 1.1.3 앱 등록
  - 프로젝트 내의 앱은 모두 설정파일에 등록 필요함
  - INSTALLED_APPLS 에 모듈명만 등록해도 되지만, 앱 설정 클래스로 등록하는 것이 더 정확함
  - startapp 앱이름 명령으로 자동생성된 apps.py 파일의 앱이름Config라고 정의되어 있음
	  `'appname.apps.appnameConfig'`
  
 ##### 1.1.4 데이터베이스
 - 장고는 SQLite3를 기본 데이터베이스로 사용
 - MySQL, PostgreSQL 등 다른 데이터베이스로 변경 가능
 - 보통 개발단계에서 SQLite3로 테스트 후 운영 시 DB변경

##### 1.1.5 타임존 지정
- 프로젝트 생성 시 세계표준시(UTC)로 설정되어있음
- 한국 시간으로 변경

       #TIME_ZONE = 'UTC'
      TIME_ZONE = 'Asia/Seoul' 

- USE_TZ 설정을 TRUE로 하면 장고가 시간대를 조절함
  한국은 일광절약시간제를 사용하지 않으므로 USE_TZ=FALSE로 설정하는 것이 더 편리함 -> DB에 저장되는 시간대도 UTC가 아닌 한국시간으로 저장됨

#### 1.2 기본테이블 생성
	python manage.py migrate 

- 장고는 기본적으로 모든 프로젝트가 DB가 필요하다고 가정함
- 아직 DB나 테이블을 생성하지 않았더라도 User 등이 필요하다고 가정하고 기본적인 테이블을 만들어주기 위해 초기에 migrate 명령을 실행함
- 해당 명령 실행 시 SQLite3 DB파일인 db.sqlite3 파일이 생성됨


#### 1.3 슈퍼유저 생성
	python manage.py createsuperuser 
    
- 입력 시 화면표시따라 Username / Email address / Password / Password(again) 입력
- 입력 시 127.0.0.1/admin 페이지에서 생성한 유저로 로그인 가능
- 로그인 시 Groups, Users 테이블이 이미 존재함
- 이는 settings.py에 django.contrib.auth 앱이 등록되어 있어서 장고에서 auth앱에 Users, Groups 테이블을 기본적으로 제공




### 2. 장고 앱

	python manage.py starrtapp 앱이름

#### 2.1 Model
모델은 DB에 테이블을 생성하고 관리하는 작업임

- models.py : 테이블을 정의
- admin.py : 정의한 테이블을 Admin 화면에 반영
- python manage.py makemigrations : DB 변경사항 추출
- python manage.py migrate : DB변경사항 반영

##### 2.1.1 테이블 정의
	from django.db import models
	class Classname(models.Model):
		field1 = models.Charfield(max_length=100)
		필드명 = models.필드유형(옵션)
		
		def __str__(self):
			return self.question_text

- 장고에서 테이블은 클래스로 정의함(django.db.models.Model을 상속)
-  컬럼(필드)는 클래스 안의 변수와 대응됨
- 기본키(Primary Key)는 클래스에서 미지정 시 장고가 자동으로 Not null, Autoincrement 설정하고 필드명은 id로 자동 생성
-  외래키(Foreing Key)는 다른 테이블의 기본키에 연결, 연결할 테이블명(클래스명) 입력하여 지정
`models.ForeignKey(연결할 클래스명)`
외래키 지정 시 외래키로 지정된 필드명은 '연결한 테이블명_id'이 됨
- __str__() 은 객체를 문자열로 표현 시 사용하는 함수임
  __str__메소드를  사용하지 않으면 Admin화면이나 장고 쉘 화면에서 테이블명을 확인하기 어려움(테이블명이 아닌 숫자,영문으로 혼합된 알수없는 값으로 표현됨)

##### 2.1.2 Admin 사이트에 테이블 반영
- 초기에는 Admin 사이트에 기본제공 테이블인 Users, Groups 만 표시됨
- models.py에서 생성한 테이블을 Admin 화면에 표시되도록 등록
- 즉 테이블 신규생성 시 models.py와 admin.py 를 함께 수정

		from django.contrib import admin
		from appname.models import Tablename1, Tablename2
		
		admin.site.register(Tablename1)
		admin.site.register(Tablename2) 

> Written with [StackEdit](https://stackedit.io/).


