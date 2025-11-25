<h2 align="center">Lesson 10: 進階：線上佈署</h2>

### 10-1 Ngrok
1. [官網](https://ngrok.com)註冊、登入

2. 下載並解壓縮 ngrok.zip 檔<br>
首次執行需連結帳戶：輸入`ngrok authotoken+金鑰密碼`。

3. 將 ngrok.exe 檔放至 project 層資料夾下<br>
myproject 將多一個 exe 檔。
<pre>
注意：不是其下 project 或 blog 資料夾。
</pre>

---
### 10-2 HTTP tunnel
```shell
(ngrok.exe/terminal(2))
# 可查看 session statis，複製 Forwarding 之網址部分
ngrok(.exe) http 8000
```

```python
(project/settings.py)
...
DEBUG=False
# 新增以下兩行
ALLOWED_HOSTS=["(複製之網址部分)"]
STATIC_ROOT=os.path.join(BASE_DIR, "static")
...
```
<br/>

```python
(terminal)
# 若出現 xxx static files copied... 代表成功收集資源
python manage.py collectstatic
python manage.py runserver
```
★ 已成功佈署至線上，測試 Forwarding 網址是否正常顯示。
