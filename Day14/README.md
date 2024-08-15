# Day 14 - 序列化器 (Serializer)

- 為什麼使用Serializer
- 什麼是Serializer
- 創建基本的Serializer
- 使用ModelSerializer
- 自定義Serializer

## 一、為什麼使用Serializer

在 Django 中，序列化器（Serializer）是一種用於將複雜的數據類型（如 QuerySet 和 Model Instance）轉換為 Python 數據類型（如 dict、list 和 str）的工具。序列化器還可以將 Python 數據類型轉換為 JSON 或 XML 格式，以便在網絡上進行傳輸。

序列化器的主要作用是將數據序列化（轉換）為可供網絡傳輸的格式，或將網絡傳輸的數據反序列化（解析）為 Python 數據類型。序列化器通常用於處理 API 請求和響應，以便在客戶端和服務器之間傳輸數據。

## 二、什麼是Serializer

在 Django REST framework 中，序列化器（Serializer）是一種用於將複雜的數據類型（如 QuerySet 和 Model Instance）轉換為 Python 數據類型（如 dict、list 和 str）的工具。序列化器還可以將 Python 數據類型轉換為 JSON 或 XML 格式，以便在網絡上進行傳輸。

序列化器的主要作用是將數據序列化（轉換）為可供網絡傳輸的格式，或將網絡傳輸的數據反序列化（解析）為 Python 數據類型。序列化器通常用於處理 API 請求和響應，以便在客戶端和服務器之間傳輸數據。

## 三、創建基本的Serializer

在 Django REST framework 中，可以通過繼承 `serializers.Serializer` 類來創建自定義的序列化器。以下是一個簡單的示例：

```python
from rest_framework import serializers
  
class UserSerializer(serializers.Serializer):
    username = serializers.CharField(max_length=100)
    email = serializers.EmailField()
    age = serializers.IntegerField()
```

在這個示例中，我們定義了一個名為 `UserSerializer` 的
序列化器，它包含了三個字段 `username`、`email` 和 `age`。每個字段都是一個 `serializers.CharField`、`serializers.EmailField` 或 `serializers.IntegerField` 實例，用於定義字段的類型和屬性。

## 四、使用ModelSerializer

在 Django REST framework 中，可以通過繼承 `serializers.ModelSerializer` 類來創建與模型相關的序列化器。`ModelSerializer` 類自動根據模型類的字段來生成序列化器的字段，並提供了一些方便的功能，如模型實例的序列化和反序列化。

以下是一個使用 `ModelSerializer` 的示例：

```python
from rest_framework import serializers
from myapp.models import User

class UserSerializer(serializers.ModelSerializer):
    class Meta:
        model = User
        fields = ['id', 'username', 'email', 'age']
```

在這個示例中，我們定義了一個名為 `UserSerializer` 的 `ModelSerializer`，它與 `User` 模型相關聯，並包含了四個字段 `id`、`username`、`email` 和 `age`。`ModelSerializer` 類會自動根據 `User` 模型的字段來生成序列化器的字段，並提供了一些方便的功能，如模型實例的序列化和反序列化。

## 五、自定義Serializer

在 Django REST framework 中，可以通過繼承 `serializers.Serializer` 類來創建自定義的序列化器。自定義序列化器可以包含任意數量的字段，並可以定義字段的類型和屬性。

以下是一個自定義序列化器的示例：

```python
from rest_framework import serializers

class CustomSerializer(serializers.Serializer):
    field1 = serializers.CharField(max_length=100)
    field2 = serializers.IntegerField()
    field3 = serializers.BooleanField()
```

在這個示例中，我們定義了一個名為 `CustomSerializer` 的自定義序列化器，它包含了三個字段 `field1`、`field2` 和 `field3`。每個字段都是一個 `serializers.CharField`、`serializers.IntegerField` 或 `serializers.BooleanField` 實例，用於定義字段的類型和屬性。

## 六、小結

序列化器（Serializer）是一種用於將複雜的數據類型轉換為 Python 數據類型的工具，並可以將 Python 數據類型轉換為 JSON 或 XML 格式，以便在網絡上進行傳輸。在 Django REST framework 中，可以通過繼承 `serializers.Serializer` 類來創建自定義的序列化器，也可以通過繼承 `serializers.ModelSerializer` 類來創建與模型相關的序列化器。序列化器通常用於處理 API 請求和響應，以便在客戶端和服務器之間傳輸數據。

## 七、參考資料