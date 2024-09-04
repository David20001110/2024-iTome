# Day 18 - 表單和模型表單（Forms and ModelForms）

- Forms 的作用
- 創建和使用 Forms
- 介紹 ModelForm 類
- 表單驗證

## 一、Forms 的作用

它接受用戶輸入的數據，進行驗證，並將這些數據存儲到資料庫中。
透過 Django 的表單系統可以輕鬆地創建和管理表單，大大簡化了表單 HTML 的生成和數據清除/驗證的過程。
#### 主要可以分為以下幾個方面
- 數據輸入：允許用戶透過網頁表單輸入數據。
- 數據驗證：驗證輸入的數據格式和內容，確保其正確性，防止應用程序錯誤或安全問題。
- 數據處理：驗證後的數據可以進一步處理，如保存到資料庫或發送電子郵件。
- 顯示和回饋：在出錯時，向用戶提供錯誤信息並顯示已輸入的數據。

## 二、創建和使用 Forms

我們可以使用 `forms.Form` 類來去創建一個聯絡表單

```python
from django import forms

class ContactForm(forms.Form):
    name = forms.CharField(label='名字', max_length=100)
    email = forms.EmailField(label='信箱')
    message = forms.CharField(widget=forms.Textarea, label='問題訊息')
```

建立完後我們可以在視圖中去使用這個 form
```python
from django.shortcuts import render
from .forms import ContactForm
from django.http import HttpResponseRedirect

def contact_view(request):
    if request.method == 'POST':
        form = ContactForm(request.POST)
        if form.is_valid():
            # 自定義處理表單數據
            return HttpResponseRedirect('/thanks/')
    else:
        form = ContactForm()

    return render(request, 'contact.html', {'form': form})
```
- 這裡會將建立的表單去做初始化，在 POST 中去處理數據，如果成功將會進一步處理並且可能會顯示感謝我們會盡快處理的頁面

## 三、介紹 ModelForm 類

ModelForm 類是基於模型（Model）的表單，可以自動生成與模型對應的表單字段。
這樣做的好處是減少了重複的 code，確保表單字段與模型字段保持一致，寫法有點類似前面講到的 serializer。

首先，我們定義一個 CustomUser 模型，包含用戶名、電子郵件和密碼
```python
from django.db import models

class CustomUser(models.Model):
    username = models.CharField(max_length=150, unique=True)
    email = models.EmailField(unique=True)
    password = models.CharField(max_length=128)

    def __str__(self):
        return self.username
```

接著，我們創建 ModelForm 表單

```python
from django import forms
from .models import User

class UserRegistrationForm(forms.ModelForm):
    class Meta:
        model = User
        fields = ['username', 'email', 'password']
        widgets = {
            'password': forms.PasswordInput(),
        }
```
- 將密碼字段設置為密碼輸入框。

建立完後我們可以在視圖中去使用這個 form
```python
from django.shortcuts import render, redirect
from .forms import UserRegistrationForm

def user_registration_view(request):
    if request.method == 'POST':
        form = UserRegistrationForm(request.POST)
        if form.is_valid():
            form.save()  # 這裡可以直接將資料儲存到資料庫
            return redirect('/welcome/')
    else:
        form = UserRegistrationForm()

    return render(request, 'registration_form.html', {'form': form})
```

## 四、表單驗證

Django 提供了多種驗證機制來確保數據的有效性。
#### 1. 內建驗證：
字段類型都自帶一些基本的驗證邏輯，比如 EmailField 會檢查輸入的內容是否是有效的電子郵件地址。

#### 2. 自定義驗證：
你可以透過覆寫表單類的方法來添加自定義驗證邏輯。例如，檢查兩個密碼字段是否一致：

```python
from django import forms

class RegistrationForm(forms.Form):
    username = forms.CharField(max_length=100)
    password1 = forms.CharField(widget=forms.PasswordInput)
    password2 = forms.CharField(widget=forms.PasswordInput)

    def clean(self):
        cleaned_data = super().clean()
        password1 = cleaned_data.get("password1")
        password2 = cleaned_data.get("password2")

        if password1 and password2 and password1 != password2:
            raise forms.ValidationError("兩組密碼不一樣")
```

#### 3. `clean_<fieldname>` :
這個好用的工具是去驗證單個字段，針對特定字段進行自定義驗證，`fieldname`指的是欄位。
例如假設有一個用戶註冊表單，其中包括用戶名和電子郵件字段，我們希望確保用戶名不包含任何不允許的字符（例如，!、@、# 等符號）

```python
from django import forms

class UserRegistrationForm(forms.Form):
    username = forms.CharField(max_length=150)
    email = forms.EmailField()

    def clean_username(self):
        username = self.cleaned_data.get('username')
        if any(char in username for char in '!@#$%^&*()'):
            raise forms.ValidationError("名字不能包含以下符號 !@#$%^&*()")
        return username
```
- 如果有錯誤將會拋出 ValidationError，並返回錯誤訊息

## 五、表單實例應用

這裡我們就接著使用 ModelForm 範例往下實作

先在 `urls.py` 中添加對應的 URL 配置
```python
from django.urls import path
from .views import user_registration_view
from django.shortcuts import render


urlpatterns = [
    path('register/', user_registration_view, name='register'),
    path('welcome/', lambda request: render(request, 'welcome.html'), name='welcome'),
]
```

需要在 `registration_form.html` 模板中顯示表單，並處理用戶的輸入
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>User Registration</title>
</head>
<body>
    <h1>User Registration</h1>
    
    <form method="post">
        {% csrf_token %}
        {{ form.as_p }}
        <button type="submit">Register</button>
    </form>

    {% if form.errors %}
    <div>
        <h3>Errors:</h3>
        <ul>
            {% for field in form %}
                {% for error in field.errors %}
                    <li>{{ field.label }}: {{ error }}</li>
                {% endfor %}
            {% endfor %}
        </ul>
    </div>
    {% endif %}
</body>
</html>
```

當用戶成功註冊後，可以重定向到一個歡迎頁面
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Welcome</title>
</head>
<body>
    <h1>Welcome!</h1>
    <p>Your account has been created successfully.</p>
</body>
</html>

```

## 參考資料
- https://developer.mozilla.org/zh-TW/docs/Learn/Server-side/Django/Forms