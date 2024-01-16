![header](https://capsule-render.vercel.app/api?type=wave&color=auto&height=300&section=header&text=minho%20lee&fontSize=90)

# 진화하는 개발자 이민호입니다🐜      
<br/> <br/> <br/> 

# 약력

 - 2017/03 ~ 2022/02 : 세종대학교 바이오융합공학과 재학
 - 2021/03 ~ 2022/02 : 세종대학교 소프트웨어공학과 재학(복수전공)
 - 2021/11 ~ 2022/09 : 42서울 카뎃
 - 2022/03 ~ 2022/06 : (주)클래스 액트 인턴
 - 2022/08 ~ 2022/10 : 4차산업기술인재 - Web 1기
 - 2022/10 ~ 현재 : 티맥스 클라우드 프론트엔드 연구원






       
# 진행한 프론트엔드 프로젝트  


# HyperCloud (오픈 소스 프로젝트)

<br/>

HyperCloud는 사용자 주도의 선택적 환경을 제공하는 클라우드 엔진으로, 사용자 환경에 맞는 인프라 및 플랫폼을 선택하여 빠르고 쉽게 구성해주는 통합 클라우드입니다.  또한, 검증을 거친 다양한 오픈소스와 Tmax의 소프트웨어의 결합 및 자동화를 통해 사용 편의를 고려하였으며, 이를 통해 클라우드 기반 신기술 활용에 최적화를 이룬 클라우드 플랫폼입니다.

<br/>
<img width="1323" alt="스크린샷 2022-12-13 오후 5 19 06" src="https://user-images.githubusercontent.com/82989054/207263087-6e74057a-d5db-4842-a1af-a57140f5bd82.png">
<br/>
1. ui를 사용하여 편하게 이용할 수 있는 프로젝트이지만 cli에 익숙한 사용자들을 위해 터미널 기능 구현<br/>
2. 많은 기능으로 인해 사용자가 복잡한 인식을 주는 것을 피하기 위해 ClusterMenuPolicy를 설정하여 필요한 메뉴만 볼수있도록 함<br/>
3. 오픈소스 프로젝트임으로 여러스타일의 코드와 컴포넌트가 섞여있어서 컴포넌트 공통화 및 최적화<br/>
4. http 에서 웹소켓을 사용한 통신으로 변경 적용<br/>
5. ci/cd를(jenkins) 통해 배포 자동화<br/>
6. 글로벌화를 위해 국제화 라이브러리 i18n적용<br/>
7. 새로운 페이지를 구성할 때 앱의 성능을 향상시키기 위해 Lazy loading 라우팅 사용<br/>
<br/>



# TCP-IaaS

<br/>

버츄얼 머신 등 iaas 서비스를 ux/ui 친화적으로 접근가능한 프로젝트
<br/>
(깃 랩)
<br/>
![스크린샷 2023-11-16 오후 4 58 23](https://github.com/inth9198/inth9198/assets/82989054/a879a152-f5a6-4b93-adfe-cdbfda466e0e)
![스크린샷 2023-12-11 오전 10 52 50](https://github.com/inth9198/inth9198/assets/82989054/803d5ba2-19ce-4363-afef-c0f867f6d671)



<br/>
1. Nginx와 도커, 쿠버네티스를 활용하여 배포(이미지를 생성한 뒤 디플로이,서비스,인그래스 yaml작성 후 쿠버네티스를 이용하여 직접 배포)<br/>
2. 클러스터, 호스트, 가상머신, 템플릿, 스토리지 도메인, 디스크, 이미지, 파일시스템 등 여러서비스 및 페이지 구현<br/>
3. 프로젝트 생성부터 배포 ci/cd까지 프론트부분 전반을 기여 및 리드 개발<br/>
4. usecontext, structure schema를 이용해서 폼 공통 컴포넌트 구현<br/>
5. eslint를 활용하여 DX최적화<br/>
6. webpack을 활용하여 번들링 작업 수행(빌드 파일 용량을 5분의 1로 감량)<br/>
7. 여러 리소스에 시각화 컴포넌트를 도입하여 그래프등 다양한 시각화 자료를 제공함<br/>
<br/>
<br/>

# platform-master

<br/>

클라우드 플랫폼 서비스를 ux/ui 친화적으로 접근가능한 프로젝트
<br/>
(깃 랩)
<br/>
![스크린샷 2023-12-06 오전 10 31 32](https://github.com/inth9198/inth9198/assets/82989054/3b86aec6-44cd-4826-ab41-2b47b961aa27)
![Dec-01-2023 15-06-13](https://github.com/inth9198/inth9198/assets/82989054/dfe804c6-86ac-4e58-a9b0-0fcb80464d21)


<br/>
1. 토폴로지 및 트레이스와 같은 복잡한 ui 컴포넌트 구현<br/>
2. react-hook-form 및 yup을 이용해서 생성폼 자동화

<br/>
<br/>

# superPX

<br/>

데이터 베이스 활용가능한 코드 에디터
<br/>
(깃 랩으로 이전)
<br/>
![스크린샷 2023-06-15 오후 3 30 13](https://github.com/inth9198/inth9198/assets/82989054/bf28c93c-7909-4db1-951c-8bbdfbe91aeb)

<br/> 
1. 파일트리, 브랜치 앤 태그, ci/cd, 머지 리퀘스트, 커밋 히스토리, 에디터페이지 등 여러서비스 및 페이지 구현<br/>
2. 모나코 에디터를 이용하여 편집기능 구현<br/>
<br/>



# 온코리더

<br/>

## 프로젝트 설명

## 유전체 정보 기반으로 큐레이션 및 의견을 공유할 수 있는 플랫폼
<br/> 

(인턴 진행하면서 참여함)

![image](https://user-images.githubusercontent.com/82989054/183032376-28917d04-b619-4c80-88d4-3d8edee64693.png)

<br/> 
1. 로그인 페이지 및 기능 구현
<br/>

# 하우머치

<br/>

중고 가격 추천 웹페이지
(https://github.com/How-much-do-you-need/how-much)
<br/>
![스크린샷 2023-06-27 오후 12 38 02](https://github.com/inth9198/inth9198/assets/82989054/5d591abc-0c85-42fb-83f5-6001941b295c)
![스크린샷 2023-06-27 오후 12 38 14](https://github.com/inth9198/inth9198/assets/82989054/1d9c95f7-d9f2-4ac0-8847-bcc5ab92e378)

<br/>
1. 클라우드를 이용한 배포까지 완료
<br/>

# INEX

<br/>

ai를 활용한 본인 얼굴상에 가장 적합한 소속사를 찾아주는 웹페이지

<br/>

![image](https://github.com/inth9198/inth9198/assets/82989054/57edb3cb-e175-4638-a88b-d3a84d7121c1)


<br/>
https://staroasis.shop/
(서비스 주소)

<br/>

<br/> <br/> 

# 4차산업기술인재 - Web 1기
* 깃허브 주소
https://github.com/inth9198/4th_industrial_technology_talent
* 개발 노트
https://www.notion.so/softsquared/8d25c2cb344b40f6a05b9ff049dd55eb
* 마켓컬리 클로코딩
https://github.com/inth9198/4th_industrial_technology_talent
* 과제 시현 영상
https://www.notion.so/softsquared/3-React-Component-JSX-Props-3d4deb639b55487b8fa83d975eec2ea5

<br/>
<br/>

### 프로젝트 스킬 스택

- next.js - react framework
- redux(redux-saga) - status management
- emotion - css-in-js(style library)
- formik - form control library
- material-ui - ui library

<br/> 

### 프로젝트에 기여한점
데이터 파이프라인을 구축하여 정제된 데이터가 화면에 출력될수 있도록 함

react framework, redux-saga를 이용하여 로그인 시스템 구현

<br/> <br/> <br/> 

# 클래스액트 인턴(특허 기여)

## 임상 유전체, 임상시험 빅데이터 기반 암 진단 및 치료 지원 전문가 서비스

<br/> 

중소벤처기업부 R&D
빅데이터 기반
서비스 개발 사업

<br/> 

## 임상 참여 조건의 정형화를 위한 데이터 파이프라인 처리방법
1) 진행기간 : 2022/03/02~2022/06/30
2) 주요내용 : 환자에게 임상실험을 추천하는 알고리즘 (특허)
3) 출원 번호 : 10-2022-0067882
4) 논문명칭 : 임상 참여 조건의 정형화를 위한 데이터 파이프라인 처리방법
5) 본인이 기여한 점 : 모든 파이프 라인에 대한 코드 구현
6) 사용한 기술스택 및 지식 : 파이썬(주피터 노트북, pandas, numpy)
7) 결과 및 성과 : 이전 임상 실험 추천 시스템보다 더욱 구체화된 환자에 맞춤형 임상 실험 추천하는 파이프라인 구축

<br/> 

## 임상 참여 조건의 정형화를 위한 속성인식방법

1) 진행기간 : 2022/03/02~2022/06/30
2) 주요내용 : 환자에게 임상실험을 추천하는 알고리즘 (특허)
3) 출원 번호 : 10-2022-0067881
4) 논문명칭 : 임상 참여 조건의 정형화를 위한 속성인식방법
5) 본인이 기여한 점 : 모든 파이프 라인에 대한 코드 구현
6) 사용한 기술스택 및 지식 : 파이썬(주피터 노트북, pandas, numpy)
7) 결과 및 성과 : 이전 임상 실험 추천 시스템보다 더욱 구체화된 환자에 맞춤형 임상 실험 추천하는 파이프라인 구축을 위해 환자의 속성을 인식하는 방법

<br/> 

## 클래스액트 인턴(논문 참여)

1) 진행기간 : 2022/03/02~2022/06/30
2) 주요내용 : 부정을 처리하는 알고리즘 논문(예정)
3) 본인이 기여한 점 : 부정을 처리하는 알고리즘에 대한 모든 코드 적 부분의 구현과 여러 알고리즘 추가
4) 사용한 기술스택 및 지식 : 파이썬(주피터 노트북, pandas, numpy)
5) 결과 및 성과 : 이전에 부정을 처리하는 알고리즘 negex, deepen 에 비하여 더 좋은 성능을 나타냄 (더 높은 accuracy, 더 높은 precision, 더 높은 recall, 더 높은 f1, 더 적은 loss)


<br/> 
## 개발자가 된 후 스터디
<br/> 
### 프론트엔드 스터디(웹팩, 바벨, eslint, 도커, 리액트 쿼리, 최신 라이브러리나 스택등등)
<br/> 
https://rigorous-tibia-973.notion.site/f70fb898b13d482db2729b51b11a9234?pvs=4
<br/> 
### UX 스터디
<br/> 
https://www.notion.so/UX-a8800e77ae0745dab052eda00818e10a

<br/> <br/> 

![inth9198's GitHub stats](https://github-readme-stats.vercel.app/api?username=inth9198&show_icons=true&theme=radical)


# 🦀Skills
### Platforms & Languages

![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E.svg?&style=for-the-badge&logo=JavaScript&logoColor=white)
![HTML5](https://img.shields.io/badge/HTML5-E34F26.svg?&style=for-the-badge&logo=HTML5&logoColor=white)
![CSS3](https://img.shields.io/badge/CSS3-1572B6.svg?&style=for-the-badge&logo=CSS3&logoColor=white)
![React](https://img.shields.io/badge/React-61DAFB.svg?&style=for-the-badge&logo=React&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-3178C6.svg?&style=for-the-badge&logo=TypeScript&logoColor=white)
![Redux](https://img.shields.io/badge/Redux-764ABC.svg?&style=for-the-badge&logo=Redux&logoColor=white)
</br>
![Python](https://img.shields.io/badge/Python-3776AB.svg?&style=for-the-badge&logo=Python&logoColor=white)
![numpy](https://img.shields.io/badge/numpy-013243.svg?&style=for-the-badge&logo=numpy&logoColor=white)
![pandas](https://img.shields.io/badge/pandas-150458.svg?&style=for-the-badge&logo=pandas&logoColor=white)



## 백준 티어

[![Solved.ac Profile](http://mazassumnida.wtf/api/v2/generate_badge?boj=inth9198)](https://solved.ac/inth9198/)

<br/> <br/> <br/> 

## CS공부 노션
https://www.notion.so/171722de241544698e075f5fe42d06ce
<br/> <br/> <br/> 
https://www.notion.so/b7a400f79ff84743ae8254472646e7e6

## 네이버 블로그
https://blog.naver.com/inth2474
<br/> <br/> <br/> 

## 개발일지
https://www.notion.so/softsquared/8d25c2cb344b40f6a05b9ff049dd55eb
