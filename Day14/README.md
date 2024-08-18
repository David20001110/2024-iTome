# Day 14 - 序列化器 (Serializer)

- 為什麼使用 Serializer
- 創建自定義的 Serializer
- 使用 ModelSerializer
- 序列化器的基本使用


## 一、為什麼使用 Serializer

它屬於 DRF 中的一部分   。最主要的目的就是將資料做轉換以及資料驗證，數據的轉換可以分為序列化以及反序列化。

序列化是將 Django 模型的實例或查詢集轉換為 JSON 或 XML 等格式的數據，以便這些數據可以通過 API 傳遞給前端應用或其他系統。
而反序列化也可以將前端傳來的 JSON 或 XML 數據轉換回 Django 模型實例，在這過程中會對傳入的資料作驗證，確保資料的格式和內容是正確的，並保存到資料庫中。

## 二、創建自定義的Serializer

在 DRF 中，可以通過繼承 `serializers.Serializer` 類來創建自定義的序列化器。以下是一個簡單的示例：

```python
from rest_framework import serializers
  
class UserSerializer(serializers.Serializer):
    username = serializers.CharField(max_length=100)
    email = serializers.EmailField()
    age = serializers.IntegerField()
```

在這個示例中，我們定義了一個名為 `UserSerializer` 的
序列化器，它包含了三個字段 `username`、`email` 和 `age`。每個字段都可以去自定義字段的類型和屬性，和創建 Model 有點類似。

## 三、使用ModelSerializer

在 Django REST framework 中，可以通過繼承 `serializers.ModelSerializer` 類來創建 Model 相關的序列化器。`ModelSerializer` 類自動根據模型類的字段來生成序列化器的字段，並提供了一些方便的功能，如模型實例的序列化和反序列化。

以下我使用之前在`my_app`裡面所建立的 UserProfile Model 作為  `ModelSerializer` 的示例：

```python
from rest_framework import serializers
from my_app.models import UserProfile

class UserProfileSerializer(serializers.ModelSerializer):
    class Meta:
        model = UserProfile
        fields = ['id', 'username', 'is_authenticated', 'age']
        # fields = '__all__'

```

- 在這個示例中，我們定義了一個名為 `UserProfileSerializer` 的 `ModelSerializer`，它與 `UserProfile` 模型相關聯，並包含了四個字段 `id`、`username`、`is_authenticated` 和 `age`。
- 在顯示字段的 field 的地方我們也可以使用 `'__all__'` 來代表顯示 UserProfile 的所有字段

## 五、序列化器的使用

#### 使用序列化器將 Django 模型實例轉換為 JSON 格式

- 單個對象的序列化
    ```python
    from my_app.models import UserProfile
    from my_app.serializers import UserProfileSerializer
  
    user_profile = UserProfile.objects.get(id=1)
    serializer = UserProfileSerializer(user_profile)
    print(serializer.data)  # 將數據轉換為字典
    # {'id': 1, 'username': 'David', 'is_authenticated': True, 'message': 'This is David profile.'}
    ```

- 多個對象的序列化(many=True)
    ```python
    from my_app.models import UserProfile
    from my_app.serializers import UserProfileSerializer
  
    user_profiles = UserProfile.objects.all()
    serializer = UserProfileSerializer(user_profiles, many=True)
    print(serializer.data)  # 將數據轉換為字典
    # [{'id': 1, 'username': 'David', 'is_authenticated': True, 'message': 'This is David profile.'}, 
    #  {'id': 2, 'username': 'Kevin', 'is_authenticated': False, 'message': 'This is Kevin profile.'}]
    ```

#### 資料的反序列化，將 JSON 數據轉換回 Django 模型實例並進行資料驗證

```python
from my_app.serializers import UserProfileSerializer
  
data = {'username': 'Tom, 'is_authenticated': True, 'message': 'This is Tom profile.'}
serializer = UserProfileSerializer(data=data)
if serializer.is_valid():
    user = serializer.save()
else:
    print(serializer.errors)  # 打印驗證錯誤信息
```

## 五、總結
在本文中，我們介紹 Serializer 用來將模型數據轉換為 JSON 格式並進行驗證的工具。
可以手動創建自定義序列化器或使用 ModelSerializer 自動生成與模型字段對應的序列化器字段。
序列化器用於序列化模型數據，或反序列化 JSON 數據並進行驗證，是 API 開發中確保數據完整性的重要部分。
下一篇文章我們將會介紹信號（Signals）


## 七、參考資料
- https://blog.csdn.net/weixin_43956958/article/details/117920417