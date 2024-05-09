---
title: "복습 - 리눅스 기본 명령어"
excerpt: "last_modified_at: 2021-07-14"
categories:
  - Blog
layout: post  
tags:
  - Blog
use_math: True
katex: True
toc: True
toc_sticky: False
toc_label: "페이지 주요 목차"
last_modified_at: 2021-07-14T08:20:00~22:00   
---


[![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https%3A%2F%2Frlagksqls17.github.io%2F2021-07-14-linux_basic%2F&count_bg=%2379C83D&title_bg=%23555555&icon=&icon_color=%23E7E7E7&title=hits&edge_flat=false)](https://hits.seeyoufarm.com)  


# 리눅스 기본 명령어  

**필자는 Git bash를 기준으로 설명함을 미리 알려드린다.**  

```$ whoami``` : 로그인한 사용자의 ID를 알려주는 명령어  
```$ passwd``` : 로그인한 사용자의 비밀번호를 변경하는 명령어  
```$ pwd``` : 현재 디렉토리 위치를 출력  
```$ ls``` : 현재 디렉토리의 목록을 출력  
```$ ls -l``` : 현재 디렉토리의 목록을 **상세히** 출력  
```$ ls -a``` : **숨겨진 파일**이나 디렉토리를 포함하여 출력  
```$ ls -al``` : 숨겨진 파일과 디렉토리를 포함하여 현재 디렉토리의 목록을 상세히 출력  
```$ cd 디렉토리 주소```  해당 디렉토리로 이동  
```$ 알고자하는 명령어 --help : ```사용하고자 하는 명령어 뒤에 붙여서 사용  
```$ chmod 777 파일명.txt``` : 파일의 권한 변경 (777은 모든 권한)   
```$ chown [소유할 사용자]:[소유할 그룹][변경 희망하는 디렉토리나 파일명]``` : 소유권 변경   
ex) chown user:use_group file.txt     
```$ mkdir [생성할 디렉토리 이름]``` : 디렉토리 생성  
ex) $ mkdir forder   
```$ touch``` : 빈 파일 생성  
ex) $ touch file.txt  
```$ rmdir``` : 디렉토리 삭제  
ex) $ rmdir forder  
```$ rm [option][삭제할 파일 및 디렉토리 명]``` : 파일 및 디렉토리 삭제  
ex) 
$ rm -r forder : 디렉토리와 그 하부 파일까지 삭제  
$ rm -f forder : 삭제 여부 묻지 않고 바로 삭제 **주의**  
$ rm -i forder: 삭제할 것인지 확인  
$rm -rf forder: 삭제 여부 묻지 않고 하부 파일이 있는 디렉토리까지 삭제    
```$ cp [oprtion] [대상 위치 및 이름][복사하고 싶은 위치]``` : 파일 및 디렉토리 복사  
ex)  
$ cp directory/forder/file1.txt directory/forder2/file2.txt :   
파일 1의 내용이 파일 2의 내용으로 복사됨     
$ cp -r  ./directory/forder1 ./directory/forder2:   
폴더1와 폴더1 내 모든 파일이 폴더2로 전체복사  **(폴더1이 폴더2 안으로 들어감)**  
$ cp -p .... :    
소유주, 그룹, 권한, 시간 정보를 그대로 복사 
```$ mv ./forder2/file1.py ./folder1``` : 폴더 1에 파일 1을 이동함  
```$ cat [option] [파일이름]``` : 파일내용 출력  
ex) cat -n file1.txt: 왼쪽에 줄 번호와 함께 file1.txt 내용을 출력  
```$ find ./forder1/file1.py -type d:``` 디렉토리 내 파일 검색  
```$ find ./forder1/file1.py -type f:``` 폴더 내 파일 검색  
```$ grep [option][pattern][파일 명]``` : 파일 내에서 지정한 패턴이나 문자열을 찾은 후에 그 패턴을 포함하고 있는 모든 행을 출력  

```
<option>  
-i : 대소문자 구분하지 않고 검색  
-v : 패턴과 일치하지 않는 행을 출력    
-c : 패턴과 일치하는 행의 개수를 출력    
-w : 패턴과 단어 단위로 매칭되어야 출력  
```

```$ ps [option]:``` 프로세스 목록 보기  

```
<option>  
-e : 현재 실행 중인 모든 프로세스 정보 출력  
-f : 모든 정보 확인  
-a : 실행중인 전체 사용자의 모든 프로세스 출력  
-u : 프로세스를 실행한 사용자와 프로세스 시작 시간 등을 출력  
-x : 터미널 제어 없이 프로세스 현황 보기  

* ps -ef 와 같이 옵션을 조합할 수도 있음  
```

```$ kill [option]``` [PID: 프로세스아이디] :   프로세스 종료  

```  
<option>  kill은 옵션과 함께 프로세스 ID가 필요  
-l : 사용 가능한 시그널 목록을 출력  
-1(숫자) : 재실행  
-9 : 강제종료  
-15 : 정상종료  
```

```$ at[옵션][시간][날짜][+증가시간]```  : 지정된 시간에 1회 실행되는 작업 예약 명령어  

```  
option  
-m : 출력결과가 없더라도 작업이 완료될 때 사용자에게 메일을 보냄  
-f : 스크립트 파일 등르 실행할 때 사용  
-l : 예약된 작업 목록 출력, atq 명령어 또한 같은 동작을 수행  
-v ; 작업이 수행될 시간 출력  
-d : 예약된 작업을 삭제, atrm 명령어 또한 같은 동작을 수행  
```

---

### file redirection  

일반적인 표준 입력, 출력을 사용하지 않고 파일의 내용 자체를 입력스트림으로 사용  

```
ls > ls.txt #ls,txt에 ls의 출력내용이 저장됨
ls >> ls.txt #ls.txt가 이미 존재할 때 뒤의 내용에 추가하여 작성함
0> : 표준 입력, 1> : 표준 출력, 2> : 표준 에러  
```

```
cat < file1.txt > file2.txt  
1. file1.txt의 내용을 cat 명령어의 입력 스트림으로 전송  
2. cat 명령어는 file1.txt 파일의 내용을 출력  
3. cat 명령어의 출력 스트림을 file2.txt로 변경  
4. cat 명령어의 출력 스트림은 화면이 아닌 file2.txt에 저장됨  
```

### piping commands  

둘 이상의 명령어를 묶어 출력의 결과를 다른 명령으로 전환할 수 있는 기능 (출력 결과를 입력값으로 사용)   

```
head a.txt | grep [0-9] > result.txt
1. head 명령을 실행하여 a.txt의 첫 10줄을 출력  
2. 출력된 결과를 | (pipe)를 통해 grep 명령으로 전달  
3. 숫자가 포함된 행을 가진 행의 결과가 모두 출력됨  
4. result.txt에 출력값 저장  
```



### mount  

물리적인 보조기억장치(USB 등)를 디렉토리에 연결시켜줌  

리눅스 사용시 위와 같은 PnP기능이 작동하지 않아서 직접 연결을 해야함  

```
mount [option][device][directory]  
# device에 있던 내용들이 해당 directory에 나타남  
option
-a : /etc/fstab에 명시된 파일 시스템을 마운트 할 때 사용  
-t : 파일 시스템의 유형을 지정, 생략할 시 /etc/fstab 파일을 참조  
-o : 추가적인 설정을 적용할 때 사용, 다수의 조건을 적용할 때는 콤마로 구분  
remount [device][directory]  
# mount를 취소하는 명령어  
df
# 현재 mount 된 디스크 정보 출력    

```

## SSH

: Secure Shell의 줄임말로 네트워크를 통해 다른 컴퓨터에 접근하거나 그 컴퓨터에서 명령 실행 가능하게 해주는 프로토콜 : SSH 통해 다른 컴퓨터에 리눅스에 접속하여 명령어 및 프로그램을 실행 가능  
: 기존에는 TELNET도 있었지만 보안저그올 매우 치명적인 결함이 존재했음  

- 패킷 데이터를 암호화하지 않고 평문으로 그대로 보냈기 때문임  
- SSH는 데이터를 암호화하였음  
  : 우분투에서는 openssh 라는 패키지를 통해 SSH를 구동 가능  

```
dpkg -l | grep opensh  # opensh 설치 여부 확인 가능    
sudo apt-get install openssh-server #apt-get 명령어로 openssh-server 설치  
sudo service ssh start # ssh 서버 실행하기   
service --status-all | grep +  # ssh 서버가 잘 실행 중인지 확인  
sudo netstat -antp   
# ssh 포트 확인하기, 접속 위해 포트를 입력해주어야 하기 때문에 확인
```



# 마치며  

근 한달 동안 블로그에는 다음의 내용들이 들어갈 것 같다.  

1. 엘리스 교육과정 복습  
2. Github 블로그 만들기  
3. 내 블로그 리모델링하기  

  

엘리스 교육과정 듣기, 운동 매일 하기, 스터디 운영하기 등 벌려놓은 일들이 너무 많아서 하루하루가 바쁘다. 제대로 마무리 할 수 있을지 걱정도 앞서지만 그만큼 내가 성장하고 살아간다는 느낌에 계속 하는 것 같다.   

다음 복습 내용으로는 이고잉님에게 배운 Git 사용법을 포스팅하도록 하겠다. :)  

