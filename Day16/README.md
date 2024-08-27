# Day 16 - 信號（Signals）
- signals 的運作
- signals 的使用

## 一、signals 的運作

在 Django 中，信號（Signals）用於可以在特定事件發生時進行一些操作，主要作用是在應用程序內部解耦不同的部分，使得它們可以在不直接依賴彼此的情況下進行通信。
解耦不清楚的話我這邊用範例來解釋解耦在 Signal 的意義，假如用戶在註冊帳號時我希望當註冊的同時發一個歡迎加入的郵件，可以使用 Signals 將這些操作解耦。這樣不需要在用戶註冊的代碼中直接寫入發送郵件的功能，而是讓信號通知接收器來完成這項工作。
這樣，如果未來要修改或替換發送郵件的邏輯，只需修改信號接收器即可，不必動用用戶註冊的邏輯，還不懂的話可以自行 GPT。

### 主要主成部分

- 信號接收器（Receiver）：信號的處理函數，當信號發送時，這些函數將被調用。
- 信號發送器（Sender）：發送信號的對象，當特定事件發生時，信號將被發送。
- 信號（Signal）：信號的實例，用於連接信號接收器和信號發送器。

## 二、 signals 的使用

### 1. 定義信號接收器

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

### 2. 連接信號接收器

在 `my_app/apps.py` 中連接信號接收器：

```python   
from django.apps import AppConfig

class MyAppConfig(AppConfig):
    default_auto_field = 'django.db.models.BigAutoField'
    name = 'my_app'

    def ready(self):
        import my_app.signals
```
- 這裡的 `ready` 方法會在 Django 啟動時被調用，這裡我們將信號接收器連接到 app。
### 3. 啟用信號

在 `my_app/__init__.py` 中啟用信號：

```python
default_app_config = 'my_app.apps.MyAppConfig'
```
- 這裡的 `default_app_config` 用於指定 app 的配置類。

### 4. 預定義信號
Django 提供了許多預定義的信號，在前面我們所使用的是模型實例保存時去調用我們建立的信號，也有其他的預定義信號。
- pre_save: 在模型保存之前發送。
- post_save: 在模型保存之後發送。
- pre_delete: 在模型刪除之前發送。
- post_delete: 在模型刪除之後發送。

