# 도커로 Django 배포(with MySQL)

먼저 해야할 설정 → 이 설정을 하지 않으면 이미지로 만들어져도 컨테이너 실행이 되지 않는다. 컨테이너를 실행하자마자 바로 꺼지니 주의하자. 나도 계속 꺼지는 컨테이너를 확인하기 위해 실행명령에서 -d 옵션을 끄고 나서야 알았다...(-d 옵션을 주면 백그라운드에서 컨테이너가 실행된다)

```python
# settings.py
DEBUG = False
ALLOWED_HOSTS = ['*']
```

마찬가지로 settings.py에서 데이터베이스를 mysql로 변경해준다.

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'DKM',
        'USER': 'root',
        'PASSWORD': 'ssafy',
        'HOST': 'k3a504.p.ssafy.io', # 배포시 사용될 주소를 적으면 된다.
        'PORT': '9876', # mysql 컨테이너의 외부포트를 적어주면 된다.
    }
}
```

먼저 mysql 컨테이너를 실행해야 한다. mysql은 기본적으로 제공되는 이미지가 있으므로 우리는 배포만 하면 된다.

```bash
# mysql image 실행
sudo docker run -d -p 9876:3306 -e MYSQL_ROOT_PASSWORD=ssafy mysql:5.6
# 한글이 깨진다면 다음 명령어
docker run -d -p 3306:3306 -e MYSQL_ROOT_PASSWORD=ssafy mysql:5.6 --character-set-server=utf8mb4 --collation-server=utf8mb4_unicode_ci
# mysql이 정상적으로 실행되는지 확인
docker ps 
# mysql의 아이피 확인 -> 별의미 없음. 안해도 된다.
docker inspect mysql아이디
# mysql의 bash에 접속
sudo docker exec -it mysql아이디 /bin/bash
# bash에서 mysql을 켜자
mysql -u root -p
```

MySQL

```sql
# 테이블들을 확인
SHOW databases;
# 사용할 테이블을 추가. 이름만 추가하면 된다.
CREATE DATABASE DB이름;

# 아래 과정은 아직 해보지 않음.
# 데이터베이스와 사용자를 생성하고 (컨테이너 내에서) MySQL에서 권한을 부여한다.
# 변경된 권한 적용
# 중요 : 컨테이너 외부에서 MySQL에 로그인도 가능해야 하므로 
# ubuntu@localhost에서 localhost 대신 %를 사용한다.

CREATE USER 'ubuntu'@'%' IDENTIFIED BY 'ssafy';
Query OK, 0 rows affected (0.00 sec)

GRANT ALL PRIVILEGES ON *.* TO 'jmlim'@'%';
Query OK, 0 rows affected (0.00 sec)

FLUSH privileges;
Query OK, 0 rows affected (0.00 sec)

EXIT
```

그리고 `EXIT` 을 한번 더 입력하여 mysql 컨테이너의 bash에서 나온다. 

이제 장고를 빌드할 차례이다.

사용한 Dockerfile

```docker
FROM python:3.6.8
ENV PYTHONUNBUFFERED=1

# app 폴더를 생성하여 거기에 파일들을 저장
RUN mkdir /app 
WORKDIR /app
RUN chmod 755 /app

COPY requirements.txt /app/
RUN pip3 install -r requirements.txt
# 누락된 라이브러리들 추가... requirements.txt 최신화 필요
RUN pip3 install django-rest-swagger
RUN pip3 install mysqlclient
RUN pip3 install pymysql
RUN pip3 install konlpy
COPY . /app/

# 포트 8000번 개방
EXPOSE 8000

# run the file
RUN python3 manage.py makemigrations
RUN python3 manage.py inspectdb
RUN python3 manage.py migrate
# 추후 로드데이터 필요

# 마지막 커맨드로 최종 실행
CMD ["python3", "manage.py", "runserver", "0:8000"]
```

이후 명령어를 통해 빌드

```bash
docker build . -t django
```

빌드가 성공했다면 이제 실행

```bash
docker run -p 8000:8000 django6
```

이렇게 되면 사용자가 [http://k3a504.p.ssafy.io:8000](http://k3a504.p.ssafy.io:8000) 으로 접속을 하게 되면 내부포트 8000번을 통해 장고로 연결 → 데이터 입력시 장고에서 [http://k3a504.p.ssafy.io:9876으로](http://k3a504.p.ssafy.io:9876으로) 데이터 전송 → 연결된 내부포트 3306을 통해 SQL이 데이터 수신이 가능해진다.

이틀이면 하겠지 라고 생각했으나 도커, nginx, mysql 그리고 AWS를 통한 배포를 제대로 공부하면서 하다보니  5일이나 걸렸다... ㅠㅠ 너무나 많이 포기하고 싶었다..

에러를 너무 많이 만나서 만난 에러와 적용해본 해결법도 적어놨는데 나중에 업로드 하자