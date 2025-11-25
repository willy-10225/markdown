<h2 align="center">Lesson 08: 進階：視覺美化</h2>

### 8-1 三種美化策略
1. bootstrap: maxcdn(3.3.7)<br>
[maxcdn](https://getbootstrap.com/docs/4.0/getting-started/download/)為通用之 css 樣式表工具，明列各種設計模板，以此為美化基底。
  
2. google font CDN<br>
[google font](https://fonts.google.com/)提供各式字型 (font-family) 供搭配下載。

3. 自定義 css<br>
透過自定義檔，結合指定字型、區塊或段落範圍，做為視覺美化之延伸。

---
### 8-2 templates 設計
```css
<!--自行創建檔案、路徑-->
(blog/static/css/blog.css)
body{padding-left: 15px}
h2, h4{color: #FCA205; font-family: "Nuhito Sans"}

.header{color: #ffffff; font-size: 36pt; text-decoration: none}
.content{margin-left: 40px}
... 
```
<br/>

```html
(blog/templates/blog/post_list.html)
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <!--基底：maxcdn 樣式表工具-->
    <link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css">
    <!--字型：google font (可列舉多字型，詳載於自訂 css 檔中)-->
    <link href="https://fonts.googleapis.com/css?family=Nunito+Sans&display=swap" rel="stylesheet">
    <!--自定義 css 檔-->
    <link rel="stylesheet" href="/static/css/blog.css">
    <title>Lynn's Page</title>
</head>

<body>
    ...
</body>
</html>
```

---
### 8-3 static 變量
```html
# static 資料夾於 Django 中，自動被視為路徑變數 (可確保異動不影響路徑問題)
(blog/templates/blog/post_list.html)
{% load staticfiles %}

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <!--基底：maxcdn 樣式表工具-->
    <link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css">
    <!--字型：google font (可列舉多字型，詳載於自訂 css 檔中)-->
    <link href="https://fonts.googleapis.com/css?family=Nunito+Sans&display=swap" rel="stylesheet">
    <!--自定義 css 檔-->
    <link rel="stylesheet" href="{% static "css/blog.css" %}">
    <title>Lynn's Page</title>
</head>

<body>
    ...
</body>
</html>
```
<br/>

```html
# 同樣列於 post_record.html
(blog/templates/blog/post_record.html)
{% load staticfiles %}

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <!--基底：maxcdn 樣式表工具-->
    <link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css">
    <!--字型：google font (可列舉多字型，詳載於自訂 css 檔中)-->
    <link href="https://fonts.googleapis.com/css?family=Nunito+Sans&display=swap" rel="stylesheet">
    <!--自定義 css 檔-->
    <link rel="stylesheet" href="{% static "css/blog.css" %}">
    <title>Lynn's Page</title>
</head>

<body>
    ...
</body>
</html>
```
