<h2 align="center">Lesson 02: MTV 架構 (1)</h2>

### 2-1 MTV 概論
1. 前、後台互動
<p align="center"><img src="https://upload.cc/i1/2021/11/02/u9qvlY.png"></p>

- 使用者透過 url 訪問 views，這是他們與後台互動的`視覺介面`(餐廳菜單)。
- 對於 user 的各項操作，models 扮演服務生接單，負責管理與後台資料庫的溝通。
- db 接收指令後，將蒐取到的資料 (炒好的菜) 給 views，最後遞由 templates `視覺化呈現` (盛盤)。

2. 角色類比

| | 角色 | 工作內容 |
| :---: | :---: | :--- |
| 點餐 | url+views | 1. 由 url 負責`各網頁的分流作業`，詳細事件則由 views 定義<br>2.交由 models 啟動後台資料庫之溝通 |
| 出餐 | views+templates | views 詳細定義了取需資料與呈現方式 (template，即 html 檔)，做為`資料庫及前台的整合`角色 |

---
### 2-2 創建 models
```python
(blog/models.py)
from django.db import models
from django.utils import timezone

class Post(models.Model):
    # 參照已創建之 superuser，以 CASCADE 定義參照外來鍵的相依關係
    author=models.ForeignKey("auth.User", on_delete=models.CASCADE, null=False)
    title=models.CharField(max_length=200)
    text=models.TextField()

    # 預設為發佈時間 (時區需至 project/setting.py 調整)
    created_date=models.DateTimeField(
        default=timezone.now)
    
    def publish(self):
        self.created_date=timezone.now()
        self.save()
        
    def __str__(self):
        return self.title
```
       
---
### 2-3 migration&registration
```python
(terminal)
# 會看到 blog/migrations 多了 0001_initial.py，產生一個建資料庫的腳本
python manage.py makemigrations

# 執行腳本，建立資料庫
python manage.py migrate
```

```python
(blog/admin.py)
from django.contrib import admin
# . 表示當前資料夾之下
from .models import Post
admin.site.register(Post)
```
<br/>

```python
(terminal) 
# 啟動 server，更改網址 127.0.0.1:8000/admin
python manage.py runserver 8000
```
★ <b>看到 Django administration 多了 BLOG/Posts，表示測試成功</b>，可於後台新增資料。
