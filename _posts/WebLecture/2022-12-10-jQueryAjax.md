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

### 코드 예제 

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

+ jQuery 메소드를 이용하여 서버에 파일을 로드할 수 있다.
+ 파일 종류: Text, HTML, XML, JSON
+ 전송 방식 : GET, POST

+ 주요 메소드
    > $.ajax(), $.get(), $.post(), $.getJSON(), $.parseJSON()  
    > load(), ajaxSend(), ajaxSuccess(), ajaxError()

+ **$.ajax()** : HTTP 요청 메소드  
    ```
    $.ajax({
        url: filename,
        type: GET/POST,
        dataType: txt, html, xml, json..,
        success: callBack
    });
    ```

### $.ajax() 사용 코드 예제

```html
    <script>
        $(document).ready(function(){
            $("#agax05").click(function(){
                var filename = document.ajax_test.filename.value;
                var selType = selectType(filename);
                // http 요청
                $.ajax({
                    url: filename,
                    type: 'GET',
                    dataType: selType,
                    success: Task(result)
                });
            });
        });

        function selectType(filename){
            switch(filename){
                case "text.txt": 
                    return ("text");
                
                case "text.htm": 
                    return ("html");

                case "text.xml": 
                    return ("xml");
                
                default:
                    return ("text");
            }
        }

        function Task(result){
            showResult(result);
        }

        function showResult(result){
            switch(document.ajax_test.showType.value){
                case "text":
                    $("#result").text(result); break;
                case "html":
                    $("#result").html(result); break;
            }
        }

    </script>

    <form name="ajax_test">
        <input type="radio" name="filename"  value="text.txt" checked />Text
        <input type="radio" name="filename"  value="text.htm" />HTML
        <input type="radio" name="filename"  value="text.xml" /> XML
        <input type="radio" name="filename"  value="text1.txt" />None
        <br>
        <input type="radio" name="showType"  value="text" checked /> .text(result)  
        <input type="radio" name="showType"  value="html" /> .html(result)
        <input type="radio" name="showType"  value="alert" /> alert(result) 
        <input type="button" id="ajax05" value="결과 보기">
        <div id="result"></div>
    </form>
```

### $.get()/$.post() 사용 코드 예제

> $.get(URL, 콜백함수)  
> $.post(URL, data, 콜백함수)

```html
    <script>
        $(document).ready(function(){
            $("#get01").click(function(){
                $.get("text.txt", function(result){
                    result00.innerText = result;
                });
            });

            $("#get02").click(function(){
                $.get("text.htm", function(result){
                    result00.innerHTML = result;
                });
            });
            
            $("#post01").click(function(){
                $.post("text.txt", {
                    // data..
                },
                function(data, status){
                    alert("Data : " + data + " Status : " + status);
                });
            });
        });
    </script>

    <button id="get01"> $.get() text" </button>
    <button id="get02"> $.get() html" </button>
    <button id="post01"> $.post() text" </button>
    <div id="result00"></div>
```