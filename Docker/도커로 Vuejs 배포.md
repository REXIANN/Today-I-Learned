# 도커로 Vuejs 배포

## **Docker 에 nginx와 node 이미지 추가**

로컬에서 도커의 사용법에 대해 충분히 익혔다. 이제 우리가 가진 AWS Key를 사용해서 EC2의 우분투에 접속하고, 우분투에 도커를 설치한 뒤 nginx와 node의 이미지 파일을 다운로드 한 다음 vuejs를 배포하기 위한 dockerfile과 nginx.conf 를 작성하고 vuejs를 빌드한 폴더를 도커 이미지로 만들어서 컨테이너화 한 다음 웹에서 접속이 되는것을 보면 된다(적으면서도 정말 숨차다. 저 세줄을 완성하기 위해 몇날 며칠을 헤맸는지 ~~부들부들~~  ~~그러나 그 뒤 백엔드와 DB때문에 프론트엔드에 쏟은 시간의 세배를...~~) 

일단 이미지를 추가하자

```bash
docker pull nginx
docker pull node:12.18 # :뒤에 사용할 버전을 명시하자
```

내려 받은 이미지를 확인하자

```bash
docker images
REPOSITORY                    TAG                 IMAGE ID            CREATED             SIZE
front-test7                   latest              08697caa0b86        2 days ago          137MB
nginx                         latest              c39a868aad02        8 days ago          133MB
mysql                         5.6                 c580203d8753        3 weeks ago         302MB
node                          12.18               28faf336034d        8 weeks ago         918MB
python                        3.6.8               48c06762acf0        17 months ago       924MB
```

## **Dockerfile 작성하기 - 오류해결(/root)**

### 작성한 **Dockerfile**

```docker
FROM node:12.18 as build-stage
WORKDIR .
COPY package*.json ./
RUN npm install

COPY ./ .
RUN npm run build

FROM nginx as production-stage
RUN mkdir /usr/local/app
COPY --from=build-stage ./dist /usr/local/app
COPY nginx.conf /etc/nginx/nginx.conf
```

사실 이 부분에서 에러가 나서 구글링을 하다 보니 디렉토리를 root 디렉터리(`/`) 바로 밑에 만들지 말라는 말이 있어서 위치를 `/usr/local/app` 으로 옮겼다. nginx 를 통해 배포를 해야 했기에 빌드가 끝나면 nginx로 이어서 작업을 하도록 설정했다.

### 사용한 **nginx.conf**

```nix
user nginx;
worker_processes 1;
error_log /var/log/nginx/error.log warn;
pid       /var/run/nginx.pid;

events {
  worker_connections 1024;
}

http {
  include /etc/nginx/mime.types;
  default_type application/octet-stream;
  log_format main '$remote_addr - $remote_user [$time_local] "$request" '
                  '$status $body_bytes_sent "$http_referer" '
                                '"$http_user_agent" "$http_x_forwarded_for"';
  access_log /var/log/nginx/access.log main;
  sendfile on;
  keepalive_timeout 65;
  
  server {
    listen 3000;
    server_name localhost;
    location / {
      root /usr/local/app;
      index index.html index.htm;
      try_files $uri $uri/ /index.html;
    }
    error_page 500 502 503 504 /50x.html;
    location = /50x.html {
      root /usr/share/nginx/html;
    }
  }
}
```

### 실행

```jsx
docker run -d -p 80:3000 front-test7
```

## **nginx 설정하기 - 외부포트 내부포트에 대한 이해**

### **실행중인 컨테이너**

```bash
ubuntu@ip-172-26-13-183:~/s03p31a504/frontend$ docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                          NAMES
222ea4d3983e        front-test7         "/docker-entrypoint.…"   12 minutes ago      Up 12 minutes       80/tcp, 0.0.0.0:80->3000/tcp   quizzical_bardeen
```

## **Vue 공식페이지에 설명되어 있는 배포**

## **General Guidelines**

If you are using Vue CLI along with a backend framework that handles static assets as part of its deployment, all you need to do is make sure Vue CLI generates the built files in the correct location, and then follow the deployment instruction of your backend framework.

If you are developing your frontend app separately from your backend - i.e. your backend exposes an API for your frontend to talk to, then your frontend is essentially a purely static app. You can deploy the built content in the `dist` directory to any static file server, but make sure to set the correct [publicPath](https://cli.vuejs.org/config/#publicpath).

### **[#](https://cli.vuejs.org/guide/deployment.html#previewing-locally)Previewing Locally**

The `dist` directory is meant to be served by an HTTP server (unless you've configured `publicPath` to be a relative value), so it will not work if you open `dist/index.html` directly over `file://` protocol. The easiest way to preview your production build locally is using a Node.js static file server, for example [serve](https://github.com/zeit/serve):

```
npm install -g serve
# -s flag means serve it in Single-Page Application mode
# which deals with the routing problem below
serve -s dist
```

### **[#](https://cli.vuejs.org/guide/deployment.html#routing-with-history-pushstate)Routing with `history.pushState`**

If you are using Vue Router in `history` mode, a simple static file server will fail. For example, if you used Vue Router with a route for `/todos/42`, the dev server has been configured to respond to `localhost:3000/todos/42` properly, but a simple static server serving a production build will respond with a 404 instead.

To fix that, you will need to configure your production server to fallback to `index.html` for any requests that do not match a static file. The Vue Router docs provides [configuration instructions for common server setups](https://router.vuejs.org/guide/essentials/history-mode.html).

### **CORS**

If your static frontend is deployed to a different domain from your backend API, you will need to properly configure [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).

### **PWA**

If you are using the PWA plugin, your app must be served over HTTPS so that [Service Worker](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API) can be properly registered.