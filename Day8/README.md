# Day 8 - 模板（Templates）
- 什麼是 Template
- 建立template
- 基礎語法
- 連接views
- 模板繼承

## 一、什麼是 Template

模板是 Django 用來生成 HTML 的文件。
它們允許我們將數據插入到靜態 HTML 中，並使網站頁面動態化。
模板系統使用標籤和過濾器來控制展示和處理數據。

## 一、建立模板
1. 在 app 的目錄下建立一個 `templates` 目錄
2. 在 `templates` 目錄下建立一個 `index.html` 文件
3. 在 `index.html` 文件中輸入以下內容：
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <h1>Hello, Django!</h1>
</body>

</html>
```

## 二、基礎語法
- 變數
```html
{{ variable }}
```
- 標籤
```html
{% tag %}
```
- 註釋
```html
{# comment #}
```

## 三、連接views
1. 在 app 的 `views.py` 中引入 `render` 函數
```python
from django.shortcuts import render
```
2. 在視圖函數中使用 `render` 函數返回模板
```python
def index(request):
    return render(request, 'index.html')
```
3. 在 app 的 `urls.py` 中設定路由
```python
from django.urls import path
from myapp import views

urlpatterns = [
    path('index/', views.index, name='index'),
]
```

## 四、模板繼承
1. 創建一個 `base.html` 文件
```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>{% block title %}{% endblock %}</title>
</head>

<body>
    {% block content %}
    {% endblock %}
</body>

</html>
```

2. 在 `index.html` 文件中繼承 `base.html`
```html
{% extends 'base.html' %}

{% block title %}
    Index Page
{% endblock %}

{% block content %}
    <h1>Hello, Django!</h1>
{% endblock %}
```

## 五、完整範例
1. 在 `myapp/views.py` 中添加視圖函數：
```python
from django.shortcuts import render

def index(request):
    return render(request, 'index.html')
```

2. 在 app 的 `urls.py` 中設定路由
```python
from django.urls import path
from myapp import views

urlpatterns = [
    path('index/', views.index, name='index'),
]
```

3. 啟動服務
```commandline
python manage.py runserver
```

4. 打開瀏覽器輸入 http://
5. 查看結果

### 建立的步驟
1. 在 app 中的 `views.py` 中建立函數視圖
2. 在 app 中的 `urls.py` 中設定路由
3. 在 app 的 `templates` 目錄下建立模板
4. 在模板中使用基礎語法
5. 在模板中使用模板繼承
6. 啟動服務查看結果
[]: # (END)

[]: # (BEGIN)
# Day 9 - 靜態文件（Static Files）
