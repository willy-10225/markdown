<h2 align="center">Appendix A: SQL-Django 語法對照</h2>

### A1-1 增、刪、改

| | SQL 語法 | Django 語法 |
| :---: | :--- | :--- |
| 新增 | insert into blog values(me, "title", "text") | Post.objects.`create(author=me, title="title", text="text")` |
| 修改 | update blog set title="title2" where id=20 | Post.objects.`get(id=20).update(title="title2")` |
| 刪除 | delete from blog where id=10 | Post.objects.get(id=10).delete() |

---
### A1-2 查

1. 基本條件

| | SQL 語法 | Django 語法 |
| :---: | :--- | :--- |
| 所有資料 | select * from blog | Post.objects.`all()` |
| 單篇內容 | select * from blog where id=10 | Post.objects.`get(id=10)` |
| 多篇列表 | select * from blog where author="Lynn" | Post.objects.`filter(author="Lynn")` |
| `小於 lt, lte`<br>`大於 gt, gte` | select * from blog where id<=15<br> select * from blog where id>40 | Post.objects.filter(id__lte=15)<br>Post.objects.filter(id__gt=40) |

2. 進階條件

| | SQL 語法 | Django 語法 |
| :---: | :--- | :--- |
| 範圍搜尋 | select * from blog where id in (10,20) | Post.objects.`filter(id__in=[10,20])` |
| 相似比對 | select * from blog where title like "%Django%" | Post.objects.`filter(title__contains="Django")` |

3. 其他查詢

| | SQL 語法 | Django 語法 |
| :---: | :--- | :--- |
| 資料排序 | select * from blog order by created_date desc | # 負號代表 desc 降冪排列<br>Post.objects.all().`order_by("-created_date")` |
| 計數 | select count(*) from blog where title like "%Django%" | Post.objects.filter(title__contains="Django").count() |
| 限定筆數 | select * from blog where author="Lynn" limit 1 | #取值結果加註 `first/last`<br>Post.objects.filter(author="Lynn").first() |
