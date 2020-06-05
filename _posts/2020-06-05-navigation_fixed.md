---
title: "github 블로그_메뉴 바 상단에 고정하기"
date: 2020-06-05 10:00:00
categories: "BLOG CUSTOM"
tags:
#excerpt:
comments: TRUE
---

  
minimal mistakes theme에서 블로그 상단에 위치한 navigation bar(메뉴 바)를 틀고정하는 방법에 대한 기록입니다.  
(아래로 스크롤해도 메뉴바가 사라지지 않고 계속 보이도록!)

-------------------------------------------------------------------------------  
  
 `_sass/minimal-mistakes/_masthead.scss` 로 들어가서 아래 내용을 찾아 하이라이트 한 부분을 수정한다.
 
 <img src = "https://user-images.githubusercontent.com/50826051/83829872-dc979300-a71e-11ea-906a-ccefe66b8846.png">
 
(1) `position: fixed;` 로 변경한다.  
(2) `z-index: 20;` 아래에 다음 내용을 삽입한다.

```
   width: 100%;
  background: white;  // set a color to hide content that may appear below masthead
  height: 70px; // need a magic number here which may break in different viewports 
``` 
> `height`의 경우, navigation bar 의 세로 길이로, 본인이 원하는 길이로 숫자를 조절해주면 된다.  

최종 변경한 모습 :point_down:  

<img src = "https://user-images.githubusercontent.com/50826051/83830374-f08fc480-a71f-11ea-8ce2-77c1c00e322b.png">


여기까지 진행하면 메뉴 바가 상단에 잘 고정되어있긴 하지만,  
아래와 같이 블로그 콘텐츠 내용의 상단이 메뉴 바에 가려지는 문제가 나타난다.

<img src = "https://user-images.githubusercontent.com/50826051/83830820-02be3280-a721-11ea-9f6b-151712fc95bf.png" width = "50%">

  
상단 내용이 잘리지 않도록 설정을 해보자.  

`_sass/minimal-mistakes/_base.scss` 로 들어가서 아래 내용을 찾아 하이라이트 한 부분을 수정한다.  

<img src = "https://user-images.githubusercontent.com/50826051/83831739-fdfa7e00-a722-11ea-9c31-c005830fa646.png">

(1) `padding: 70px;` 로 변경한다. 

최종 변경한 모습 :point_down:

<img src = "https://user-images.githubusercontent.com/50826051/83831936-677a8c80-a723-11ea-854b-b4c8e4c49256.png">

> 여기서 70px 이라는 숫자는 메뉴 바 세로 길이와 연관된다. 메뉴 바의 세로 길이 이상의 숫자로 설정해주어야 내용이 잘리지 않는다.  

이 설정까지 마치고 나면 아래와 같이 내용이 잘리지 않는 것을 확인할 수 있다. :sparkles:  
<img src = "https://user-images.githubusercontent.com/50826051/83830723-cbe81c80-a720-11ea-8047-665082eb8967.png" width = "50%">

------------------------------------------------------------------------------------------------
참고 사이트:  [https://github.com/mmistakes/minimal-mistakes/issues/490]
