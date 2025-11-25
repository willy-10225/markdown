<h2 align="center">Appendix B: 資料庫串接</h2>

### A2-1 MySQL 資料庫串接
```python
(project/settings.py)
DATABASES={
    "default"={  
    "Engine":   "django.db.backends.mysql",
    "NAME":     (dbname),
    "USER":     "root",
    "PASSWORD": (password),
    "HOST":     "localhost",
    "PORT":     "3306"
    }
}
```

---
### A2-2 資料庫同步
```python
(terminal)
# 查看目前綁定之資料庫
python manage.py inspectdb

# 同步處理：將結果存入 models 後，執行腳本
python manage.py inspectdb > models.py
python manage.py migrate
```
