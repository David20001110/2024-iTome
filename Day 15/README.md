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

如果 models 之間有關聯像是外建字段或是多對多的關聯，我們可以使用嵌套序列化來處理這些關聯數據來處理關聯數據。例如，如果一個模型有一個外鍵字段，我們可以使用另一個序列化器來序列化這個外鍵字段的數據。

```python
from django.db import models

class Department(models.Model):
    name = models.CharField(max_length=100)
    description = models.TextField()

    def __str__(self):
        return self.name

class UserProfile
    username = models.CharField(max_length=10)
    is_authenticated = models.BooleanField(default=False, help_text='使用者是否有認證')
    message = models.TextField(max_length=100, null=True, blank=True)
    age = models.IntegerField(default=18)
    departments = models.ManyToMany(Department, on_delete=models.CASCADE, related_name='users')

    def __str__(self):
        return self.username
```
- 建立一個 Department 的模型  (記得要 migrate)
- 在 UserProfile 模型中新增一個多對多字段 `departments` 來關聯 Department 模型

```python
from rest_framework import serializers
from my_app.models import UserProfile, Department

class DepartmentSerializer(serializers.ModelSerializer):
    class Meta:
        model = Department
        fields = '__all__'
        
class UserProfileSerializer(serializers.ModelSerializer):
    departments = DepartmentSerializer(many=True)

    class Meta:
        model = UserProfile
        fields = ['username', 'is_authenticated', 'age', 'message','departments']
```
- 建立一個 Department 的序列化器
- 在 UserProfileSerializer 中新增 departments 的序列化器，因為可能會有多個要加上參數 `many=True`
- 這樣就可以將 Department 的數據序列化到 UserProfile 的數據中


#### 使用範例

```python
department = Department.objects.create(name="IT", description="Information Technology")
user_profile = UserProfile.objects.create(username="Alice", message="This is Alice profile.", age=30, departments=department)

serializer = UserProfileSerializer(user_profile)
print(serializer.data)

# {
#     "username": "Alice",
#     "message": "This is Alice profile.",
#     "is_authenticated": False,
#     "age": 30,
#     "departments":[
#            {
#                "name": "IT",
#                "description": "Information Technology"
#            }
#     ]
# }
```
- 這樣就可以將 Department 的數據序列化到 UserProfile 的數據中

## 三、create 和 update 方法的實作
我們能夠根據經過驗證的資料傳回完整的物件實例這件事在 serializer 裡面進行，我們可以去實作.create()和.update()方法或是只實作其中一個方法，因應自己的需求而定。

延續著上面的範例，實作兩個方法
```python
from my_app.models import Department

class UserProfileSerializer(serializers.ModelSerializer):
    departments = DepartmentSerializer(many=True)

    class Meta:
        model = UserProfile
        fields = ['username', 'is_authenticated', 'age', 'message', 'departments']

    def create(self, validated_data):
        departments_data = validated_data.pop('departments', [])
        user_profile = UserProfile.objects.create(**validated_data)
        departments = Department.objects.filter(id__in=departments_data).all()
        user_profile.department.set(departments) 
        return user_profile

    def update(self, instance, validated_data):
        departments_data = validated_data.pop('departments', [])

        instance.username = validated_data.get('username', instance.username)
        instance.email = validated_data.get('email', instance.email)
        instance.age = validated_data.get('age', instance.age)
        instance.save()

        instance.department.clear()  # 先清除所有關聯
        for department_data in departments_data:
            department, created = Department.objects.get_or_create(**department_data)
            instance.department.add(department)

        return instance
```

## 四、總結

## 五、參考資料
- https://blog.csdn.net/weixin_43956958/article/details/117920417
