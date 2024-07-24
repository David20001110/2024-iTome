# Day 5 - 
- 什麼是DRF
- 建立、註冊app
- 建立簡易的 Django API


## 一、什麼是DRF
DRF(Django REST framework) 是一個功能強大的工具包，專門為構建 Web API 而設計的套件。提供了一套簡潔且強大的工具來處理API的各個方面，如強大的序列化器、權限管理、通用 API 視圖。

DRF 的優點:
- 快速開發 API
- 強大的序列化器
- 內建認證和權限控制
- 客製化 API 視圖
- 自動生成 API 文檔

## 二、安裝 DRF
1. 安裝 DRF
    ```commandline
    poetry add djangorestframework
    ```
2. 將 rest_framework 添加到 Django 項目的 `INSTALLED_APPS` 中
    ```python
    INSTALLED_APPS = [
        ...
        'rest_framework',
    ]
    ```

## 二、建立、註冊app
昨天建立完專案之後今天我們要建立一個 Django app，並註冊到專案中。

1. 什麼是 app
    - Django app 是一個獨立的模塊或組件，每個 app 可以獨立開發和測試，它包含了模型、視圖、模板等等，以實際例子來說明，假如我想要建立一個線上書店，就會分別需要有管理使用者和書籍的 app 等等。


2. 建立 app
    ```commandline
    python manage.py startapp dataset
    ```
    - `dataset` 是 app 的名稱，可以自行更改
    - 執行完後會看到 app 的結構如下:  
    ![img.png](img.png)
        - models.py: 定義模型
        - views.py: 定義視圖
        - admin.py: 註冊模型到管理站台
        - apps.py: app 的配置文件
        - migrations: 存放數據庫遷移文件
        - tests.py: 測試文件 
   

3. 註冊 app

    - 在專案的 `settings.py` 中的 `INSTALLED_APPS` 中加入剛剛建立的 app
        ```python
        INSTALLED_APPS = [
            ...
            'dataset',
        ]
        ```

4. 設定路由
    - 在 app 中創建一個`urls.py`檔案
    - 在專案的 `urls.py` 中加入 app 的路由
        ```python
        from django.urls import path, include
        urlpatterns = [
        path('admin/', admin.site.urls),
        path('dataset/', include('dataset.urls')),
        ]
       ```

## 三、建立簡易的 Django API
1. 在 `views.py` 中建立一個 function 回傳 Hello World
    ```python
    from django.http import HttpResponse

    # Create your views here.
    def hello_world(request):
        return HttpResponse("Hello World")
   ```
2. 在 `urls.py` 建立對應的路由和 views 進行連結
    ```python
    from django.urls import path
    from dataset import views

    urlpatterns = [
        path('hello/', views.hello_world),
    ]
    ```
3. 啟動服務
    ```commandline
    python manage.py runserver
    ```
   
4. 打開瀏覽器輸入 `http://127.0.0.1:8000/dataset/hello/` 看到以下實際畫面就成功了  

    ![img_2.png](img_2.png)

## 參考資料
- https://blog.kyomind.tw/django-rest-framework-01/
- https://ithelp.ithome.com.tw/m/articles/10289337