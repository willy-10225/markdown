<h2 align="center">Lesson 05: 前端表單整合 (1)</h2>

### 5-1 表單元件
```python
# 自行創建檔案
(blog/forms.py)
from django import forms

class PostForm(forms.Form):
    # html 語法：<input type="text" maxlength=100>
    title=forms.CharField(max_length=100)
    text=forms.CharField(max_length=2000, widget=forms.Textarea())
```

---
### 5-2 動態化 templates
```html
(blog/templates/blog/post_list.html)
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Lynn's Page</title>
</head>

<body>
    <div>
    {% for post in posts%}
    <h2><a href="/blog{{post.id}}">{{post.title}}</a></h2>
    <p>{{post.text|linebreaks}}</p>
    <p>發布時間：{{post.created_date}}</p>
    {% endfor %}
    </div>

    <div>
    <h4>發布新文章：</h4>
    <!--按下 submit 後，觸發 action，並以 post 方式攜帶資訊-->
    <form action="/blog/add_record" method="post" accept-charset="utf-8">
    <table>
        <!--以 {{xxx}} 帶入動態參數，需在 views.py 定義-->
        {{post_form.as_table}}
    </table>
    <input type="submit" name="add_record" value="提交">
    </form>
    </div>
</body>
</html>
```

---
### 5-3 views 取動態參數
```python
(blog/views.py)
from django.shortcuts import render
from .models import Post
# 與 posts 邏輯相同，多接一個 post_form 物件的動態參數
from .forms import PostForm

def post_list(request):
    posts=Post.objects.all().order_by("-created_date")
    # 串聯資料庫，同時也取得文本框資訊：呼叫 forms.py 檔之 PostForm 函式
    post_form=PostForm()
    return render(request, "blog/post_list.html", {"posts": posts, "post_form": post_form})
```
<br/>

```python
(terminal)
# 啟動 server，更改網址 127.0.0.1:8000/blog
python manage.py runserver 8000
```
★ <b>出現各篇 post 及一組發布文本框，代表 post_list.html 套用成功</b>。

---
### 5-4 小結
1. 動態化 MTV<br>
其實就是將 templates 挖洞，並在 views.py 加入動態的資訊`接收 ({"posts"})`，藉以串聯資料庫。

2. 前端表單整合
- 加入 forms.py 物件並置於 templates 當中，同時將此動態資訊定義於 views.py 中。
- 差異在於 posts 取用資料庫現有內容，post_form 既尚未寫入資料庫，更遑論被讀取、顯示。

3. 整合策略
- add_record: 仿照 post_list 方法，定義 submit 後之`資料寫入及頁面重導向`。
- `文本分頁讀取`: 仿照 post_list 方法，定義文章分頁後之資料讀取及顯示。
