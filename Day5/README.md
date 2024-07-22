# Day 5 - 
- 什麼是DRF
- 建立、註冊app


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
    pip install djangorestframework
    ```
2. 將 rest_framework 添加到 Django 項目的 `INSTALLED_APPS` 中
    ```python
    INSTALLED_APPS = [
        ...
        'rest_framework',
    ]
    ```

## 二、建立、註冊app
在這個章節我們會開始建立一個 Django app，並註冊到專案中。

1. 建立 app
    ```commandline
    python manage.py startapp myapp
    ```
    - `myapp` 是 app 的名稱，可以自行更改
    - 執行完後會看到以下結構，下面會加以介紹  
    ![img.png](img.png)
   

2. 註冊 app
    - 在專案的 `settings.py` 中的 `INSTALLED_APPS` 中加入剛剛建立的 app
    ```python
    INSTALLED_APPS = [
        ...
        'myapp',
    ]
    ```
3. 啟動 app
    - 註冊完後可以透過下列指令確認是否有創建成功：
    ```commandline
    python manage.py runserver
    ```
4. 確認是否建立成功
5. 結束


## 參考資料
- https://blog.kyomind.tw/django-rest-framework-01/