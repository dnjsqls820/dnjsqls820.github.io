---
title:  'Github 구글 검색 노출 시키기' 
excerpt: 'Git블로그를 구글에 노출시켜보자' 
categories: 
 - tip 
tags:
  - [Github,blog,Google Search]
toc: true
toc_sticky: true
date: 2021-09-29
last_modified_at: 2021-09-29
---
# Github io 구글 검색 노출

---

Github 블로그를 만들고 여러 글을 작성했는데 구글에 아무리 검색을 해봐도 제 블로그가 나오지 않아 저 혼자만 보는 일기장이 되었습니다. 그래서 구글에 제가 쓴 글을 검색하면 노출시키기 위해 필요한 여러 정보를 찾아보며 공부한 내용을 정리했습니다.

---

## 구글 검색 원리

> 구글에서는 검색 서비스를 위해 아래와 같은 작업을 진행하고 있습니다.
>
> 1. 웹 크롤러를 이용해 공개된 웹 페이지 검색
>
> 2. 찾은 웹 페이지의 콘테츠 렌더링 및 색인 생성
>
> 3. 검색어와 색인을 이용해 결과 제공
>
>    [구글 검색 원리](https://www.google.com/intl/ko/search/howsearchworks/) 페이지에서 자세하게 나와있습니다.
>
> 구글에 제 블로그를 노출시키기 위해 구글 웹 크롤러가 제 사이트를 크롤링 할 수 있게 작업을 해주어야만 했습니다.

---

## Google Search Console에 사이트 등록

> 1. Google Search Console 에 접속합니다.
> 2. 속성 추가를 누릅니다.
> 3. 속성 유형 선택이 나오면 URL 접두어 방식에 Github io 주소를 입력하고 계속을 누릅니다.
>
> ![18c39b25088ce](https://cdn.imweb.me/upload/18c39b25088ce.png)
>
> 4. 계속을 누르면 해당 페이지의 소유권을 확인하는 절차를 진행합니다.
> 5. 제공하는 html을 다운받고, 다운 받은 파일을 github.io.root 경로에 업로드합니다.
>
> ![](https://user-images.githubusercontent.com/75882110/135209575-0d8d739a-eca6-4e05-ab9d-8baaf6481cfc.png)
>
> 6. 정상적으로 업로드 되었다면 소유권이 확인되었다는 창을 볼 수 있습니다.
>
>    ![GoogleConsoleConfirmDone](https://yammong.github.io/assets/postImages/20190403/GoogleConsoleConfirmDone.png)

## Sitemap.xml생성

> **sitemap.xml은자신의 사이트에 있는 페이지들이 어떤 구조로 되어 있는지를 웹 크롤러에게 전달해 주는 역할을 합니다.**
>
> 1. _config.yml 파일에 url 속성에 자신의 블로그 이름이 들어가 있는지 확인합니다.
> 2. sitemap.xml 파일을 자기 github.io root 경로에 생성합니다.
> 3. 아래 내용을 sitemap.xml에 추가합니다.
>
> ~~~xml
>     ---
>     layout: null
>     sitemap:
>       exclude: 'yes'
>     ---
>     <?xml version="1.0" encoding="UTF-8"?>
>     <urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
>       {% for post in site.posts %}
>         {% unless post.published == false %}
>         <url>
>           <loc>{{ site.url }}{{ post.url }}</loc>
>           {% if post.sitemap.lastmod %}
>             <lastmod>{{ post.sitemap.lastmod | date: "%Y-%m-%d" }}</lastmod>
>           {% elsif post.date %}
>             <lastmod>{{ post.date | date_to_xmlschema }}</lastmod>
>           {% else %}
>             <lastmod>{{ site.time | date_to_xmlschema }}</lastmod>
>           {% endif %}
>           {% if post.sitemap.changefreq %}
>             <changefreq>{{ post.sitemap.changefreq }}</changefreq>
>           {% else %}
>             <changefreq>monthly</changefreq>
>           {% endif %}
>           {% if post.sitemap.priority %}
>             <priority>{{ post.sitemap.priority }}</priority>
>           {% else %}
>             <priority>0.5</priority>
>           {% endif %}
>         </url>
>         {% endunless %}
>       {% endfor %}
>     </urlset>
> ~~~
>
> - 작성한 sitemap.xml 파일을 github에 업로드 합니다.

## robots.txt 생성

> **robots.txt는 웹 크롤러가 내 사이트의 정보를 크롤링 할 때 크롤링 할 정채을 결정해주기 위한 파일입니다. 참고하지 않았으면 하는 파일들과 참고 했으면 하는 파일들을 설정해주면 웹 크롤러가 해당 내용을 확인하고 내가 설정한 파일들만 참고합니다.**
>
> > 1. github.io root 경로에 robots.txt 파일을 생성합니다.
> >
> > 2. 아래 내영을 robots.txt 파일에 작성합니다.
> >
> >    ~~~
> >    User-agent: *
> >    Allow: /
> >    Sitemap: {{ '/sitemap.xml' | relative_url | prepend: site.url }}
> >    ~~~
> >
> >    - User-agent: 규칙을 지정할 검색엔진 봇의 이름을 지정합니다. *로 지정했다는 뜻은 AdsBot을 제외한 모든 크롤러에 규칙을 지정한다는 의미입니다.
> >    - Allow : 위에 언급한 User-agent가 크롤링할 루트 위치를 설정합니다. 사이트의 전체 페이지를 크롤링 범위에 포함하기 위해 '/'를 넣었습니다.
> >    - Sitemap : sitemap.xml의 위치를 입력합니다. jekyll을 이용해 root경로에 있는 sitemap.xml의 위치로 설정하였습니다.
> >
> > 3. 작성한 robots.txt 파일을 github에 업로드 합니다.

## Google Search Console에 sitemap 제출

> 1. [Google Search Console](https://search.google.com/search-console?hl=ko)에 접속합니다.
>
> 2. 본인의 github.io 속성을 선택하고, 왼쪽 메뉴에 색인 > Sitemaps에 들어갑니다.
>
> 3. 새 사이트맵 추가에 sitemap.xml을 입력하고 제출합니다.
>
>    ![](https://user-images.githubusercontent.com/75882110/135213357-cc9c0db4-25a7-4521-8d43-51978fd94cbc.png)
>
> 4. 제출이 완료되면 아래 같이 뜨며, 추가 된 url을 확인할 수 있습니다.
>
>    ![](https://user-images.githubusercontent.com/75882110/135213552-40c001c6-4274-4128-b0a7-019ae5cbaaa6.png)

