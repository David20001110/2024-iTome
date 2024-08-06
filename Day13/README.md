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



### 1. 聚合函數

Django 提供了一些內置的聚合函數，用於對查詢集的結果進行聚合操作，如計數、求和、平均值等。

```python

from django.db.models import Count, Sum, Avg

# 計數
count = UserProfile.objects.count()

# 求和
total_age = UserProfile.objects.aggregate(Sum('age'))

# 平均值

average_age = UserProfile.objects.aggregate(Avg('age'))
```

### 2. 連接查詢

Django 支持對查詢集進行連接操作，可以使用 `Q` 對象來創建複雜的查詢條件。

```python
from django.db.models import Q

```

### 3. 排除篩選 

## 、參考資料
- https://ithelp.ithome.com.tw/articles/10301965