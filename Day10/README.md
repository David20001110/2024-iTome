# Day 11 - 資料庫遷移、基本指令

- 資料遷移
   - 什麼是 makemigrations
   - 什麼是 migrate
- 查看遷移狀況

## 一、資料遷移

資料遷移是將 app 或是模型（Model）中的變更應用到資料庫的過程。這包括創建表、修改表結構、刪除表等操作。
Django 的遷移系統自動生成和應用這些變更，讓開發者不需要手動編寫 SQL 語句，
它實際上是一個Python的檔案，可以同步Models.py裡的類別以及資料庫，因此只要有更動 Model 就需要去執行遷移的動作，主要會去執行的動作兩個一個是 makemigrations 一個是 migrate。

### 1. 什麼是 makemigrations
- `makemigrations` 是一個用於將模型更改的遷移文件生成到專案中的命令。
- 當我們執行  `makemigrations` 命令對模型進行更改時，Django 會自動檢測到這些更改，並生成一個 python 的遷移文件，這個遷移文件包含了對模型的更改操作。

### 2. 什麼是 migrate
- `migrate` 是一個用於將遷移文件應用到資料庫的命令。
- 當我們執行 `migrate` 命令時，Django 會將遷移文件應用到資料庫，這樣我們的資料庫就會根據模型的更改進行更新。
- `migrate` 命令會自動檢測專案中的遷移文件，並將這些遷移文件應用到資料庫中，使資料庫和模型保持同步。

### 實際操作
我們延續昨天建立的 `UserProfile` model 去進行遷移的操作


```commandline
python manage.py makemigrations
```
![img_1.png](https://github.com/David20001110/2024-iTome/blob/master/Day11/img_1.png?raw=true)
- 執行完後就會看到 Create model UserProfile 的訊息，代表成功建立遷移檔案

```commandline
python manage.py migrate
```
![img_2.png](https://github.com/David20001110/2024-iTome/blob/master/Day11/img_2.png?raw=true)
- 執行完後就會看到 Applying myapp.0001_initial... OK 的訊息，代表成功將遷移檔案應用到資料庫中 _(因為這裡我是先建立又刪除再建立所以會顯示0003)_


## 二、查看遷移狀況
我們可以使用以下指令來查看遷移的狀況，查看哪些遷移已經被應用
```commandline
python manage.py showmigrations
```
![img_3.png](https://github.com/David20001110/2024-iTome/blob/master/Day11/img_3.png?raw=true)

> 這邊補充一下我是使用 pycharm 專業版內建的 Database 工具來查看資料庫的資料，可以直接在IDE中查看資料庫的資料，非常方便。
> 也可以使用一些第三方的工具來查看資料庫的資料，例如：`DBeaver`、`Navicat`、`HeidiSQL`等等。

## 三、總結
今天我們學習了什麼是資料遷移以及如何進行資料遷移。下一篇文章我們將會學習如何進行資料的增刪改查。

## 四、參考資料
- https://ithelp.ithome.com.tw/articles/10298464