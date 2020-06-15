---
title: "github 블로그_ 메뉴바 밑줄 색상 바꾸기"
date: 2020-06-15 12:00:00
categories: "BLOG_CUSTOM"
tags:
#excerpt:
comments: TRUE
---

minimal mistakes theme의 contrast 에서 메뉴 바의 밑줄 색상을 변경하는 방법에 대한 기록입니다.   

------------------------------------------------------------------------------- 

minimal mistakes의 contrast를 적용했을 때 기본적인 강조색은 빨~간 색이다.  
메뉴바 호버 시 나타나는 밑줄 색이나,   

<img src = "https://user-images.githubusercontent.com/50826051/84619949-9954d580-af11-11ea-9630-ab15e2757a6a.png" width="80%">

.  
.  
toc의 색상, 인용구 표시 등이 그렇다.   

<img src = "https://user-images.githubusercontent.com/50826051/84620038-dc16ad80-af11-11ea-8f03-1cf8f1b2c7be.png" width="80%">

포인트 강조 역할을 매우 충실히 수행하고 있는데, 나는 좀 더 눈이 편한 색상을 적용하고 싶어서 이를 변경하게 되었다.  
방법은 아주 간단하다!!  

(1) `/skins/_contrast.scss` 로 들어가서 `primary-color`를 찾는다.  
<img src = "https://user-images.githubusercontent.com/50826051/84620485-16347f00-af13-11ea-82a1-2604542db26f.png" width="50%">  

(2) `#ff0000` 을 자신이 원하는 색상 코드로 변경해주면 된다.  
색상 코드는 `#ff0000` 을 구글 검색창에 치면 아주 편하게 알 수 있다.  

<img src = "https://user-images.githubusercontent.com/50826051/84620564-53990c80-af13-11ea-86a4-5c13ebe53c3c.png" width="70%">  

현재 아주아주 빨간색으로 지정이 되어있는 것을 확인할 수 있다.  
마우스로 자신이 원하는 색상을 클릭하고, `HEX`에 나타난 색상코드를 복사하여 `#ff0000`을 대체해주면 된다.  
나는 옅은 회색(#a3a3a3)으로 선택을 하여 아래와 같이 변경을 해주었다.  

<img src = "https://user-images.githubusercontent.com/50826051/84621039-8a235700-af14-11ea-8d1e-2763f5bf1924.png" width="50%">  

이렇게 수정을 하고 나면, 아래와 같이 변경된 것을 확인할 수 있다. :sparkles:    

<img src = "https://user-images.githubusercontent.com/50826051/84620835-fce00280-af13-11ea-99f6-b7175998e3bd.png" width="80%">

.  
.  

<img src = "https://user-images.githubusercontent.com/50826051/84620933-403a7100-af14-11ea-909c-f3bd8ccfa1e8.png">
