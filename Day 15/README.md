# Day 15 -  Serializer 進階
- 自定義字段驗證
- create 和 update 方法的實作
- 嵌套的序列化

## 一、自定義字段驗證
除了使用內建的驗證方法之外，還可以在序列化器中添加自定義的 `validate_<field_name>` 方法來處理其他的驗證邏輯。

```python

from rest_framework import serializers
from my_app.models import UserProfile

class UserProfileSerializer(serializers.ModelSerializer):
    class Meta:
        model = UserProfile
        fields = ['id', 'username', 'is_authenticated', 'age']
        # fields = '__all__'

    def validate_age(self, value):
        if value < 18:
            raise serializers.ValidationError("Age must be at least 18.")
        return value
```
- 這邊舉的自定義例子是年齡如果小於18就會回傳錯誤(可能是某個站台不接受未滿18歲的人註冊之類的)

#### 實際使用這個序列化器進行實際驗證的完整示例

```python
from my_app.serializers import UserProfileSerializer

# 假設我們有一個新用戶的數據
user_data = {
    'username': 'Curry',
    'email': 'curry@example.com',
    'age': 16  
}

# 初始化序列化器，並傳入數據
serializer = UserProfileSerializer(data=user_data)

# 檢查數據是否有效
if serializer.is_valid():
    # 如果有效，保存數據
    user_profile = serializer.save()
    print("User profile created:", user_profile)
else:
    # 如果無效，打印錯誤
    print("Validation errors:", serializer.errors)

# Validation errors: {'age': ['Age must be at least 18.']}
```

## 二、create 和 update 方法的實作
我們能夠根據經過驗證的資料傳回完整的物件實例這件事在 serializer 裡面實作，我們可以去實作.create()和.update()方法之一或兩者都建立，看自己的需求。

```python
class UserProfileSerializer(serializers.ModelSerializer):
    addresses = AddressSerializer(many=True)

    class Meta:
        model = UserProfile
        fields = ['username', 'email', 'age', 'addresses']

    def create(self, validated_data):
        addresses_data = validated_data.pop('addresses')
        user_profile = UserProfile.objects.create(**validated_data)
        for address_data in addresses_data:
            Address.objects.create(user_profile=user_profile, **address_data)
        return user_profile

    def update(self, instance, validated_data):
        addresses_data = validated_data.pop('addresses')
        instance.username = validated_data.get('username', instance.username)
        instance.email = validated_data.get('email', instance.email)
        instance.age = validated_data.get('age', instance.age)
        instance.save()

        instance.addresses.clear()
        for address_data in addresses_data:
            Address.objects.create(user_profile=instance, **address_data)
        return instance
```
