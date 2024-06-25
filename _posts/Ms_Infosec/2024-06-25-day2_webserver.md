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
> ps -ef  
> service httpd start  

