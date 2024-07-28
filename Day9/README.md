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

## 三、加載靜態文件
