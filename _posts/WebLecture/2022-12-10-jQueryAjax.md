---
title: "[웹프로그래밍응용] jQuery Ajax"
excerpt: "[웹프로그래밍응용] jQuery Ajax"
categories: [Weblecture]
tags: [web, study]
toc: true
toc_sticky: true
---

# jQuery Ajax : 파일 로드 및 서버와 Ajax 통신

## 파일 로드

> $(객체).load(filename);

+ 파일로드 시 오류가 발생한 경우 **예외 처리 방법**
    > $(객체).load(filename, function(responseText, status, xhr){...}  
    > function(responseText, status, xhr) ➡️ callBack 함수  

```html
    <script src="https://code.jquery.com/jquery-3.6.1.min.js"></script>
    <script>
        $(document).ready(function(){
            $("#btn01").click(function(){
                $("#result00").load("text.txt");
            });
            $("#btn02").click(function(){
                $("#result00").load("text.htm");
            });
            $("#btn03").click(function(){
                $("#result00").load("text.xml");
            });
            $("#btn04").click(function(){
                $("#result00").load("text1.txt", function(responseText, status, xhr){
                    if(status == "success") $("#result00").text(responseText);
                    if(status == "error") $("#result00").text("error : " + xhr.status + "/ " + xhr.statusText);
                });
            });
        })
    </script>

    <button id="btn01">txt 로드</button>   
    <button id="btn02">htm 로드</button>  
    <button id="btn03">xml 로드</button>
    <button id="btn04">txt 로드(예외처리)</button>
    <div id="result00"></div>
```    

## AJAX 메소드로 서버에 파일 로드

