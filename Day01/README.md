# Day 1 - 30天從0開始學習 Django 內容簡介

這個鐵人賽的文章會同步上傳到我的 [github](https://github.com/David20001110/2024-iTome) ，這裡的markdown格式好像有點不一樣如果看得不習慣也可以直接到 github 上看，有什麼想討論的可以留言或是寄email: david.yw.liu@gmail.com

## 一、簡介
嗨各位~~去年我有參加過一次鐵人賽，去年寫的內容是[30天從零開始學習NLP(自然語言處理)](https://ithelp.ithome.com.tw/users/20160436/ironman/6761)，是將我實習的內容寫成像是整理自己的所學，這次的鐵人賽主要是要是想分享 Django 的內容，想藉著現在工作時在使用這個框架這個機會整理自己學習的 Django 的知識，接下來 30 天會帶大家一起學習，請各位多多指教謝謝🙏🙏

> 今天先讓各位看一下這30天的主題及大綱，大綱內容可能會有所調整，但大致上會是這樣，大分類的話可以分為從基礎介紹和設置 -> 基本組件 -> 數據庫與模型 -> 進階功能和技術 > 安全性和測試 -> 實作與總結 。

## 二、日程主題及大綱
日程大致是以下可能會有一些內容之後在製作上會有一點點不一樣哈哈(┬┬﹏┬┬)

|     | 主題                               | 大綱                                                                                 |
|-----|----------------------------------|------------------------------------------------------------------------------------|
| 1.  | 30天從0開始學習 Django 內容簡介            | - 介紹30天標題與大綱                                                                       
| 2.  | 介紹甚麼是 Django、安裝、開發環境             | - 介紹 Django <br> - 開發IDE <br> - 下載pyhton、設置虛擬環境(使用poetry)、安裝django                 |
| 3.  | MTV 架構說明                         | - MTV架構說明 <br> - MTV 架構流程                                                          |
| 4.  | 開始創建 Django 專案                   | - 創建django專案 <br> - 項目結構說明 <br> - 設定資料庫                                            |
| 5.  | 建立第一個app以及簡易的API                 | - 介紹 app <br>- 建立、註冊app <br>- 建立簡易的 django API                                     |
| 6.  | 視圖（Views）                        | - 視圖和 URLS <br> - function based views <br>- 建立views <br>- 如何連接URL                 |
| 7.  | 模板（Templates）                    | - 什麼是 template <br> - 建立template <br>- 基礎語法 <br>- 連接views                          |
| 8.  | Templates 進階                     | - 模板繼承 <br>- 使用過濾器 <br> - 加載靜態文件 (配置路徑、 使用CSS美化模板) <br>- 模板繼承                      |
| 9.  | 模型（Models）                       | - 介紹ORM <br>- 建立 Models <br>- 字段類型和屬性                                              |
| 10. | 資料庫遷移、基本指令                       | - 資料遷移 ( 什麼是 makemigrations、什麼是 migrate ) <br>- 查看遷移狀況                             |
| 11. | 資料庫操作                            | - 使用shell command <br>- 創建和保存資料 <br>- 新增 修改 刪除                                     |
| 12. | 資料庫查詢                            | - 建立查詢集 <br>- 進階查詢                                                                 |
| 13. | 介紹DRF以及RESTful                   | - 介紹DRF <br>- 介紹 RESTful <br>- 使用Django REST framework創建API <br>- CRUD操作           |
| 14. | 序列化器 (Serializer)                | - 為什麼使用Serializer <br> - 創建自定義 Serializer <br>- 使用 ModelSerializer <br>- 序列化器的基本使用 |
| 15. | Serializer 進階          | - 自定義字段驗證 <br>- 嵌套的序列化 <br> - create 和 update 方法的實作                                |
| 16. | 信號（Signals）(把它擺在DRF後面)           | - signals的運作 <br> - 自定義信號器 <br>- 自定義信號器                                            |
| 17. | 管理站台 (Admins)                    | - 設置和使用Django管理後台 (創建superuser、註冊模型到管理站台、使用管理站台)                                   |
| 18. | 表單和模型表單（Forms and ModelForms）    | - Forms 的作用 <br>- 創建和使用 Forms <br>- 介紹*forms.ModelForm*類 <br>- 表單驗證                |
| 19. | Views 進階 - Class Based Views (CBVs) | - 為何要使用 CBVs <br>- 常用的Class-Based Views <br> - 創建一個 CBV                                    |
| 20. | Session framework                | - Sessiong是什麼、如何運作 <br>- 在Django中使用Session                                         |
| 21. | Middleware                       | - Middleware是什麼、如何運作 <br>- 如何編寫自定義 Middleware <br>- Middleware 的應用示例               |
| 22. | 理解 Django 認證：標準方法與 JWT 應用             | - 認證概述 <br>- JWT 認證介紹 <br>- 在 Django 中實施 JWT <br> - 安全實踐和策略                        |
| 23. | 用戶授權（Authorization）              | - 使用 Django 權限系統 <br>- 設置 User 和 Group 的權限                                         |
| 24. | 客製化權限控制 (Permission)             | - 自定義權限系統                                                                          |
| 25. | 生成API文檔（Swagger）                 | - 安裝和配置drf-yasg <br>- 設置URL路由 <br>- 生成Swagger文檔                                    |
| 26. | 介紹 Django 測試框架                   | -  測試框架介紹 <br>-  單元測試、集成測試 <br>-  自動化測試                                            |
| 27. | 實作簡易的 Django 商店網站 - 設置基本框架和功能    | - 設計模型<br>- 設置資料庫<br>- 建立基本Views和URL配置<br>- 設計基本的模板                                |
| 28. | 實作簡易的 Django 商店網站 - 加強功能和交互性     | - 增加用戶認證（Authentication）<br>- 購物車功能(編輯、刪除等)<br>- 美化模板和樣式調整                         |
| 29. | 實作簡易的 Django 商店網站 - 完善應用和部屬      | - 測試和調試<br>- 部屬                                                                    |
| 30. | 總結與回顧                            | - 總結                                                                               |

## 三、參考資料

- https://carolhsu.gitbooks.io/django-girls-tutorial-traditional-chiness/content/django/README.html