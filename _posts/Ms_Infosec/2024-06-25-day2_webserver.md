---
title: "[MS 정보보안] 2일차 - 웹서버"
excerpt: "Microsoft와 함께하는 2024 정보보안 인재 양성 프로그램"
categories: [Ms_Infosec]
tags: [study, infosec]
toc: true
toc_sticky: true
---

# 2일차 - 웹 서버

+ **<실습>** wireshark를 사용하여 http/https 사이트에 로그인을 시도한 패킷 비교해보기

![fail to bring image](/assets/Image/ms_infosec/day2_1.png)
![fail to bring image](/assets/Image/ms_infosec/day2_2.png)
![fail to bring image](/assets/Image/ms_infosec/day2_3.png)
![fail to bring image](/assets/Image/ms_infosec/day2_4.png)
![fail to bring image](/assets/Image/ms_infosec/day2_5.png)

## 가상머신

물리적 pc 혹은 서버 없이 pc나 서버를 구현할 수 있는 소프트웨어로, 컴퓨터 한 대의 물리적 자원(cpu, ram, gpu)을 논리적으로 분할하여 하나의 독립적인 컴퓨터 역할을 수행할 수 있게 해준다.  

## 리눅스

무료 유닉스 (made by 베네딕트 토르발스)  

installer disc: iso 확장자로 설치파일을 다운로드 
putty: 원격 접속 프로그램

리눅스의 terminal 창: @앞의 문자열이 계정, 뒤의 문자열이 서버 이름. ex.root@localhost
현재 어디 서버에 위치해 있는지 알기 편하도록 작명시 web-1, web-2 혹은 haksa-web 이런식으로 작명한다.  

명령어는 `명령어 -옵션 -인자값`으로 구성된다.  

절대경로를 통해 내 위치를 확인하고 싶으면, `pwd(path working directory)`명령어를 사용해 알 수 있다. 

![fail to bring image](/assets/Image/ms_infosec/day2_6.png)
![fail to bring image](/assets/Image/ms_infosec/day2_7.png)
![fail to bring image](/assets/Image/ms_infosec/day2_8.png)
![fail to bring image](/assets/Image/ms_infosec/day2_9.png)

어느 포트가 열려있는지 확인할 수도 있다. `netstat`명령어를 사용한다.
`netstat -antp`에서 a는 all, t는 tcp, p는 port.  

![fail to bring image](/assets/Image/ms_infosec/day2_10.png)
State에서 포트가 열려있는지 아닌지 알 수 있다. `ip:portnum`가 local address에 나와있다. State가 `LISTEN`인 것은 누군가의 접속을 기다리고 있는 상태이고, pid/program name이 포트를 열리게 한 프로그램이다.   

+ **PID** 
    프로세스는 현재 실행 중인 프로그램인데, 프로세스는 항상 인과관계를 갖는데, 부모-자식 관계를 가지고 있다. 부팅할 시 스케줄러가 실행한 프로세스를 제외하면 모든 프로세스는 부모 프로세스를 갖는다.  

    ![fail to bring image](/assets/Image/ms_infosec/day2_11.png)
    CMD는 실행중인 프로그램. PID는 프로세스 아이디, PPID는 부모 프로세스 아이디. 따라서 PID와 PPID를 통해 프로그램을 실행시킨 일련의 부모 프로그램들을 알 수 있다. 
    부팅할 때 스케줄러가 실행한 프로세스인 `systemd와 kthreadd` 프로세스만 부모 프로세스가 없다. 
 
    ![fail to bring image](/assets/Image/ms_infosec/day2_12.png) 
    `grep` 명령어는 뒤의 인자값이 포함된 줄만 보여주는 명령어. 따라서 위의 명령어는 앞의 명령어`ps -ef`의 결과값들 중에 `httpd`를 포함한 줄만 보여주게 된다.  

    ![fail to bring image](/assets/Image/ms_infosec/day2_13.png) 
    ![fail to bring image](/assets/Image/ms_infosec/day2_14.png) 

    현재 실행중인 프로세스들의 pid가 어떤 정보들을 가지고 있는지 실어서 보여주는 것. 
    exe파일은 바로가기가 걸려있는 파일이라서 보안적 관점에서 주시해야함. 

> netstat -antp  
> netstat -r (라우팅 테이블 검색하는 명령어)   
> ps -ef | grep  
> service httpd start   
> ping  
> ifconfig  
> nslookup  
> arp -a  
> ls -ltr  
> pwd  

+ netstat -r   
    Destination은 호스트 주소. 네트워크 주소. 대표하는 주소. 예를 들면 르네상스관 404호. 즉 개인이 가질 수 없는 주소임. 
 
+ arp -a  
    물리적 주소 == 2계층 하드웨어 주소 == mac 주소  
    인터넷 주소 == 3계층 논리적 주소 == ip주소  
    통신을 하기 위해서는 물리적 주소와 논리적 주소 모두 필요한데, 만약 통신하고 싶은 상대의 ip주소는 알 수 있지만 해당 논리적 주소의 물리적 주소 또한 알아야한다. 이를 알기 위해 사용하는 것이 `arp`이다. `arp`는 브로드캐스트로 "xx.x.x.x 를 이용하는 분은 맥주소를 알려주세요" 라고 뿌리고 반응을 얻어 물리적 주소를 얻게 된다.

## VI 편집기

다른 운영체제와 달리 리눅스는 보이는 창에서만 무언가를 실행할 수 있다. 따라서 vi 편집기를 실행하면 vi만 할 수 있는 것.  

> `:q` vi 편집기 종료 명령어

+ 리눅스에서 실행하는 프로그램 죽이는 것은 `kill`, vi에서는 `:q`.  
+ 프롬프트에서 명령어에 대한 결과를 모두 창에서 지우는 것은 `clear`.    
+ 현재 내가 작업하고 있는 위치를 알고 싶으면 `pwd(print working directory)`.  
+ vi 편집기를 실행하면서 파일을 생성하고 싶으면 `vi [fileName]`. 리눅스는 확장자의 개념이 없다. 확장자를 적지 않아도 된다. 생성된 파일에 원하는 내용을 적을 수 있고 다 적었다면 `esc`를 누르고 `:wq`를 입력하면 편집한 내용이 함께 저장되면서 종료된다. 저장이 잘 되었는지 확인하기 위해 `ls [fileName]` 명령어를 실행해보면 확인할 수 있다. 
+ `cat [fileName]`은 해당 파일의 내용을 터미널창에서 확인할 수 있다. 

![fail to bring](/assets/Image/ms_infosec/day3_1.png)

+ `:set nu`는 행을 옆에 보여준다. 

+ 입력모드로 들어갈 수 있는 문자는 `a`, `i`, `o` 이다.  
    - `a`를 누르면 커서가 그 다음 문자로 움직인다.  
    - `i`를 누르면 현재 커서가 그대로 있다.  
    - `o`를 누르면 커서가 그 아래 줄로 움직인다. 

+ `:q!`를 누르면 편집한 내용을 저장하지 않고 종료한다. `!`는 강제의 명령어다. 

키보드에서 화살표로 커서를 움직이는 것은 지양하는 것이 좋다. 화살표 대신 `h,j,k,l`을 사용하자. `h`는 좌, `l`은 우, `k`는 상, `j`는 하.  

![fail to bring](/assets/Image/ms_infosec/day3_2.png) 

+ `x`는 현재 커서에 있는 문자를 삭제한다.
+ `shift + R`는 현재 커서의 문자를 삭제하고 원하는 문자를 삽입한다.(한글이나 워드에서 insert기능) 
+ `u` 는 undo, crtl+z 기능을 한다. 

+ `/`는 뒤에 적은 단어를 찾아준다. ctrl+f의 기능과 비슷하다. 

+ `d`를 계속 누르면 한 줄 씩 지워진다. 지우고 싶은 행 수를 누르고 d를 누르면 그 행 수만큼 지워진다. 

+ `y`를 계속 누르면 한 줄 복사가 된다. 그리고 `p`를 누르면 paste 붙여넣기가 된다.  
+ 'dw`는 한 단어를 삭제한다. 이 때 단어는 띄어쓰기 만나기 전까지로 본다. 

**실습**
- exec.txt 파일 생성 & 내용 쓰고 저장(편한대로 좋아하는 노래 가사 적음)
    ![fail to bring](/assets/Image/ms_infosec/day3_3.png)
    ![fail to bring](/assets/Image/ms_infosec/day3_4.png)


