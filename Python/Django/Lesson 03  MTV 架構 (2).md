<h2 align="center">Lesson 03: MTV 架構 (2)</h2>

### 3-1 url 分層管理
- project 層
```python
(project/urls.py)
from django.contrib import admin
from django.urls import path, include

urlpatterns=[
    # 127.0.0.1:8000/admin: admin 登入頁面
    path("admin/", admin.site.urls),
    # 127.0.0.1:8000/blog: 交由 blog.urls 管理
    path("blog/", include("blog.urls"))
]
```

- app 層
```python
# 自行創建檔案
(blog/urls.py)
from django.urls import path
from . import views

urlpatterns=[
    # 執行 view.py 之 post_list 方法：凡 127.0.0.1:8000/blog 之下網址的顯示，均透過 post_list 來定義
    path("", views.post_list, name="post_list")
]
```

---
### 3-2 post_list 方法
```python
# views, templates 即是 MTV 當中的 V&T
(blog/views.py)
from django.shortcuts import render

def post_list(request):
    # 以 request 方式，顯示 blog/post_list.html 檔案內容
    return render(request, "blog/post_list.html", {})
```

```html
# 自行創建路徑、檔案
(blog/templates/blog/post_list.html)
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Lynn's Page</title>
</head>

<body>
    <div>測試成功就會出現這段話喔！</div>
</body>
</html>
```
<br/>

```python
(terminal)
# 啟動 server，更改網址 127.0.0.1:8000/blog
python manage.py runserver 8000
```
★ <b>出現\<div>標籤之文字，代表 post_list.html 套用成功</b>。

---
### 3-3 小結
1. 點餐：url+views
- 透過 `url-dispatch`，分別在 project、app 層做了訪問頁面的分流。
- 目前使用者還不能與後台互動，但已經搭建並確認 models 可做為資料庫的對話樞紐。

2. 出餐：views+templates
- 配合 views.py 函式設定，定義了呈現方式及內容 (templates，即 html 檔)。
- 目前尚未定義動態之資料傳接，但已經確認 `V&T 兩者之整合`能做為資料庫及前台的橋樑。

3. MTV 串聯策略
- `動態化 MTV`：將嘗試把 views、templates 搭配動態資料，串接資料庫。
- 前端表單整合：於前台`加入表單供 user 操作`，確保資料能寫入資料庫，也可被取用呈現。
