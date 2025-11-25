<h2 align="center">Lesson 04: 動態化 MTV</h2>

### 4-1 動態化概念
1. models.py: 定義 Post 類別
- 透過 model 之創建、註冊，使用者可於管理介面 (127.0.0.1:8000/admin) 寫入資料。
- 當資料寫入時將`觸發 Post`，記錄作者、標題、內容及時間等類別屬性。

2. views.py: 提出`資料庫 query`
- 透過 view 之自訂函式，以 request 方法在前台頁面 (127.0.0.1:8000/blog) 顯示 html 結構。
- 若將 html 設定為動態格式，則可由外部`下 (類 SQL 查詢) 指令填入網頁架構`。

---
### 4-2 動態化 templates
```html
# 以 posts 代表 (系列) 文章列表，製作動態資料填入
(blog/templates/blog/post_list.html)
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Lynn's Page</title>
</head>

<body>
    <div>
    <!--以 % for...endfor %，定義迴圈範圍-->
    {% for post in posts %}
    <!--以 {{xxx}} 帶入動態參數，埋一項 id-->
    <h2><a href="/blog{{post.id}}">{{post.title}}</a></h2>
    <p>{{post.text|linebreaks}}</p>
    <p>發布時間：{{post.created_date}}</p>
    {% endfor %}
    </div>
</body>
</html>
```

---
### 4-3 views 取動態參數
```python
(blog/views.py)
from django.shortcuts import render
from .models import Post

def post_list(request):
    # 串聯資料庫取資料：符合查詢之結果，將成為一組 posts 列表 (參考 Appendix A)
    posts=Post.objects.all().order_by("-created_date")
    # {} 負責接收丟入 post_list.html 中的動態參數
    return render(request, "blog/post_list.html", {"posts":posts})
```
<br/>

```python
(terminal)
# 啟動 server，更改網址 127.0.0.1:8000/blog
python manage.py runserver 8000
```
★ <b>出現標題、內容及時間，代表 post_list.html 套用成功</b>。可在 127.0.0.1:8000/admin 新增一篇 post 測試。
