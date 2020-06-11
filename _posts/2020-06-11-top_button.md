---
title: "github 블로그_ back to top 버튼 만들기"
date: 2020-06-11 15:00:00
categories: "BLOG_CUSTOM"
tags:
#excerpt:
comments: TRUE
---

minimal mistakes theme에서 back to top 버튼을 만드는 방법에 대한 기록입니다.   

[이 github 사이트](https://github.com/mmistakes/minimal-mistakes/issues/1731)를 참고하여 만들었습니다.  

------------------------------------------------------------------------------- 
colab 노트북으로 공부한 내용을 기록하다보니, 포스트의 길이가 너무나 길어지게 되었다. 한번에 위로가기 버튼의 필요성을 느꼈고, 이 버튼을 뭐라고 부르는 지 모르겠어서, 방법을 찾는 데 꽤 애를 먹었다.  
영어로 back to top 버튼 정도로 말하는 것 같고, 잘 정리된 깃허브 사이트를 찾아서 따라했다.  


(1) `_sass/minimal-mistakes/_sidebar.scss` 로 들어가서  sidebar 내용이 시작되는 구간을 찾는다.
```
/* ==========================================================================
   SIDEBAR
   ========================================================================== */

/*
   Default
   ========================================================================== */
```  
이 내용 아래 부분에, 다음의 내용을 삽입한다.

```
.sidebar__top {
  position: fixed;
  bottom: 1.5em;
  right: 2em;
  z-index: 10;
}
```


(2) `_layouts/default.html` 로 들어가서 아래의 footer 내용을 찾는다.
```
 <div id="footer" class="page__footer">
      <footer>
        {% include footer/custom.html %}
        {% include_cached footer.html %}
      </footer>
    </div>

```
footer 내용 윗부분에, 다음의 내용을 삽입한다. 
```
<aside class="sidebar__top">
<a href="#site-nav"> <i class="fas fa-angle-double-up fa-2x"></i></a>
</aside>
```

이렇게 하고 나면, 블로그 오른쪽 하단에 아래와같은 버튼이 생긴 것을 확인할 수 있다. :sparkles:  
<img src ="https://user-images.githubusercontent.com/50826051/84364811-88ebe480-ac0b-11ea-961d-c5a2a582a264.png" width="10%">

