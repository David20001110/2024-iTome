# Day 6 - 視圖（Views）和 URLS

- 視圖和 URLS
- function based views
- 建立views
- 如何連接URL

在 Day5 的時候有建立簡易的 API。在這篇文章中，我們將繼上次來學習視圖和 URL 的如何運作。  
在 Django 中 Views 主要是呈現 WHAT information 給 client 端，而 URL 則是決定 WHERE 是這些 information 呈現的地方。我們可以將每個 View 和 URL 視為網站上的特定網頁。

## 一、視圖（Views）

視圖是 Django 應用程序的核心，它們會接收 HTTP 請求並返回 HTTP response，它可以是函數或基於類的視圖，class base view 的部份我們會在後面的章節才會詳細說明。


### 1. function based views
函數型視圖是以Python函數的形式定義的視圖，它接收一個 Web 請求並返回一個 Web 響應。它們簡單且易於理解，像是在 Day5 時所建立的範例 。

我們可以在一個 `myapp/views.py` 中建立不止一個函數視圖。(以下我使用 JSON 回應)
```python
from django.http import JsonResponse

def hello(request):
    return JsonResponse({"message": "Hello"})

def world(request):
    return JsonResponse({"message": "World"})
```

## 二、URLS
URL 配置（URLconf）可以指 Django 項目中的兩個層級，整個項目的URL配置以及每個應用（app）中的URL配置。
每個 URL 模式都與一個視圖函數或視圖類相關聯，當用戶訪問該 URL 時，Django將調用對應的視圖來處理請求。

在前面的章節就有分別提到這兩者 URL設置的不同，這邊再次提醒一次 Django 的 URL 設置是由專案的 `urls.py` 和 app 的 `urls.py` 兩個檔案組成。

### path() 函數
`path()` 函數是 Django 中定義 URL 模式的主要方法，它將特定的 URL 模式映射到相應的視圖。path 函數來自 `django.urls` 模組。

語法：
```python
path(route, view, kwargs=None, name=None)
```
參數說明
1. route:  
類型: 字符串  
它是用來匹配請求URL的部分簡單來說就是路徑。例如，`hello/`將匹配以 hello/ 結尾的URL。路由中可以包含動態參數，使用尖括號 < > 括起來，例如，`books/<int:id>/` 表示URL中包含一個整數參數 id。(day6時有提到)

2. view:  
類型: 可調用對象（如視圖函數或視圖類）  
當URL模式匹配成功時，調用的視圖函數或視圖類。這個視圖負責處理請求並呈現給 client。

3. name:  
類型: 字符串（默認為 None）  
為這個URL做一個命名，這樣可以在Django的其他地方（如模板中）使用這個名稱來引用這個URL。


4. kwargs:  
類型: 字典（默認為 None）  
傳遞給視圖的附加參數。這是一個可選參數，允許將附加的關鍵字參數傳遞給視圖函數。


## 三、完整範例
以下是完整的項目設置，展示如何創建和連接視圖和URL。(從建立完 app 後開始)

1. 在 `myapp/views.py` 中添加視圖函數：
```python
from django.http import JsonResponse

def hello(request):
    return JsonResponse({"message": "Hello"})

def world(request):
    return JsonResponse({"message": "World"})
```

2. 在專案的 `urls.py` 中引入 app 的路由 :
```python
# urls.py
from django.urls import path, include
from django.contrib import admin
    
urlpatterns = [
    path('admin/', admin.site.urls),
    path('myapp/', include('myapp.urls')),
]
```

3. 在 app 中的 `urls.py` 中設定路由 :
```python
# urls.py
from django.urls import path
from myapp import views

urlpatterns = [
    path('hello/', views.hello, name='hello'),
    path('world/', views.world, name='world'),
]
```

4. 啟動服務 :
```commandline
python manage.py runserver
```

5. 執行結果
- 打開瀏覽器輸入 http://127.0.0.1:8000/myapp/world/ 做查看
- 打開瀏覽器輸入 http://127.0.0.1:8000/myapp/hello/ 做查看

### 建立的步驟
1. 在 app 中的 `views.py` 中建立函數視圖
2. 在 `views.py` 中引入 `HttpResponse` or `JsonResponse` 類別
3. 在函數中返回一個 `HttpResponse` or `JsonResponse` 物件
4. 在專案的 `urls.py` 中設定路由
5. 在 app 的 `urls.py` 中引入剛剛建立的函數視圖
6. 啟動服務
7. 在瀏覽器中輸入對應的 URL 查看回傳結果


## 四、總結
視圖是 Django Web 應用程序的核心，它們接收 HTTP 請求並返回 HTTP 響應。在這篇文章中，我們學習了如何創建函數視圖和如何連接 URL。在下一篇文章中，我們將介紹如何使用模板（Templates）來渲染 HTML 頁面。

## 五、參考資料
- https://ithelp.ithome.com.tw/articles/10289461
- https://developer.mozilla.org/zh-TW/docs/Learn/Server-side/Django/Home_page