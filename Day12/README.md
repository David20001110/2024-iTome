# Day 13 - 資料庫查詢

- 建立查詢集
- 進階查詢

## 一、建立查詢集（QuerySet）

在 Django 中，查詢集（QuerySet）是一個可以執行查詢的對象，它允許我們從資料庫中檢索、過濾和排序資料。所有的查詢集都是由模型管理器（例如 objects）來生成的。

### 1. 獲取所有/單個對象 

- 要獲取模型的所有對象，可以使用 `all` 方法：
    
    ```python
    from my_app.models import UserProfile
    
    # 獲取所有對象
    users = UserProfile.objects.all()
    ```
  
- 要獲取模型的單個對象，可以使用 `get` 方法：

    ```python
    from my_app.models import UserProfile
  
    user = UserProfile.objects.get(username='David')
    ```

### 2. 過濾對象 

要過濾模型的對象，可以使用 `filter` 方法：

```python
from my_app.models import UserProfile

# 過濾對象
users = UserProfile.objects.filter(is_authenticated=True)
```

### 3. 排序對象

要對模型的對象進行排序，可以使用 `order_by` 方法：

```python
from my_app.models import UserProfile

# 排序對象
users = UserProfile.objects.order_by('username')
```

## 二、進階查詢

除了基本的查詢之外，Django 還提供了一些進階的查詢方法，用於執行複雜的查詢操作。  
(為了方便講解進階查詢的做法我這裡會新建不一樣的 models 做講解)

```python
from django.db import models

class Article(models.Model):
    title = models.CharField(max_length=200)
    content = models.TextField()
    created_at = models.DateTimeField(auto_now_add=True)
    author = models.CharField(max_length=100)

    def __str__(self):
        return self.title
```

#### 聚合函數

Django 提供了一些內置的聚合函數，用於對查詢集的結果進行聚合操作，如計數、求和、平均值等。

```python

from django.db.models import Count, Sum, Avg
from my_app.models import Article

# 計數
count = Article.objects.count()

# 求和
total_age = Article.objects.aggregate(Sum('content'))

# 平均值
average_age = UserProfile.objects.aggregate(Avg('content'))
```

#### 使用 Q 物件進行複雜查詢


Django 支持對查詢集進行連接操作，可以使用 `Q` 對象來創建複雜的查詢條件。

```python
from django.db.models import Q
from my_app.models import Article

articles = Article.objects.filter(Q(title='Django') | Q(title='Python'))
```

#### 排除篩選 
```python
non_django_articles = Article.objects.exclude(title__contains='Django')
```

#### 連接查詢 (Lookup)

Django 提供了一些內置的查詢操作，稱為 Lookup，用於對查詢集進行進一步的過濾。

```python
from my_app.models import Article

# 查詢標題包含 'Django' 的文章
articles = Article.objects.filter(title__contains='Django')

# 查詢標題以 'Django' 開頭的文章
articles = Article.objects.filter(title__startswith='Django')

# 查詢 created_at 大於 '2021-01-01' 的文章
articles = Article.objects.filter(created_at__gt='2024-01-01')

# 查詢 created_at 介於 '2023-01-01' 到 '2024-01-01' 的文章
articles = Article.objects.filter(created_at__range=('2023-01-01', '2024-01-01'))
```

#### 查詢查詢集中的特定字段
可以使用 values 或 values_list 方法來指定查詢集應該返回的特定字段。

```python
from my_app.models import Article

# 獲取所有文章的標題
titles = Article.objects.values_list('title', flat=True)
```


## 三、總結

在本文中，我們介紹了如何在 Django 中進行資料庫查詢。我們學習了如何創建查詢集、過濾對象、排序對象，以及如何使用進階查詢方法來執行複雜的查詢操作。下一篇文章將會介紹 Django REST framework 以及 RESTful API 的概念。

## 、參考資料
- https://ithelp.ithome.com.tw/articles/10301965