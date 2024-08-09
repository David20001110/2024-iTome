# Day 10 - 模型（Models）

- 介紹ORM
- 建立 Models
- 字段類型和屬性

在 Django 中，模型是與資料庫進行交互的一種方式。模型是一個類，它包含了定義資料庫表的字段和行為。Django 提供了一個 ORM（Object-Relational Mapping）工具，它允許我們使用 Python 代碼來定義模型，而不需要直接使用 SQL 語句。

## 一、ORM（Object-Relational Mapping）

ORM 是一種編程技術，ORM 的目的是讓開發者使用 Python 代碼來操作資料庫，而不需要直接使用 SQL 語句。這使得我們可以更容易地與資料庫進行交互，並且可以提高代碼的可讀性和可維護性。

## 二、建立 Models

ORM 提供了一個模型類（Model class）來定義資料庫表。每個模型類對應到資料庫中的一個表，每個模型類的屬性對應到表中的一個欄位，我們可以通過定義模型類來定義資料庫表的結構。每個模型類都是 `django.db.models.Model` 的子類。

### 1. 創建模型

```python
from django.db import models

class UserProfile
    username = models.CharField(max_length=10)
    is_authenticated = models.BooleanField(default=False, help_text='使用者是否有認證')
    message = models.TextField(max_length=100, null=True, blank=True)
    age = models.IntegerField(default=18)

    def __str__(self):
        return self.username
```

- 在這個例子中，我們定義了一個名為 `UserProfile` 的模型類，它包含了四個字段 `username`、`is_authenticated`、`message` 和 `age`
- 我們還定義了 `__str__` 方法，它返回模型對象的 `username` 屬性。


## 三、字段類型和屬性

### 1. 字段類型

Django 提供了多種字段類型，用於定義資料庫表中的字段。以下是一些常用的字段類型：

- `CharField`：字符字段，用於存儲短文本
- `TextField`：文本字段，用於存儲長文本
- `IntegerField`：整數字段，用於存儲整數
- `FloatField`：浮點字段，用於存儲浮點數
- `BooleanField`：布爾字段，用於存儲布爾值
- `DateTimeField`：日期時間字段，用於存儲日期和時間
- `ForeignKey`：外鍵字段，用於定義與其他模型的關係
- `ManyToManyField`：多對多字段，用於定義多對多關係
- `EmailField`：郵件地址字段，用於存儲郵件地址
- `URLField`：URL 地址字段，用於存儲 URL 地址
- `ImageField`：圖片字段，用於存儲圖片文件
- `FileField`：文件字段，用於存儲文件

### 2. 字段屬性

每個字段類型都有一些屬性，用於定義字段的特性。以下是一些常用的字段屬性：

- `max_length`：指定字段的最大長度
- `default`：指定字段的默認值
- `null`：指定字段是否可以為空
- `blank`：指定字段是否可以為空白
- `choices`：指定字段的選項
- `help_text`：指定字段的幫助文本
- `auto_now_add`：自動添加當前時間
- `auto_now`：自動更新當前時間

```python
class UserProfile
    username = models.CharField(max_length=10)
    is_authenticated = models.BooleanField(default=False, help_text='使用者是否有認證')
    message = models.TextField(max_length=100, null=True, blank=True)
    age = models.IntegerField(default=18)
```

- 在 UserProfile 定義了四個字段 `username`、`is_authenticated`、`message` 和 `age`
- `username` : 是一個 `CharField` 類型，最大長度為 10
- `is_authenticated` : 是一個 `BooleanField` 類型，默認值為 `False`，並且有一個幫助文本 `使用者是否有認證`
- `message` : 是一個 `TextField` 類型，最大長度為 100，可以為空值
- `age` : 是一個 `IntegerField` 類型，默認值為 18

## 四、總結

模型是 Django 中與資料庫進行交互的一種方式，它定義了資料庫表的結構，包括表名、字段、屬性等。Django 的 ORM 提供了一個模型類來定義資料庫表，並且可以通過對模型類的操作來對資料庫進行增刪改查的操作。在定義模型時，我們可以使用多種字段類型和屬性來定義字段的特性，以滿足不同的需求。在下一篇文章中，我們將介紹資料庫介紹、遷移和基本指令。

## 五、參考資料
- https://ithelp.ithome.com.tw/articles/10295259