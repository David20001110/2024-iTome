# Day 6 - 介紹 DRF 以及 RESTful
- 介紹DRF
- 介紹RESTful
- 使用Django REST framework創建API
- CRUD操作

## 一、介紹 DRF

1. 甚麼是 DRF  
   DRF(Django REST framework) 是一個功能強大的工具包，專門為構建 Web API 而設計的套件。提供了一套簡潔且強大的工具來處理API的各個方面，如強大的序列化器、權限管理、通用 API 視圖。

   DRF 的優點:
   - 快速開發 API
   - 強大的序列化器
   - 內建認證和權限控制
   - 客製化 API 視圖
   - 自動生成 API 文檔


2. 安裝 DRF
    ```commandline
    poetry add djangorestframework
    ```

3. 將 rest_framework 添加到 Django 項目的 `INSTALLED_APPS` 中
    ```python
    INSTALLED_APPS = [
        ...
        'rest_framework',
    ]
    ```
## 二、介紹 RESTful
在講 RESTful 時實際上是在談論 REST 架構風格以及它的實現方式，後綴`ful`通常指的是系統或API符合REST架構風格的程度，所以要先來介紹 REST 架構。

1. REST 介紹 :  
REST 的全名是 Representational State Transfer 它是一種軟體架構的設計風格，用於構建可擴展且易於維護的Web服務。它基於HTTP協議，使用簡單的URL結構和標準的HTTP方法   。

2. RESTful API的設計原則 :  
- **唯一資源識別符**: 使用URL(也稱為請求端點)來唯一標識資源。例如，/api/books/ 識別所有書籍資源，/api/books/1/ 識別ID為1的書籍。
- **HTTP方法**: 定義資源操作的標準四種方法
  - GET: 獲取資源
  - POST: 創建資源
  - PUT: 更新資源
  - DELETE: 刪除資源
- **狀態無關性**: 每個請求都應該包含足夠的信息來理解和處理請求，不依賴於服務器的任何存儲上下文。

## 三、CRUD 操作
CRUD 我們指的是在數據庫中執行的四個基本功能：創建（Create）、讀取（Read）、更新（Update）和刪除（Delete）。這些操作是任何數據驅動應用程序的基礎。在 RESTful API 的上下文中，CRUD 操作通常與 HTTP 動詞對應(以下圖表)：

| CRUD 操作 | HTTP 請求方法 |
|---------|-----------|
| CREATE  | POST      |
| READ    | GET       |
| UPDATE  | PUT/PATCH |
| DELETE  | DELETE    |

## 四、使用 DRF 創建 API

下面，我們來詳細說明上面說的每一個操作，並通過示例代碼展示如何在 DRF 中實現這些操作
，那因為接下來的章節才會詳細說明 models、views 等，所以這邊就不先做說名。

**示例**  
假設我們有一個簡單的圖書應用程序，包含一個書籍模型（Book）。首先定義這個模型：

```python
# models.py
from django.db import models

class Book(models.Model):
    title = models.CharField(max_length=100)
    author = models.CharField(max_length=100)
    published_date = models.DateField()
```

創建一個序列化器來處理這個模型：
```python
# serializers.py
from rest_framework import serializers
from .models import Book

class BookSerializer(serializers.ModelSerializer):
    class Meta:
        model = Book
        fields = '__all__'
```

進行文件遷移
```commandline
python manage.py makemigrations
python manage.py migrate
```

### 1. 創建資源 (Create): 創建操作用於新增一個新的資源。這通常對應於HTTP的 POST 方法。  
在視圖中實現創建操作
```python
# views.py
from rest_framework.response import Response
from rest_framework.decorators import api_view
from .serializers import BookSerializer

@api_view(['POST'])
def create_book(request):
    serializer = BookSerializer(data=request.data)
    if serializer.is_valid():
        serializer.save()
        return Response(serializer.data, status=201)
    return Response(serializer.errors, status=400)
```

加上對應的路由設置
```python
# urls.py
from django.urls import path
from . import views

urlpatterns = [
    path('books/', views.create_book),
]
```

使用方法  
> 創建書籍（Create Book）：
>- 發送 POST 請求到 `/books/`，請求體包含書籍信息。

### 2. Read (讀取): 讀取操作用於獲取現有資源的信息。這通常對應於HTTP的 GET 方法。  
在視圖中實現創建操作
```python

# views.py
from rest_framework import generics
from .models import Book
from .serializers import BookSerializer

# 獲取所有書籍
class BookList(generics.ListCreateAPIView):
    queryset = Book.objects.all()
    serializer_class = BookSerializer

# 獲取單本書籍
class BookDetail(generics.RetrieveUpdateDestroyAPIView):
    queryset = Book.objects.all()
    serializer_class = BookSerializer
```

加上對應的路由設置
```python
# urls.py
from django.urls import path
from .views import BookList, BookDetail

urlpatterns = [
    path('books/', BookList.as_view()),
    path('books/<int:pk>/', BookDetail.as_view()),
]
```

使用方法
>   讀取所有書籍（Read All Books）：  
> - 發送 GET 請求到 `/books/`。  

> 讀取單本書籍（Read Single Book）：  
> - 發送 GET 請求到 `/books/<int:pk>/`，其中 pk 是書籍的ID。  


### 3. Update (更新): 更新操作用於修改現有資源的信息。這通常對應於HTTP的 PUT 或 PATCH 方法。
在上面的`BookDetail`視圖中，我們已經處理了更新操作。具體來說，`RetrieveUpdateDestroyAPIView`通過 PUT 或 PATCH 請求來處理資源的更新。

使用方法
> 更新書籍（Update Book）：
> - 發送 PUT 或 PATCH 請求到 `/books/<int:pk>/`，請求體包含要更新的書籍信息。

### 4. Delete (刪除): 刪除操作用於刪除現有資源。這通常對應於HTTP的 DELETE 方法。
同樣，在上面的`BookDetail`視圖中，我們已經處理了刪除操作。`RetrieveUpdateDestroyAPIView`通過 DELETE 請求來處理資源的刪除。

使用方法

> 刪除書籍（Delete Book）：
> - 發送 DELETE 請求到 `/books/<int:pk>/`，其中 pk 是書籍的ID。

## 五、總結
在這篇文章中，我們介紹了 DRF 和 RESTful API 的基本概念，並通過一個簡單的示例展示了 CRUD 的操作。下一篇文章中，我們將介紹序列化器 (Serializer)


## 六、參考資料
- https://cindyliu923.medium.com/%E4%BB%80%E9%BA%BC%E6%98%AF-rest-restful-7667b3054371
- https://aws.amazon.com/tw/what-is/restful-api/