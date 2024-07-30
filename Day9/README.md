# Day 9 - Templates 進階
- 模板繼承
- 使用過濾器
- 加載靜態文件
   - 配置路徑
   - 使用CSS美化模板


## 一、模板繼承
模板繼承允許我們定義一個基礎模板，並在其他模板中擴展它。這有助於保持模板的一致性和重用性。
1. 創建一個 `base.html` : 定義頁面的通用結構
    - 在基本模板中，使用 {% block %} 標籤來定義佔位符。這些標籤內部的內容可以在子模板中被替換或擴展。
    - 佔位符（Placeholder）是指模板中被 {% block %} 標籤包圍的區域。這些區域定義了可以由子模板進行填充的部分。佔位符允許開發者定義一個基本模板，然後在子模板中定義具體的內容以覆蓋這些佔位符，實現模板繼承和重用。
    ```html
    <!-- templates/base.html -->
    <!DOCTYPE html>
    <html lang="en">
    
    <head>
        <meta charset="UTF-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>{% block title %}David Practise Site{% endblock %}</title>
    </head>
    
    <body>
        <header>
            <h1>David Practise Site</h1>
        </header>
        <main>
            {% block content %}{% endblock %}
        </main>
        <footer>
            <p>© 2024 David Practise Site</p>
        </footer>
    </body>
    
    </html>
    ```

2. 在 `index.html` 文件中繼承 `base.html`
    - 在子模板中，我們可以使用 {% extends %} 標籤來繼承基本模板，並使用 {% block %} 標籤來填充或覆蓋基本模板中的佔位符。
    ```html
    <!-- templates/myapp/index.html -->
    {% extends 'base.html' %}
    
    {% block title %}Home{% endblock %}
    
    {% block content %}
        <h2>Welcome to My Site!</h2>
        <p>{{ message }}</p>
    {% endblock %}
    ```

## 二、使用自定義過濾器
如果內建過濾器無法滿足需求，你可以創建自定義過濾器。在 app 的 templatetags 模塊中創建過濾器檔案，並註冊自定義過濾器。

1. 創建一個 `templatetags` 目錄
    - 在 app 的目錄下創建一個名為 `templatetags` 的 Package，並在其中創建 `custom_filters.py` 來建立過濾器。
    ```commandline
    myapp/
    ├── __init__.py
    ├── admin.py
    ├── apps.py
    ├── models.py
    ├── tests.py
    ├── urls.py
    ├── views.py
    └── templatetags/
        ├── __init__.py
        └── custom_filters.py
    ```

2. 在 `custom_filters.py` 文件中定義過濾器
    - 在 `custom_filters.py` 文件中定義一個自定義過濾器，並將其註冊到模板中。
    ```python
    # myapp/templatetags/custom_filters.py
    from django import template
    
    register = template.Library()
    
    @register.filter
    def add_str(value, arg):
        return f"{value} {arg}"
    ```
    - 在這個例子中，我們定義了一個名為 `add_str` 的過濾器，它將兩個字符串連接在一起。
    - template.Library() 用於註冊自定義過濾器。


3. 在模板中使用自定義過濾器
    ```html
    <!-- templates/myapp/index.html -->
    {% extends 'base.html' %}
    {% load custom_filters %}
    {% block title %}Home{% endblock %}
    
    {% block content %}
        <h2>Welcome to My Site!</h2>
        <p>{{ message|add_str:"from David" }}</p>
    {% endblock %}
    ```
    - 在模板中使用自定義過濾器，只需在變量後面使用 `|` 符號，然後跟上過濾器的名稱。
    - 在這個例子中，我們將 `message` 變量和字符串 "from David" 連接在一起。
    - 使用 `{% load custom_filters %}` 標籤來加載自定義過濾器。  


4. view 中傳遞參數
    ```python
    # views.py
    from django.shortcuts import render
    
    def index_view(request):
        return render(request, 'myapp/index.html', {'message': 'Hello'})
    ```
    - 在視圖中傳遞參數給模板


5. 啟動服務查看結果
    ```commandline
    python manage.py runserver
    ```
    - 這樣就成功連結我們建立的 template  
    ![img.png](https://github.com/David20001110/2024-iTome/blob/master/Day9/img.png?raw=true)
## 三、加載靜態文件
在 Django 中提供了靜態文件管理系統，使得在模板中加載 CSS、JavaScript 和圖像等靜態文件。靜態文件通常存儲在 `static` 目錄中。(這次我使用 css 來做示範)

首先，在 `settings.py` 文件中配置靜態文件的路徑 (這一步通常在建立專案時就會先預設好了)：
```python
STATIC_URL = '/static/'
```

1. 創建 `static` 目錄
    - 在 app 的目錄下創建一個名為 `static` 的目錄，並在其中存儲靜態文件。
    ```commandline
    myapp/
    ├── __init__.py
    ├── admin.py
    ├── apps.py
    ├── models.py
    ├── tests.py
    ├── urls.py
    ├── views.py
    ├── templates/
    ├── templatetags/  
    └── static/
        └── myapp/
            └── style.css
    ```
    ```css
    /* static/myapp/style.css */
    body {
    font-family: Arial, sans-serif;
    }
   
    h2 {
        color: blue;
    }
   
    p {
        color: green;
    }
    ```
2. 在 template 中使用 `{% load static %}` 標籤來加載靜態文件。
    ```html
    <!-- templates/myapp/index.html -->
    {% extends 'base.html' %}
    {% load custom_filters %}
    {% block title %}Home{% endblock %}
    
    {% block content %}
        {% load static %}
        <link rel="stylesheet" href="{% static 'myapp/style.css' %}">
        <h2>Welcome to My Site!</h2>
        <p>{{ message|add_str:"from David" }}</p>
    {% endblock %}
    ```
    - 使用 `{% static 'myapp/style.css' %}` 標籤來引用 `style.css` 文件。
    - 這個標籤會生成一個靜態文件的 URL，這樣我們就可以在模板中引用靜態文件了。


3. 重新啟動服務查看結果
    ```commandline
    python manage.py runserver
    ```
    - 這樣就成功連結我們建立的 css 靜態文件   
    ![img_1.png](https://github.com/David20001110/2024-iTome/blob/master/Day9/img_1.png?raw=true)

## 四、總結

在這篇文章中，我們學習了如何使用模板繼承、自定義過濾器和加載靜態文件。模板繼承允許我們定義一個基礎模板，並在其他模板中擴展它。自定義過濾器允許我們定義自己的過濾器，以滿足特定的需求。加載靜態文件允許我們在模板中加載 CSS、JavaScript 和圖像等靜態文件。在下一篇文章中，我們將介紹如何使用模型（Models）來定義資料庫表的結構。