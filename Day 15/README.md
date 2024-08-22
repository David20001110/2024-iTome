# Day 15 -  Serializer 進階
- 自定義字段驗證
- 嵌套的序列化
- create 和 update 方法的實作


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

## 二、嵌套的序列化

如果 models 之間有關聯像是外建字段或是多對多的關聯，我們可以使用嵌套序列化來處理這些關聯數據來處理關聯數據。

```python
class Order(models.Model):
    order_number = models.CharField(max_length=20)
    order_date = models.DateField(auto_now=True)
    user_profile = models.ForeignKey(UserProfile, related_name='orders', on_delete=models.CASCADE)

    def __str__(self):
        return self.order_number
```
- 建立 Order 的模型，它有一個外鍵字段 user_profile 關聯到 UserProfile 模型
- 使用 `related_name='orders'` 來設定反向關聯的名稱，這樣我們就可以使用 orders 來訪問與 UserProfile 關聯的所有 Order 實例
- 原則這樣 UserProfile 對上 Order 是屬於一對多的關係

```python
class OrderSerializer(serializers.ModelSerializer):
    class Meta:
        model = Order
        fields = ['order_number', 'order_date']


class UserProfileSerializer(serializers.ModelSerializer):
    orders = OrderSerializer(many=True, read_only=True)
    order_id_list = serializers.ListField(write_only=True, child=serializers.IntegerField(), required=False)

    class Meta:
        model = UserProfile
        fields = ['username', 'is_authenticated', 'age', 'message', 'orders']
```
- 在 UserProfileSerializer 中，我們添加了一個 orders 字段，它是一個 OrderSerializer 的實例，並設置了 many=True，這樣我們就可以序列化與 UserProfile 關聯的所有 Order 實例
- 多添加了一個 order_id_list 字段，它是一個整數列表，用於接收用戶提交的 Order 的 id 列表，這樣我們就可以通過 id 列表來創建與 UserProfile 關聯的 Order 實例

#### 關於參數的解釋
- 使用 read_only=True 來設置 orders 字段為只讀
- 使用 write_only=True 來設置 order_id_list 字段為只寫
- 使用 child=serializers.IntegerField() 來設置 order_id_list 字段的子字段為整數類型


## 三、create 和 update 方法的實作
我們能夠根據經過驗證的資料傳回完整的物件實例這件事在 serializer 裡面進行，我們可以去實作.create()和.update()方法或是只實作其中一個方法，因應自己的需求而定。

延續著上面的 UserProfileSerializer 範例來實作兩個方法 
```python
class UserProfileSerializer(serializers.ModelSerializer):
    orders = OrderSerializer(many=True, read_only=True)
    order_id_list = serializers.ListField(write_only=True, child=serializers.IntegerField(), required=False)

    class Meta:
        model = UserProfile
        fields = ['username', 'is_authenticated', 'age', 'message', 'orders']

    def create(self, validated_data):
        order_id_list = validated_data.pop('order_id_list', None)
        user_profile = UserProfile.objects.create(**validated_data)
        if order_id_list is not None:
            for order_id in order_id_list:
                user_profile.orders.add(Order.objects.get(id=order_id))
        return user_profile

    def update(self, instance, validated_data):
        order_id_list = validated_data.pop('order_id_list', None)
        if order_id_list is not None:
            instance.orders.clear()
            for order_id in order_id_list:
                instance.orders.add(Order.objects.get(id=order_id))

        for attr, value in validated_data.items():
            setattr(instance, attr, value)
        instance.save()
        return instance
```
- 在 create 方法中，先從 validated_data 中取出 order_id_list，遍歷 order_id_list 並將與這些 id 相關聯的 Order 實例添加到 user_profile.orders 中
- 在 update 方法中，先從 validated_data 中取出 order_id_list，然後清空 instance.orders，然後遍歷 order_id_list，並將與這些 id 相關聯的 Order 實例添加到 instance.orders 中，最後遍歷 validated_data，並將數修改後的資料保存到 instance 中

#### 實際使用這個序列化器進行實際驗證的完整示例
```python
from my_app.serializers import UserProfileSerializer, OrderSerializer

# 先建立兩個 Order 實例
order1 = Order.objects.create(order_number='0001', order_date='2022-01-01')
order2 = Order.objects.create(order_number='0002', order_date='2022-01-02')

# 建立一個新用戶的數據
user_data = {
    'username': 'Curry',
    'age': 18,
    'message': 'This is Curry profile.',
    'order_id_list': [order1.id, order2.id]
}

# 初始化序列化器，並傳入數據
serializer = UserProfileSerializer(data=user_data)

# 檢查數據是否有效
if serializer.is_valid():
    # 如果有效，保存數據
    user_profile = serializer.save()
    print("User profile created:", user_profile)
else:
    print("Validation errors:", serializer.errors)

```
這樣我們就可以根據用戶提交的數據來創建或更新 UserProfile 實例，並處理與 UserProfile 關聯的 Order 實例

## 四、總結
今天我們學習了如何在序列化器中添加自定義的字段驗證方法，以及如何使用嵌套序列化來處理模型之間的關聯數據。下一篇文章我們將會學習如何使用信號（Signals）。

## 五、參考資料
- https://blog.csdn.net/weixin_43956958/article/details/117920417
