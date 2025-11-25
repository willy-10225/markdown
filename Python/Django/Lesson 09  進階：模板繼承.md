<h2 align="center">Lesson 09: 進階：模板繼承</h2>

### 9-1 繼承模板
```html
(blog/templates/blog/base.html)
{% load staticfiles %}

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css">
    <link href="https://fonts.googleapis.com/css?family=Nunito+Sans&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="{% static "css/blog.css" %}">
    <title>Lynn's Page</title>
</head>

<body>
    <div class="header"><h2>Lynn's Page</h2></div>
    <!--其他模板沿用時，在這段範圍輸入剩下程式即可-->
    {% block content %}
    {% endblock %}
</body>
</html>
```

---
### 9-2 沿用模板
```html
(blog/templates/blog/post_record.html)
<!--宣告套用 base.html 模板-->
{% extends "blog/base.html" %}

{% block content %}
<div class="header">
    <h2>{{post.title}}</h2>
</div>
<div class="content">
    <p>{{post.text}}</p>
    <p>{{post.created_date}} by {{post.author}}</p>
    <a href="/blog">回首頁</a>
</div>
{% endblock %}
```
<br/>

```html
(blog/templates/blog/post_list.html)
<!--宣告套用 base.html 模板-->
{% extends "blog/base.html" %}

{% block content %}
<div>
    {% for post in posts%}
    <h2><a href="/blog{{post.id}}">{{post.title}}</a></h2>
    <p>{{post.text|linebreaks}}</p>
    <p>發布時間：{{post.created_date}}</p>
    {% endfor %}
</div>

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
{% endblock %}
```
