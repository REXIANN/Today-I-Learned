# 도커로 MySQL 실행하는 방법

- MySQL 의 경우에는 DockerHub에 있기 때문에 별도의 과정없이 즉시, image를 띄어볼 수 있습니다.
    
    `sudo docker run -d -p 9876:3306 -e MYSQL_ROOT_PASSWORD=password mysql:5.6`
    
    `9876:3306` : EC2의 9876 포트와 mysql 기본포트인 3306을 연결해줍니다.
    
    `MYSQL_ROOT_PASSWORD=password`: mysql의 비밀번호를 "password"로 설정해줍니다.
    
    `mysql:5.6` : msyql5.6을 (dockerhub 로 부터 다운로드 받아서)실행하겠습니다.
    
    ![https://media.vlpt.us/images/wimes/post/45f26b56-c83d-48f3-a63f-5c75e397da76/image-20200411144021533.png](https://media.vlpt.us/images/wimes/post/45f26b56-c83d-48f3-a63f-5c75e397da76/image-20200411144021533.png)
    

- mysql container에 접속 후 실행을 해보도록 합니다.
    
    `sudo docker exec -it ddd225c1d4e9 /bin/bash`
    
    ![https://media.vlpt.us/images/wimes/post/ea22f773-e071-4ed5-8465-fc21e3666f89/image-20200411144235445.png](https://media.vlpt.us/images/wimes/post/ea22f773-e071-4ed5-8465-fc21e3666f89/image-20200411144235445.png)
    
    `mysql -u root -p`
    
    ![https://media.vlpt.us/images/wimes/post/d975f58d-04da-4909-886f-8e4af95e20c6/image-20200411144356626.png](https://media.vlpt.us/images/wimes/post/d975f58d-04da-4909-886f-8e4af95e20c6/image-20200411144356626.png)
    
- mysql에서 DB를 생성하도록 합니다.
    
    ![https://media.vlpt.us/images/wimes/post/d3ff2b1a-b867-428c-9f85-93411fa9707b/image-20200411144451175.png](https://media.vlpt.us/images/wimes/post/d3ff2b1a-b867-428c-9f85-93411fa9707b/image-20200411144451175.png)
    
- 이제 container 자체가 아닌 container 안에 있는 mysql에 접속을 해보도록 하겠습니다.
    
    우선 해당 컨테이너의 세부정보를 알기위해서 docker insspect 명령어를 사용하도록 하겠습니다.
    
    ```
      exit
      docker ps -a
      docker inspect ddd225c1d4e9
    ```
    
    ![https://media.vlpt.us/images/wimes/post/0f02308e-6c16-4b8f-bdca-0289e92201c6/image-20200411144655260.png](https://media.vlpt.us/images/wimes/post/0f02308e-6c16-4b8f-bdca-0289e92201c6/image-20200411144655260.png)
    
    밑에 부분을 보면 ip주소를 확인 할 수 있는데 현재 우리의 경우엔는 172.17.0.2 로 되어 있습니다.
    
    ![https://media.vlpt.us/images/wimes/post/914b9f07-ebb8-4619-a137-62c9ef368e2b/image-20200411144823356-6587660.png](https://media.vlpt.us/images/wimes/post/914b9f07-ebb8-4619-a137-62c9ef368e2b/image-20200411144823356-6587660.png)
    
    우선 현재의 EC2에 mysql을 설치하도록 하겠습니다.
    
    `sudo apt install mysql-clinet-core-5.7`
    
    ![https://media.vlpt.us/images/wimes/post/331f59b5-55c9-4719-965c-c09dc7e8b840/image-20200411145140108.png](https://media.vlpt.us/images/wimes/post/331f59b5-55c9-4719-965c-c09dc7e8b840/image-20200411145140108.png)
    
    이제 mysql 접속을 해보도록 하겠습니다.
    
    `mysql -u root -p --host 172.17.0.2 --port 3306`
    
    ![https://media.vlpt.us/images/wimes/post/957e7537-4e6a-43cd-9d47-e3bf7444378e/image-20200411145206414.png](https://media.vlpt.us/images/wimes/post/957e7537-4e6a-43cd-9d47-e3bf7444378e/image-20200411145206414.png)
    
    아까 만들었던 TEST 데이터베이스도 존재하는 것을 볼 수 있습니다.
    
    ![https://media.vlpt.us/images/wimes/post/0cf8a6d2-1477-4f56-bd9a-5cd3c7eaba00/image-20200411145324599.png](https://media.vlpt.us/images/wimes/post/0cf8a6d2-1477-4f56-bd9a-5cd3c7eaba00/image-20200411145324599.png)
    
    또 다른 방법으로는
    
    `sudo docker ps -a`
    
    ![https://media.vlpt.us/images/wimes/post/f0697cc4-e7c4-4eae-ac4e-81753aea9acc/image-20200411145709960.png](https://media.vlpt.us/images/wimes/post/f0697cc4-e7c4-4eae-ac4e-81753aea9acc/image-20200411145709960.png)
    
    를 보면 9876과 연결되어 있는 것을 볼 수 있습니다.아래의 명령어를 통해 자기자신 ip(127.0.0.1)를 이용해 연결할 수도 있습니다.
    
    `mysql -u root -p --host 127.0.0.1 --port 9876`
    
    ![https://media.vlpt.us/images/wimes/post/1f43901f-2746-4c45-8b9a-2d2a0e9dbac4/image-20200411145801261.png](https://media.vlpt.us/images/wimes/post/1f43901f-2746-4c45-8b9a-2d2a0e9dbac4/image-20200411145801261.png)
    
    이렇게 docker를 이용해서 mysql을 만들었지만 docker container 특성상 **언제든지 없어질 수 있기 때문에 보통은 docker에 DB를 올리지는 않습니다.**
    

- 관리자 유저 생성 후 외부 접속
    
    TEST 라는 이름의 password 는 "password"로 user를 생성해줍니다.
    
    ```
    use mysql;
    CREATE USER 'test'@'%' IDENTIFIED BY 'password';
    ```
    
    그리고 외부에서 접속 할 수 있도록 권한을 부여하도록 합니다.
    
    ```
    GRANT ALL PRIVILEGES ON *.* TO 'test'@'%';
    ```
    
    ![https://media.vlpt.us/images/wimes/post/5cb9bee9-09ff-4ceb-8c59-5a7440578be5/image-20200411153603293.png](https://media.vlpt.us/images/wimes/post/5cb9bee9-09ff-4ceb-8c59-5a7440578be5/image-20200411153603293.png)
    
    그 다음 해당 컨테이너를 재시작 해줍니다.
    
    ```
      sudo docker ps -a
      sudo docker restart ddd225c1d4e9
    
    ```
    
    ![https://media.vlpt.us/images/wimes/post/b1b48223-2809-41e7-923e-b4d1666d6739/image-20200411153714095.png](https://media.vlpt.us/images/wimes/post/b1b48223-2809-41e7-923e-b4d1666d6739/image-20200411153714095.png)
    
    그리고 인바운드 규칙을 수정하여 외부에서 접속(9876포트)할 수 있도록 합니다.
    
    ![https://media.vlpt.us/images/wimes/post/d738a354-8161-4abe-9b33-af356e0087f6/image-20200411153843058.png](https://media.vlpt.us/images/wimes/post/d738a354-8161-4abe-9b33-af356e0087f6/image-20200411153843058.png)
    
    그리고 [sequelPro](https://sequelpro.com/download#auto-start)을 이용해 외부에서 접속을 해보도록 합니다.(윈도우의 경우 heidiQSL을 이용합니다.)
    
    ![https://media.vlpt.us/images/wimes/post/227b877a-ead8-40bb-a1bd-3d361d3153f6/image-20200411154346548.png](https://media.vlpt.us/images/wimes/post/227b877a-ead8-40bb-a1bd-3d361d3153f6/image-20200411154346548.png)
    
    현재 mysql의 버전을 출력해주는 Query를 날려보겠습니다.
    
    ![https://media.vlpt.us/images/wimes/post/4c8f2872-28d6-4005-9564-009a3c9f7b8b/image-20200411154507876.png](https://media.vlpt.us/images/wimes/post/4c8f2872-28d6-4005-9564-009a3c9f7b8b/image-20200411154507876.png)
    
    정상적으로 5.6이 출력되는 것을 볼 수 있습니다.