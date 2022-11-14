# Gitlab runner

유튜브 [GITLAB CICD로 간단한 자동배포](https://www.youtube.com/watch?v=WyH_ihsIaik)
## installation
gitlab 페이지에 있는 Manual installation 사용해서 설치(안되면 brew로 설치해도 되지만 수동설치가 추천임)

러너를 설치했다면 이제 깃랩 CI 서버와 연동해야 한다. 연결되지 않으면 러너는 쓸모가 없다.
```shell
gitlab-runner register
```
이후 URL 과 토큰을 터미널에 입력해준다. 

깃랩CI에서 러너를 돌리는 방법은 다양하다. 도커 컨테이너를 사용하거나 shell 을 사용하는게 제일 간단하다. 

도커를 선택하고 이미지 버전을 입력(동영상에 는 `ruby:2.6`을 사용)

이후 혹시 모르니 깃랩러너를 설치
```shell
gitlab-runner install
gitlab-runner start
```

이제 깃랩의 설정 페이지를 새로고침하면 해당 프로젝트의 러너가 활성화되어 있는 것을 볼 수 있다.
shared runners 를 사용하면 러너를 공유할 수 있다.

나는 브루로 설치해서 `gitlab-runner register`로 설정 했다. 설정도 저장이 된다고 한다.
Configuration (with the authentication token) was saved in "/Users/jinhyuckkim/.gitlab-runner/config.toml"

설정 시 러너를 돌릴 주체가 무엇인지 물어보는데 나는 shell, 강사님은 docker 를 골랐다.
강사님은 기본 이미지를 node:latest 로 적었다.

## .gitlab-ci.yml
gitlab-ci.yml 파일은 원래는 깃랩 저장소에 푸시가 될 때마다 실행된다. 그러면 큰일난다.

그래서 각 단계에서 `rules` 을 필수적으로 만들어주어야 한다. 
```yaml
image: node:latest # 스크립트 구동 환경

stages: # 파이프라인 단계 구성
  - install
  - build
  - deploy

install:
  stage: install
  script:
    - apt-get update -qq && apt-get install # 얀 실행시키기 위함
    - yarn config set cache-folder .yarn # 얀 설정
    - yarn install # 패키지.제이슨 에 있는 거 설치
  artifacts: # 해당 단계를 실행한 후 결과물에 대한 설정
    name: "artifacts"
    untracked: true # 추적 안함
    expire_in: 30 mins # 결과물 만료시
    paths:
      - .yarn/
      - node_modules/
  rules: # 스크립트가 실행되기 위한 조건
    - if: $CI_COMMIT_REF_NAME == "main" # 마스터브랜치에 찍힌 커밋이면 실행
      when: always # 무조건 실행
    - if: $CI_MERGE_REQUEST_TARGET_BRANCH_NAME == "main" # 마스터브랜치를 대상으로한 머지리퀘스트면 실행
      when: always
    - when: never # 상기 조건 이외의 경우 실행하지 않음

build:
  stage: build
  dependencies:
    - install
  script:
    - yarn build
  artifacts:
    paths:
      - build
    expire_in: 30 mins
  rules:
    - if: $CI_COMMIT_REF_NAME == "main"
      when: on_success # 앞의 단계를 성공했을 경우에만 실행
    - if: $CI_MERGE_REQUEST_TARGET_BRANCH_NAME == "main"
      when: on_success
    - when: never

deploy:
  image: python:latest # aws cli에 있는 스크립트를 통해 배포를 해야함. 파이썬으로 설치하는게 간단해서 파이썬 이미지 사용
  stage: deploy
  dependencies:
    - build
  variables:
    S3_BUCKET_NAME: your-bucket-name.com
  before_script:
    - pip install awscli
  script:
    - aws s3 sync build s3://$S3_BUCKET_NAME/ --acl public-read
  rules:
    - if: $CI_COMMIT_REF_NAME == "main"
      when: on_success
    - if: $CI_MERGE_REQUEST_TARGET_BRANCH_NAME == "main"
      when: on_success
    - when: never

```

위의 install 단계를 거치지 않았다면 파이프라인이 실행이 되지 않고 멈춰있는다. 왜냐하면 깃랩 러너가 없기 때문이다.


만약 깃랩을 쓴다면 자동으로 shared runner 가 스크립트를 실행해 줄 것이다.

SWTT 강사분도 깃랩러너를 사용했다.
