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
