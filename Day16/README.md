# Day 16 - 信號（Signals）
- signals 的運作
- signals 的使用
- 自定義信號器

## 一、signals 的運作

在 Django 中，信號（Signals）用於可以在特定事件發生時進行一些操作，主要作用是在應用程序內部解耦不同的部分，使得它們可以在不直接依賴彼此的情況下進行通信。
解耦不清楚的話我這邊用範例來解釋解耦在 Signal 的意義，假如用戶在註冊帳號時我希望當註冊的同時發一個歡迎加入的郵件，可以使用 Signals 將這些操作解耦。這樣不需要在用戶註冊的代碼中直接寫入發送郵件的功能，而是讓信號通知接收器來完成這項工作。
這樣，如果未來要修改或替換發送郵件的邏輯，只需修改信號接收器即可，不必動用用戶註冊的邏輯，還不懂的話可以自行 GPT。

### 主要主成部分

- 信號接收器（Receiver）：信號的處理函數，當信號發送時，這些函數將被調用。
- 信號發送器（Sender）：發送信號的對象，當特定事件發生時，信號將被發送。
- 信號（Signal）：信號的實例，用於連接信號接收器和信號發送器。

## 二、 signals 的使用

### 1. 接收和處理信號

```python
from django.db.models.signals import post_save
from django.dispatch import receiver
from my_app.models import UserProfile

@receiver(post_save, sender=UserProfile)
def user_created(sender, instance, created, **kwargs):
    if created:
        print('User created:', instance.username)

```
- 這裡的信號接收器，會在 `UserProfile` 模型的實例被創建時被調用。

### 2. 內建的信號
Django 提供了許多預定義的信號，在前面我們所使用的是模型實例保存時去調用我們建立的信號，也有其他的預定義信號。
- pre_save: 在模型保存之前發送。
- post_save: 在模型保存之後發送。
- pre_delete: 在模型刪除之前發送。
- post_delete: 在模型刪除之後發送。

### 3. 連接信號接收器的其他方法

在 `my_app/apps.py` 中連接信號接收器：

```python   
from django.apps import AppConfig

class MyAppConfig(AppConfig):
    default_auto_field = 'django.db.models.BigAutoField'
    name = 'my_app'

    def ready(self):
        import my_app.signals
```
- 這裡的 `ready` 方法會在應用的所有配置和初始化完成後被調用，這裡我們將信號接收器連接到 app。

```python
default_app_config = 'my_app.apps.MyAppConfig'
```
- 這裡的 `default_app_config` 用於指定 app 的配置類。

## 三、自定義信號

### 1. 定義自定義信號
```python
# my_app/signals.py 

from django.dispatch import Signal

my_custom_signal = Signal(providing_args=["user_profile", "request"])
```
- 定義一個名為 my_custom_signal 的信號，並且可以傳遞 'user_profile' 和 'request' 參數
- Django 提供了 django.dispatch.Signal 類來創建信號。

### 2. 寫一個信號器

```python
# my_app/handlers.py

def my_custom_signal_handler(sender, **kwargs):
    user_profile = kwargs.get('user_profile')
    request = kwargs.get('request')
    # 在這裡執行你想要的邏輯
    print(f"我的自定義信號被用戶: {user_profile} 和請求: {request} 觸發了")
```
- 會建立一個 `handlers.py` 去編寫信號器

### 3. 連接信號處理器
```python
# my_app/apps.py
from django.apps import AppConfig

class MyAppConfig(AppConfig):
    name = 'my_app'

    def ready(self):
        from .signals import my_custom_signal
        from .handlers import my_custom_signal_handler
        
        # 連接信號和信號處理器
        my_custom_signal.connect(my_custom_signal_handler)
```

### 4. 觸發信號

通常會是在 `views.py` 當中去觸發
```python
# myapp/views.py

from django.shortcuts import render
from .signals import my_custom_signal

def my_view(request):
    # 其他操作
    
    # 觸發信號，並傳遞 user 和 request 作為參數
    my_custom_signal.send(sender=None, user_profile=request.user_profile, request=request)
    
    return render(request, 'my_template.html')
```
- 當曜觸發信號時使用 send() 方法。
- 這時在 terminal 應該就會看到輸出"我的自定義信號被用戶: [user_profile] 和請求: [request] 觸發了"

## 四、總結

這篇文章我們介紹了信號在 django 中的作用包含解耦的定義、使用情境等以及它的運作方式，用實際的例子去操作一次。
下一篇我們將會介紹管理站台 (Admin site)

## 五、參考資料

- https://juejin.cn/post/7300789760703561754