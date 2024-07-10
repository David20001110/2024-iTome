
## 一、簡介
嗨各位~~去年我有參加過一次鐵人賽，去年寫的內容是[30天從零開始學習NLP(自然語言處理)](https://ithelp.ithome.com.tw/users/20160436/ironman/6761)，是將我實習的內容寫成像是整理自己的所學，這的鐵人賽主要是要是想分享 Django 的內容，想藉著現在工作時在使用這個框架這個機會整理自己學習的 Django 的知識，接下來 30 天會帶大家一起學習，請各位多多指教謝謝🙏🙏

> 今天先讓各位看一下這30天的主題及大綱，大綱內容可能會有所調整，但大致上會是這樣的，以及介紹什麼是 Django。

## 二、日程主題及大綱
日程還沒完全想好哈哈(┬┬﹏┬┬)

|     | 主題                                  | 大綱                                                                                                      |
|-----|-------------------------------------|---------------------------------------------------------------------------------------------------------|
| 1.  | 30天從0開始學習 Django 內容簡介               | - 介紹30天標題與大綱                                                                                            
| 2.  | 介紹甚麼是 Django、安裝、開發環境                | - 開發IDE <br> - 安裝pyhton、django <br> - 設置虛擬環境(使用poetry)                                                  |
| 3.  | MTV 架構說明                            | - MTV架構說明                                                                                               |
| 4.  | 開始創建 Django 專案                      | - 創建django專案 <br> - 項目結構說明 <br> - 設定資料庫                                                                 |
| 5.  | 介紹DRF以及建立第一個app                     | - 什麼是DRF <br>- 建立、註冊app                                                                                 |
| 6.  | REST API基礎                          | - 介紹RESTful架構 <br>- 使用Django REST framework創建API <br>- CRUD操作                                           |
| 7.  | 視圖（Views）                           | - function based views <br>- 建立views <br>- 如何連接URL                                                      |
| 8.  | 模板（Templates）                       | - 建立template <br>- 基礎語法 <br>- 連接views <br>- 模板繼承                                                        |
| 9.  | Templates 進階                        | - 使用過濾器 <br>- 加載靜態文件 (配置路徑、 使用CSS美化模板)                                                                  |
| 10. | 模型（Models）                          | - 介紹ORM <br>- 建立 Models <br>- 字段類型和屬性                                                                   |
| 11. | 資料庫介紹、遷移、基本指令                       | - 資料遷移 ( 什麼是 makemigrations、什麼是 migrate ) <br>- 查看遷移狀況                                                  |
| 12. | 資料庫操作                               | - 使用shell command <br>- 創建和保存資料 <br>- 新增 修改 刪除                                                          |
| 13. | 資料庫查詢                               | - 建立查詢集 <br>- 進階查詢                                                                                      |
| 14. | 序列化器 (Serializer)                   | - 為什麼使用Serializer <br>- 什麼是Serializer <br>- 創建基本的Serializer <br>- 使用ModelSerializer <br>- 自定義Serializer |
| 15. | 信號（Signals）(把它擺在DRF後面)              | - signals的運作 <br>- 自定義 signals                                                                          |
| 16. | 管理站台 (Admins)                       | - 設置和使用Django管理後台 ( 創建superuser、註冊模型到管理站台 ) <br>- 自定義管理後台界面                                             |
| 17. | 表單和模型表單（Forms and ModelForms）       | - Forms 的作用 <br>- 創建和使用 Forms <br>- 介紹*forms.ModelForm*類 <br>- 表單驗證                                     |
| 18. | Views 進階 - Class Based Views (CBVs) | - 為何要使用 CBVs <br>- 常用的Class-Based Views                                                                 |
| 19. | Session framework                   | - Sessiong是什麼、如何運作 <br>- 在Django中使用Session                                                              |
| 20. | Middleware                          | - Middleware是什麼、如何運作 <br>- 如何編寫自定義 Middleware <br>- Middleware 的應用示例                                    |
| 21. | 用戶認證（Authentication）                | - 用戶註冊和登錄系統 <br>- 使用Django內置的用戶認證系統 <br>- 自定義用戶模型                                                       |
| 22. | JWT驗證                               | - JWT是什麼 <br>- 安裝和配置JWT <br>- 在Django中使用JWT進行身份驗證                                                       |
| 23. | 用戶授權（Authorization）                 | - 使用 Django 權限系統 <br>- 設置 User 和 Group 的權限                                                              |
| 24. | 客製化權限控制 (Permission)                | - 自定義權限系統                                                                                               |
| 25. | 生成API文檔（Swagger）                    | - 安裝和配置drf-yasg <br>- 設置URL路由 <br>- 生成Swagger文檔                                                         |
| 26. | 介紹 Django 測試框架                      | -  測試框架介紹 <br>-  單元測試、集成測試 <br>-  自動化測試                                                                 |
| 27. | 實作簡易的 Django 商店網站 - 設置基本框架和功能       | - 設計模型<br>- 設置資料庫<br>- 建立基本Views和URL配置<br>- 設計基本的模板                                                     |
| 28. | 實作簡易的 Django 商店網站 - 加強功能和交互性        | - 增加用戶認證（Authentication）<br>- 購物車功能(編輯、刪除等)<br>- 美化模板和樣式調整                                              |
| 29. | 實作簡易的 Django 商店網站 - 完善應用和部屬         | - 測試和調試<br>- 部屬                                                                                         |
| 30. | 總結與回顧                               | - 總結                                                                                                    |


## 三、什麼是 Django