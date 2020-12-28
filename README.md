# 정승현의 지식 저장 공유소 깃허브에 오신걸 환영합니다. 👋



## History

* Suwon Uni. (2015/02)~(2021/02)
* Wins (co). (2020/08)~(2020/12) - Web Dev

## 사용 언어 및 프레임워크❗️❗️

**실무에서 사용한 언어 및 프레임워크**
**JavaScript(보통) TypeScript(보통) NodeJS(보통) NestJS(보통) Angular(낮음) TypeORM(보통) Jest(중/상)** <br>

**지속적으로 공부중인 언어 및 프레임 워크**
**Java(중) / Spring(하) / JPA(중)**

## 실무에서 했던 일들 및 느낀점

1. Angular Context-menu 개발
<적는중...>
2. TEST CODE 작성 (JEST FRAME WORK 를 통한)
<적는중...>
3. Python Script 를 통한 업무 자동화

- 회사에서 테스트 업무를 맞게되면서 Git Repository 를 두가지로 구분하게 되었다. 개발 서버와, Test Report 를 따로 저장하는 일이였다.
- Dev Repo 에서 CI/CD pipeline 을 거친뒤, Jest 를 통한 UT TEST 후 해당 테스트 정보를 Test repository 에 Push 하도록 설계했다. 도식 정보는 아래와 같다.
4. Shell Script 를 통한 업무 자동화
<적는중...>

## 5. Jest FrameWork 를 통한 Test 및 Git-lab CI/CD Pipeline 을 통한 테스트 자동화 및 코드 검증

### 처음 입사시
내가 다니던 회사에서는 테스트 환경이 적절히 구성되어 있지 않았다. 회사에 처음 입사했을때, JavaScript 와 TypeScript 를 본격적으로 접했는데, 생각보다 자바의 신 문법들과 비슷해서<br>
배울만 했던것 같다. 특히 TypeScript 는 정말 친숙하기도 하고 맘에 들었다. 그래서 공부를 하다가 회사에서 테스트 환경을 구성하라는 업무를 받았고, Jest로 일단 백엔드 구간에 대한 테스트 코드들을 작성해 나갔다.<br>
남의 코드를 보면서 느꼈던 것들은, 일단 테스트 기반으로 작성된 코드들이 아니라, 함수가 잘게잘게 쪼개져 있지 않았다. 그래서 처음에는 테스트 하는데 되게 애를 먹었다.<br>
그래서 부분적으로 선임분들에게 해당 코드를 다른 방식으로 바꿔보는것이 어떨까란 이야기도 해봤고, 그래서 꽤 코드들도 수정되었다.<br>
회사에서는 테스트 환경을 본격적으로 처음 도입하는것인데, 생각보다 성과가 있는것에 놀라했다. 그래서 나에게 테스트 환경과 업무를 전담하여 맡게 했고, Jest 에 대한 공부를 하고, 효율적으로 셋팅할 수 있게 하려했다.<br>
### 고려사항
첫번째로 문제가 됬던 부분은 백엔드 구간의 코드가 노드의 실행 파일의 위치를 ROOT_PATH 로 잡는 것이 제일 큰 문제였다.<br>
회사에서는 테스트 환경을 기반으로 설계한것이 아니다보니, 테스트 수행시는 /root/node_module 의 위치를 따라가므로, 해당 루트를 자동으로 인식할 수 있게 해야 했다. <br>
그래서 Jest Setup.ts 파일을 생성하여, Jest 파일이 생성되기전, setRoot() Promise 함수를 통해, PATH 를 원하는 lib이 있는 경로로 인식하게 했다. <br>
근데 한가지 문제점이라면, docker 환경이 Build 될 시점 부터 Git Repository 를 Clone 하지 않아, /path/project 의 위치가 제각각 이였다. 그래서 테스트 환경이 정상적으로 작동하기 위해서는<br>
해당 ROOT_PATH 를 사용자가 설정할 수 있어야 했고, 이를 JEST 실행 전, setup 파일에 간단한 매개변수로 /path 부분만 넘기면 자동으로 설정하게 하였다.<br>
그리고 테스트 프레임워크를 적용하게 되면서 함수단위로 명확히 구분되는 코드가 중요하다는 것이 테스트 하기 쉽고, 테스트 하기 쉽다는 말은, 함수 단위의 작업 프로세스가 한가지로 명확해 질 수 있다는 점에서 앞으로 나도 코드를 적을때 함수단위로 명확하게 적어야겠다는 생각이 들었다. 

### Git-lab CI/CD 에서의 UT TEST
![파이프라인설계](./PipelineArch.png) 
- 위와 같이 파이프라인을 설계하기로 마음먹었다. 기존에는 Lint 와 Build 과정만 진행했으나, UT TEST 를 진행하고, 해당 Report 파일을 Git-lab Cache-Server 에 올린 뒤, Report 하는 단계에서 해당 파일을 사용할 수 있게 하였다.

### CI/CD 문제점

- 이건 도커 환경의 문제였는데, 아무래도 테스트를 하지않다보니, 테스트 환경의 DB 를 개발서버와 똑같은 DB를 보고 있었다. 그래서 TEST 를 돌리는데 한명당 2\*N의 DB Connection 의 요구사항이 필요하게 되었고, 이는 자연스레 데이터베이스 서버의 부담감으로 작용했다. 그래서 회사에서 사용하고 있던 TypeORM 에 대해 공부했고, TypeORM 이 Default 로 Connection을 10개 가져간다는 사실을 알게되었다. 그래서 테스트 환경에서 Connection 에 대해 TEST 를 진행했고, 5개정도로 설정했을때 적당하다는 결과를 도출하게되었다.

## 계속해서 해나갈 것들🧘🏻‍♂️

### 1일 1Commit 운동

* 매일 학습하며 점점 기술적으로 습득속도 그리고, 공부해야 되는게 더 많다고 느끼게 되어 계속해서 하고 있는 운동

### CS 기초이론 공부

* CS 기초 이론을 공부하다보면 기본적인 판단 능력을 배양해준다고 생각되어, 계속해서 구현하고 보강해나갈 예정

### Java Spring && JPA 공부

* 기초 원리 동작이나, 여러 실습등을 통한 공부 및 지속적인 프로젝트로 학습 예정

### DB 공부

* Database 를 관리하고, WindowFunction 및 어떻게 운용해야 되는지 천천히 공부하고 정리하며 학습해나갈 예정

## 하고 싶은 것들

* OpenSource Project 참여!

# Velog 주소

<https://velog.io/@tmdgusya>

<!--
**tmdgusya/tmdgusya** is a ✨ _special_ ✨ repository because its `README.md` (this file) appears on your GitHub profile.
