---
title: "github 블로그_프로필 이미지 모양 변경, 테두리 없애기"
date: 2020-06-07 15:00:00
categories: "BLOG_CUSTOM"
tags:
#excerpt:
comments: TRUE
---

minimal mistakes theme에서 블로그 왼쪽에 위치한 author avatar 이미지 (프로필 이미지) 모서리 모양과 테두리를 없애는 방법에 대한 기록입니다.  

------------------------------------------------------------------------------- 
minimal mistakes theme의 `config.yml`에서 아바타 이미지를 넣으면 아래와 같이 기본적으로 테두리가 있는 원형 이미지로 삽입된다.  
<img src = "https://user-images.githubusercontent.com/50826051/83965191-c5a69b80-a8ec-11ea-9df4-3a4e20854eba.png" width="25%">  

세로가 긴 이미지를 넣어서인지 몰라도, 타원형의 이미지 모양이 마음에 들지 않았고 테두리가 없는 깔끔한 이미지를 넣고 싶어서 커스텀하는 방법을 알아보게 되었다.  

`minimal-mistakes/_sidebar.scss`로 들어가서 'Author profile and links' 주석 내용을 찾아 아래의 하이라이트 한 부분을 수정한다.  

<img src = "https://user-images.githubusercontent.com/50826051/83965133-62b50480-a8ec-11ea-957d-3b4d0dbfb4ce.png">  
(1) 모서리 모양 변경  
`border-radius:` 를 자신이 원하는 숫자로 변경한다. 숫자에 따라 모양이 바뀌는데 아래의 사이트에 들어가서 숫자를 변경해보며 시뮬레이션을 통해 원하는 모양을 찾으면 된다.  
[사이트 링크](https://developer.mozilla.org/ko/docs/Web/CSS/border-radius)

<center><img src = "https://user-images.githubusercontent.com/50826051/83965315-ce4ba180-a8ed-11ea-95f4-69b66d4cf567.png" width="60%"><center>


(2) 테두리 없애기  

`padding: 0 px` 로 변경한다. 이 부분도 사실 본인의 기호에 맞게 숫자로 조절하여 테두리의 굵기를 정하면 된다.

최종 수정한 모습:point_down:  
<img src = "https://user-images.githubusercontent.com/50826051/83965393-75c8d400-a8ee-11ea-89c8-747ab8a8c694.png">

이렇게 하고나면 아래와 같이 프로필의 아바타 이미지 모양이 변경된 것을 확인할 수 있다.:sparkles:  
<img src = "https://user-images.githubusercontent.com/50826051/83965164-94c66680-a8ec-11ea-85c6-37a65b599f18.png" width="25%">  

  * 테두리를 없애니 속이 후련하다 ^^ 
