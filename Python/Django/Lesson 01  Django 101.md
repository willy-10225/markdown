<h2 align="center">Lesson 01: Django 101</h2>

### 1-1 project 建置
```python
(terminal)
pip install django
# 創建 project，並啟動
mkdir myproject
cd myproject

# . 表示當前路徑。將看到 myproject 下有 manage.py 及 project 資料夾，為專案設定檔
django-admin startproject project .
# 將看到 myproject 下多了 db.sqlite3，為預設資料庫
python manage.py migrate

# 啟動 server，點擊網址 127.0.0.1:8000
python manage.py runserver 8000
```
★ <b>看到火箭圖表示測試成功</b>，Ctrl+C 退出測試。

---
### 1-2 APP 建置
```python
(terminal)
# 將看到 myproject 下多了 blog 資料夾，這種 app 層可有多個，確保元件分離、架構分明
python manage.py startapp blog
```

```python
(project/settings.py)
# 創建 app 後於設定檔寫入登錄
INSTALLED_APPS=(
    ...
    "blog"
)
```
<br/>

```python
(terminal)
# 設定 user, email, password
python manage.py createsuperuser

# 啟動 server，更改網址 127.0.0.1:8000/admin
python manage.py runserver 8000
```
★ 輸入 user, password，<b>看到 Django administration 表示測試成功</b>。

---
### 1-3 小結
1. 初步完成基本架構之建設<br>
包含執行 py 檔、資料庫 (db.sqlite3)，以及`雙層架構 (project 層：project+app 層：blog)`。

2. 雙層架構之規劃

| | 工作任務 |
| :---: | :---: |
| project 層 | `url-dispatch (上層)` 
| app 層 | `MTV(models, views, templates) 規劃`<br>url-dispatch (下層) |
