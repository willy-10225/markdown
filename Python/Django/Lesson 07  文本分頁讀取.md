<h2 align="center">Lesson 07: 文本分頁讀取</h2>

### 7-1 post_record 方法
```python
# 欲讓各篇 post 在不同分頁顯示
(blog/urls.py)
from django.urls import path
from . import views

urlpatterns=[
    # 執行 view.py 之 post_list 方法
    path("", views.post_list, name="post_list"),
    # 執行 view.py 之 add_record 方法
    path("add_record", views.add_record, name="add_record"),
    # 執行 view.py 之 post_record 方法
    path("<int: id>", view.post_record, name="post_record")
]
```
<br/>

```python
# 仿照 post_list 方法，定義文章分頁後之資料讀取及顯示
(blog/views.py)
from django.shortcuts import render
from .models import Post
from .forms import PostForm

from django.contrib.auth import get_user_model
from django.shortcuts import redirect
me=get_user_model().objects.get(username="samuel19950915")

def post_list(request):
    posts=Post.objects.all().order_by("-created_date")
    post_form = PostForm()
    return render(request, "blog/post_list.html", {"posts": posts, "post_form": post_form})

def add_record(request):
    if request.POST: 
        title=request.POST["title"]
        text=request.POST["text"]
        Post.objects.create(author=me, title=title, text=text)
    return redirect("/blog")

def post_record(request, id):
    # get 篩選單篇文章：id 對應到一筆內容及一個顯示分頁
    post=Post.objects.get(id=id)
    # 以 locals() 泛括現有之所有 url 參數
    return render(request, "blog/post_record.html", locals())
```

---
### 7-2 動態化 templates
```html
# 自行創建檔案
(blog/templates/blog/post_record.html)
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Lynn's Page</title>
</head>

<body>
    <div class="header">
        <h2>{{post.title}}</h2>
    </div>
    <div class="content">
        <p>{{post.text}}</p>
        <p>{{post.created_date}} by {{post.author}}</p>
        <a href="/blog">回首頁</a>
    </div>
</body>
</html>
```
<br/>

```python
(terminal)
# 啟動 server，更改網址 127.0.0.1:8000/blog/<id>
python manage.py runserver 8000
```
★ 自由輸入\<id>數字，<b>若出現單篇 post 代表 post_record.html 套用成功</b>。

---
### 7-3 小結：至此完成前、後端之 MTV 整合
1. post_list<br>
使用者初接觸頁面，顯示資料庫之`所有現存文章`；表單文框允許 user 主動寫入操作。

2. add_record<br>
允許使用者新增、寫入內容於資料庫，並重導向回初始頁面 (post_list)。

3. post_record<br>
允許使用者`分頁讀取逐筆資料`，仿 post_list 以動態資料另製一 html 顯示模板。
