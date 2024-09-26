# Day 20 - Session framework
- Session 是什麼、如何運作
- 在 Django 中使用 Session

## 一、Session 是什麼、如何運作

在 Session 是一種在網站應用中，允許伺服器在不同的 HTTP 請求之間保持對使用者資料的追蹤機制。
由於 HTTP 是一個無狀態的協議，每次請求都是獨立的，這裡的獨立指的是客戶端和服務端之間的消息是完全獨立沒有互通的，
這意味著伺服器在收到多個請求時無法知道這些請求來自同一個使用者。
這時候，Session 就成為了解決這個問題的重要工具。

**Session 運作流程：**  

1. **Session ID 與 Cookie**：當使用者第一次拜訪網站時，伺服器會生成一個 Session ID 並儲存在使用者的瀏覽器 Cookie 中。這個 Session ID 不包含使用者的具體數據，只是用來識別使用者的「號碼牌」。    


2. **伺服器儲存會話資料**：伺服器根據 Session ID 將會話相關的數據存放在伺服器端，像是使用者的偏好設定、登入狀態或購物車內容等。Django 提供多種方式儲存會話數據，包括資料庫、快取、檔案系統，甚至是直接儲存在 Cookie 中（但容量有限）。


3. **會話維持**：當使用者在網站中進行下一次請求時，瀏覽器會自動帶上 Session ID，讓伺服器根據這個 ID 讀取之前儲存的資料


4. **會話的過期與管理**：伺服器可以設定會話的有效期限。當會話到期後，伺服器會自動刪除對應的數據。這個過期時間可以設定為使用者關閉瀏覽器時自動失效，或是指定一段時間後過期。


> **我使用生活一點的例子來舉例**  
> 想像你去了一間餐廳點餐，餐廳給你一個號碼牌（Session ID），上面沒有任何個人資訊。接著餐廳把你點的菜單（會話數據）儲存在後面的大櫃子裡。當你需要加點或修改訂單時，你只需要出示號碼牌，餐廳就能根據號碼去到櫃子查到你之前點的餐。  
> 餐廳也會設定號碼牌的有效時間，如果你長時間不點餐（會話過期），餐廳就會把你的訂單記錄刪除，釋放空間。同樣地，如果你用完餐離開（關閉瀏覽器），號碼牌也會失效，餐廳就不再保存你的資料。

## 二、在 Django 中使用 Session

Django 提供了一個內建的 Session 框架，讓開發者能夠輕鬆地在應用中使用會話功能。
Session 框架的使用非常直觀，你可以在每個請求中存取、更新或刪除會話數據。

1. **啟用 Session**：
Django 默認情況下會啟用 Session 中介軟體，這是處理會話數據的核心。會在項目文件的 `INSTALLED_APPS` 和 `MIDDLEWARE` 部分中進行配置

    ```python
    INSTALLED_APPS = [
        'django.contrib.sessions',
    ]
    MIDDLEWARE = [
        'django.contrib.sessions.middleware.SessionMiddleware',
    ]
    ```
   
2. **Session 的儲存方式**：
預設會使用資料庫來儲存會話數據。預設情況下，`SESSION_ENGINE` 的值是 `django.contrib.sessions.backends.db`，這會將會話數據儲存在你的資料庫中。
如果你想改變儲存方式，例如使用快取、檔案系統或基於 Cookie 的會話儲存，你就需要手動設定 `SESSION_ENGINE` 來指定不同的儲存後端。

3. **操作 Session**：
在視圖（views）中從 request 訪問 session 屬性來存取與當前使用者相關的數據。
這個屬性像用法像 Python 的字典一樣。
Django 會自動生成一個 Session ID 並存放在瀏覽器的 cookie 中。每次使用者發出請求時，伺服器都可以透過這個 ID 找到對應的會話數據。  

   可以讀取、寫入和修改刪除會話中的數據。
   ```python
   request.session['username'] = 'John Doe'
   username = request.session.get('username', 'Guest')  # 預設值為 'Guest'
   del request.session['username']
   request.session.set_expiry(300)  # 設定會話在 5 分鐘後過期

   ```
### 範例應用場景：
**購物車應用**：可以使用 Session 來儲存使用者購物車中的商品，當使用者在網站不同頁面之間瀏覽時，購物車的內容會保持不變。  
**使用者登入狀態**：Session 用於儲存登入狀態，當使用者登入後，系統可以追蹤其登入狀態，直到使用者登出或會話過期。


## 三、總結

這篇文章介紹了 Django 的 Session 框架，如何在多個 HTTP 請求間追蹤和保存使用者的數據。Session 透過生成唯一的 Session ID 存放在 Cookie 中，伺服器則根據這個 ID 存取相關數據。開發者可以像操作字典一樣讀取、寫入和刪除會話數據，常用於購物車、登入狀態等應用場景。在下一篇文章中我們將介紹 Middleware

## 四、參考資料
- https://developer.mozilla.org/zh-TW/docs/Learn/Server-side/Django/Sessions