# 🧭 AMR ⇄ MES HTTPS 通訊規格書

## 一、文件目的

本文件說明自走式搬運機器人（AMR）與製造執行系統（MES）之間透過 **HTTPS 通訊** 進行任務派發、回報與例外處理的協定規格，  
並附上實際的時序圖，供軟體與設備工程師對接開發使用。

---

## 二、通訊架構概述

- **協定**：HTTPS / RESTful API  
- **資料格式**：JSON UTF-8  
- **安全機制**：TLS 1.3 加密 + JWT 驗證  
- **連線方式**：AMR 主動呼叫 MES API，MES 回傳同步回應  
- **通訊方向**：
  - AMR → MES：請求任務、回報進度、異常上報  
  - MES → AMR：Webhook 通知、任務取消、Token 更新  

---

## 三、通訊端點與認證

### 1️⃣ API 端點範例
| 類別 | URL | 方法 |
|------|------|------|
| 任務請求 | `https://mes.company.com/api/v1/task/request` | POST |
| 任務回報 | `https://mes.company.com/api/v1/task/report` | POST |
| 任務取消 | `https://mes.company.com/api/v1/task/cancel` | POST |
| Token 申請 | `https://mes.company.com/api/v1/auth/token` | POST |

### 2️⃣ 認證方式
AMR 於每次呼叫 API 時，需於 HTTP Header 內附上 Token：
