# Day 6 - 介紹DRF以及REST API基礎
- 介紹DRF
- 介紹RESTful架構
- 使用Django REST framework創建API
- CRUD操作

## 一、什麼是DRF
DRF(Django REST framework) 是一個功能強大的工具包，專門為構建 Web API 而設計的套件。提供了一套簡潔且強大的工具來處理API的各個方面，如強大的序列化器、權限管理、通用 API 視圖。

DRF 的優點:
- 快速開發 API
- 強大的序列化器
- 內建認證和權限控制
- 客製化 API 視圖
- 自動生成 API 文檔

## 二、安裝 DRF
1. 安裝 DRF
    ```commandline
    poetry add djangorestframework
    ```
2. 將 rest_framework 添加到 Django 項目的 `INSTALLED_APPS` 中
    ```python
    INSTALLED_APPS = [
        ...
        'rest_framework',
    ]
    ```