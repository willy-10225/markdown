<h2 align="center">Lesson 06: 前端表單整合 (2)</h2>

### 6-1 add_record 方法
```python
# 文本框雖然可輸入，但 submit 會出現換頁問題
(blog/urls.py)
from django.urls import path
from . import views

urlpatterns=[
    # 執行 view.py 之 post_list 方法
    path("", views.post_list, name="post_list"),
    # 執行 view.py 之 add_record 方法
    path("add_record", views.add_record, name="add_record")
]
```
<br/>

```python
# 仿照 post_list 方法，定義 submit 後之資料寫入及頁面重導向
(blog/views.py)
from django.shortcuts import render
from .models import Post
from .forms import PostForm

# get_user_model 取得 superuser (即文本的作者資料)
from django.contrib.auth import get_user_model
# 頁面重導向
from django.shortcuts import redirect
me=get_user_model().objects.get(username="samuel19950915")

def post_list(request):
    posts=Post.objects.all().order_by("-created_date")
    post_form = PostForm()
    return render(request, "blog/post_list.html", {"posts": posts, "post_form": post_form})

def add_record(request):
    # 檢查文本是否正常 submit (提出 request)
    if request.POST:
        # 取出 POST 字典的兩個鍵-值對   
        title=request.POST["title"]
        text=request.POST["text"]

        # 等同 SQL 之 insert 語法，created_date 已預設為 timezone.now()
        Post.objects.create(author=me, title=title, text=text)
    # 不另增 127.0.0.1:8000/blog/add_record，重導向功能定義頁面
    return redirect("/blog")
```
          
---
### 6-2 CSRF
```python
(terminal)
# 啟動 server，更改網址 127.0.0.1:8000/blog
python manage.py runserver 8000
```
✖ 出現各篇 post 及一組發布文本框，但發現 submit 之後仍然報錯。

```html
# CSRF-token: 為一資安防護管理措施，防止 cookies 遭竊取、盜用
(blog/templates/blog/post_list.html)
<!DOCTYPE html>
 ...
    <div>
    <h4>發布新文章：</h4>
    <form action="/blog/add_record" method="post" accept-charset="utf-8">
    {% csrf_token %}
    <table>
        {{post_form.as_table}}
    </table>
    <input type="submit" name="add_record" value="提交">
    </form>
    </div>
 ...
```
<br/>

```python
(terminal)
# 啟動 server，更改網址 127.0.0.1:8000/blog
python manage.py runserver 8000
```
★ <b>若 post 能成功新增至原頁面之頂篇，代表測試成功</b>。
