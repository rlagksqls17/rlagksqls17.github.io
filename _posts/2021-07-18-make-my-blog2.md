---
title : "github 블로그 만들기 - 2 (블로그 개설)"  
excerpt : "last_modified_at: 2021-07-17"  
categories:  
- Blog  
tags:  
- Blog  
katex: True  
toc: True  
toc_sticky: False  
toc_label: "페이지 주요 목차"  
last_modified_at: 2021-07-17T08:20:00~23:00  
---

지난주 포스팅에 이어 이제 github 블로그를 직접 개설하기 위한 설명을 하도록 하겠다.  
최대한 따라가기 쉽도록 상세하게 작성할테니 꼭 성공하길 바란다.  

설명 전, **꼭 집고 넘어가야할 필수사항**이 몇 가지 있다.   

---

## **1. 이 글은 필자의 컴퓨터 운영체제인 윈도우 기반으로 설명되어 있다.**   
리눅스 운영체제를 쓰시는 분들은 [이 분의 블로그](https://devinlife.com/howto%20github%20pages/github-prepare/)를 방문하여 만들어보길 바란다.   
**필자도 이 분의 블로그에서 블로그 만드는 법을 배웠기 때문에** 유용한 정보를 많이 배울 수 있으리라 생각된다.  

## **2. 컴퓨터 환경에 따라 버그가 발생할 수 있다.**  
필자는 블로그 개설하는 데만 주말 이틀 밤을 꼬박 새웠다.  
코딩을 처음 하는 입장에서 버그 발생 시 고치는 방법을 구글링 하느라 시간을 다 보냈기 때문이다.  
하지만 이런 과정 또한 지식이 늘어나는 과정 중에 하나라고 생각하기 때문에 스스로 버그를 고쳐보도록 하자.    
**(사실 버그 질문이 들어왔을 때 필자가 제대로 그 버그를 고치리란 자신이 없다 ㅠㅠ)**      

## **3. 최대한 아래 설명사항에 맞게 똑같이 따라와 주시길 바란다.**  
안내 사항을 모두 따르게 될 경우 결국 모두 같은 형식의 블로그가 만들어 질 테지만,  
일단 블로그 만드는 법을 알고나서 각자 개성에 맞게 꾸미는 것이 순서라고 생각한다.  

---

###  git bash 계정 확인

1. visual studio 코드를 켠다.
2. Ctrl과 물결표시를 눌러 터미널을 실행하고 git bash로 바꾼다.  
(git bash선택 시 아래 사진처럼 나와야 한다)    
![image](https://imgur.com/P9d2Pui.jpg) 
(git bash가 안 뜰 경우 스튜디오 코드를 실행하지 말고  
[링크](https://gitforwindows.org/)에서 Download 하고 Git bash 프로그램을 실행한다.)  
3. 이제 다음 코드를 입력해서 현재 어느 git 계정에 연결되어 있는지 확인하자.  
```    
git config user.name  
git config user.email  
```

올바르게 자신의 git 계정 메일과 이름이 떴다는 가정하에 다음으로 넘어갈 것이다.  
**참고로 gitlab이 아닌 github.com 계정으로 블로그를 운영할 것이다.**  
만약 자신의 github 계정과 git bash가 연결되어 있지 않으면 **꼭 연결**을 하고 넘어가자.  
왠만하면 본계정으로 쓰자. 부계정으로 git 계정 변경하기가 번거로울 수 있다.  
[github 기본 계정 연결](https://jfbta.tistory.com/6)  
[github 계정 변경](https://meaownworld.tistory.com/78)      

---

### Ruby 설치 (윈도우)  
1. (**새 탭 열기로**) [이 링크](https://www.ruby-lang.org/ko/)에 접속한다.  
2. 홈페이지 상단의 Ruby 다운로드 버튼을 클릭하면 아래와 같은 화면이 뜬다.      
   ![image](https://imgur.com/9H5iRpE.jpg)  

3. 위와 같이 rubyinstaller를 클릭

4. 클릭 후 뜨는 화면에서 Download 빨간색 버튼을 클릭

5. 여기까지 하면 아래와 같이 버전화면이 뜨는데,  

![image](https://imgur.com/ZJhce8O.jpg)   

여기서 우리는 가장 안전한 버전인 Ruby \+ Devkit 2.6.8\-1(x64)를 다운할것이다.  

6. 따라서 표시된 부분을 클릭

7. 이후 하단에 exe 파일이 다운되면 해당 파일을 클릭

8. 창이 뜨면 accept 클릭하고 이어서 next 클릭

9. 설치 경로가 뜨는데, 경로는 수정하지 않는다.  

10. install 클릭

11. 옵션 모두 체크 후 (이미 체크 되어 있을 것이다) next 클릭

12. 용량이 꽤 많아서 시간이 걸릴 것이다. **(잠시 쉬자 ㅎ..)**    

13. 다운이 완료되면 자동으로 cmd 창이 뜨는데, 여기서 엔터한번 눌러주자.  

14. 음... 막 여러가지를 설치하는데, 루비에 필요한 기본 요소들을 설치하는거라고 생각하면 된다.  

15. 실행이 완료되면 cmd 창은 닫아두자.  

### Jekyll 설치 (윈도우)

1. 시작키를 누르고, Start Command Prompt with Ruby를 검색하여 실행한다.  

2. **자 여기서 jeckyll 설치 폴더를 정하자.**  
   - 바탕화면에 myblog라는 이름을 가진 폴더를 만든다.  

3. 다음 명령어 예제를 이용해 **실행 중인 검은색 창에서** myblog 폴더로 이동하자.  

```python  
cd .. # 전 디렉토리로 이동
cd 디렉토리명 # 디렉토리로 이동

#ex) 
cd c
cd users
cd joo
cd desktop
cd myblog
```

4. 이 경로에서 아래사진과 같이 명령어를 입력한다.  

![image](https://imgur.com/F7l70QP.jpg)  

설치하면서 jekyll이 뭔지 알아보자. [링크](http://t-robotics.blogspot.com/2016/04/jekyll.html#.YPLvk9Uzapp)  

설치가 완료되었다면 이제 다음 과정을 수행해보자.  

### jeckyll-minima  

github jeckyll 블로그에는 테마가 아주 많다.  
우리는 이 중에서 가장 기본적인 minima를 사용할 것이다.  
참고로 필자 블로그는 minimal-mistakes라는 테마를 사용 중인데,  
굳이 여러분들께 minima 테마를 추천드리는 이유는 후에 여러분들이 html코드를 직접 수정하며  
자신만의 블로그를 만들기에는 minima 만한게 없다고 생각하기 때문이다.  

지금 필자도 블로그를 리모델링 중인데,  
minimal-mistakes(현재 사용 중인 테마)는 이미 색이 다 입혀져 있기 때문에  
하나하나 다 수정해주어야 한다.  
**즉 이미 칠해진 물감을 지워주는 작업까지 해야한다.**  

따라서 나중에 좀 더 블로그를 꾸미기 비교적 간편한,    
**도화지 백지인 minima**를 알려드리려고 한다.  

1. 먼저 새 탭 열기로 [링크](https://github.com/jekyll/minima)에 접속한다.  

2. 자, 저번 수업시간에 배운 것처럼 아래사진을 참고하여 HTTP 링크를 복사한다.  

![image](https://imgur.com/nldV9Lr.jpg)

3. 이제 visual studio 나, git bash를 켠다.  

4. (bash 터미널에서) 예제 코드를 참고하여 myblog 폴더로 이동한다.  

```python
예제 코드
cd c
cd users
cd joo  
cd desktop
cd myblog
```

5. 아까 복사한 url을 이용해 이 폴더에서 git clone 명령어를 실행한다.  (코드 참고)

```python
git clone https://github.com/jekyll/minima.git  
# visusl studio에서 URL 붙여넣기 할 때는 컨트롤 + 쉬프트 + v를 누른다.  
# git bash에서 URL 붙여넣기 할 때는 컨트롤 + 쉬프트 + insert를 누른다.  
```
6. 아래 사진과 같이 떳으면 성공!  
   ![image](https://imgur.com/SY9fzJ9.jpg)  

7. 다시 Start Command Prompt with Ruby 창을 켜서   
8. **minima 폴더로 이동해서 아래 사진 처럼 bundle 입력한다.**  

![image](https://imgur.com/tss0DHb.jpg)  

### 중간점검  

자 이제 중간점검 한 번 해보자.  
아래 사진처럼 파란색 명령어를 입력하여 실행했을 때, (bundle exec ...) 

빨간색 부분의 링크가 뜨는데, 이 주소를 크롬에 입력해서 들어가보자.  
블로그가 떴으면 이제 거의 다 왔다고 보면 된다.  

![image](https://imgur.com/hoIGoRN.jpg)  

### 웹 호스팅  

이제 github에 내 블로그를 올려보자.  

1. github 본계정으로 들어가서, 새 repository를 만든다. (repositores - New 클릭)   

2. 이때 repository 이름은 아래 사진처럼 **자기 계정 이름.github.io**여야 한다.  
   (public으로 설정하자)  

![image](https://imgur.com/e2jtRCl.jpg)   

3. 아까 생성한 minima 폴더 이름은 이제 {자기 계정 이름}.github.io로 바꿔놓자.  

4. 이후 다음의 명령어들을 입력한다.  

```python  
cd {username}.github.io # 아까 바꿔놓은 폴더로 이동  
git remote remove origin # 연결 해제
git remote -v # 입력해서 아무것도 안 뜨면 연결 되어 있는 것이 없다는 거다.
git remote add origin {url} ### !!주목, url 획득 방법은 아래 사진에 설명되어 있다.
git push -u origin master
```

![image](https://imgur.com/oAlNVkh.jpg)

**이 사진처럼 올바른 url을 복사하자.**



5. 자, 이제 크롬에서 {username}.github.io를 쳐보자.  

[링크](https://hanbins-code.github.io/) 와 같이 뜬다면 축하합니다.  
당신은 github 블로그입니다.  

만약 뜨지 않는다면,  
안되는 가장 큰 이유는 레포지토리 이름이 안 맞아서이다.(본인도 여기서 애먹었었다.)  
다시 한번 레포지토리 이름을 확인해보자.  
 [이 블로그](https://devinlife.com/howto%20github%20pages/new-blog-from-template/)가 큰 도움이 될 것이다.  


다음 포스팅에서는 글 쓰기와 여러가지 기본 설정 하는 법을 설명하도록 하겠다.  

모두 고생 많았다. 박수 짞짞  

:)  
