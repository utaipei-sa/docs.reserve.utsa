# 3.3 應用程式介面 APIs
使用者分為借用者與管理者，使用者可登記預約並管理自己登記的預約內容，管理者可管理所有場地及物品的預約資料。  

## 目錄 Table of Contents
- [3.3 應用程式介面 APIs](#33-應用程式介面-apis)
  - [目錄 Table of Contents](#目錄-table-of-contents)

### GET `/` : 指引訊息

### GET `/items` : 取得物品搜尋清單
- query parameters
  - item_id: 物品 _id  
    query, String, Optional  
    參考格式: /^[a-fA-F0-9]{24}$/  
    範例: 652765ed3d21844635674e71
  - name: 物品名稱  
    query, String, Optional
- response:
  - status: 200
    body: 
  - 
### GET `/spaces` : 取得場地搜尋清單
### POST `/reserve` : 新增預約資料
### GET `/reserve/:reservation_id` : 取得預約資料
- parameters:
  - reservation_id: 預約資料 _id
    path, String
    參考格式: /^[a-fA-F0-9]{24}$/  
    範例: 652765ed3d21844635674e71
- 

### PUT `/reserve/:reservation_id` : 更改預約資料
- parameters:
  - reservation_id: 預約資料 _id
    path, String
    參考格式: /^[a-fA-F0-9]{24}$/  
    範例: 652765ed3d21844635674e71
- 

### DELETE `/reserve/:reservation_id` : 刪除預約資料
- parameters:
  - reservation_id: 預約資料 _id
    path, String
    參考格式: /^[a-fA-F0-9]{24}$/  
    範例: 652765ed3d21844635674e71
- 

### GET `/item_available_time` : 查詢特定時段物品可預約數量
- parameters:
  - 
- 

### GET `/space_available_time` : 查詢特定時段場地是否可預約
