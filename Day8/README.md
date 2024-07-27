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
       <h1>Welcome Django</h1>
   </body>
   
   </html>
   ```

## 二、基礎語法
模板語言包括標籤和過濾器，用來插入動態數據和控制HTML輸出。
- **變數**: 用 {{ }} 包圍  
    例如：
    ```html
    <p>{{ name }}</p>
    ```
- **標籤**: 用 {% %} 包圍，用來執行邏輯操作，如條件和迴圈：
  ```html
  {% if user.is_authenticated %}
     <p>Welcome, {{ user.username }}!</p>
  {% else %}
    <p>Please log in.</p>
  {% endif %}
  ```
- **過濾器**: 過濾器用 | 符號連接，用來修改變量的值
  ```html
  <p>{{ message|upper }}</p>
  ```

## 三、連接views
1. 在 app 的 `views.py` 中引入 `render` 函數
    ```python
    from django.shortcuts import render
    ```
2. 在視圖函數中使用 `render` 函數返回模板
    ```python
    def index_view(request):
        # 匹配到我們建立的 html 檔
        return render(request, 'my_app/index.html')
    ```
3. 在 app 的 `urls.py` 中設定路由
    ```python
    from django.urls import path
    from myapp import views

    urlpatterns = [
        path('index/', views.index_view, name='index_view'),
    ]
    ```
4. 執行查看結果
    ```commandline
    python manage.py runserver
    ```
    - 這樣就成功連結我們建立的 template
    ![img.png](img.png)

## 四、模板繼承
模板繼承允許我們定義一個基礎模板，並在其他模板中擴展它。這有助於保持模板的一致性和重用性。
1. 創建一個 `base.html` : 定義頁面的通用結構
    - 在基本模板中，使用 {% block %} 標籤來定義佔位符。這些標籤內部的內容可以在子模板中被替換或擴展。
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
    - 在子模板中，我們可以使用 {% extends %} 標籤來繼承基本模板，並使用 {% block %} 標籤來填充或覆蓋基本模板中的佔位符。
    ```html
    {% extends 'base.html' %}
    
    {% block title %}
        Index Page
    {% endblock %}
    
    {% block content %}
        <h1>Hello, Django!</h1>
    {% endblock %}
    ```

### 建立的步驟
1. 在 app 中的 `views.py` 中建立函數視圖
2. 在 app 中的 `urls.py` 中設定路由
3. 在 app 的 `templates` 目錄下建立模板
4. 在模板中使用基礎語法 (在模板中使用模板繼承)
6. 啟動服務查看結果

## 五、參考資料

