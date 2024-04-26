# 2.1 功能性需求 Functional Requirements
使用者分為借用者與管理者，使用者可登記預約並管理自己登記的預約內容，管理者可管理所有場地及物品的預約資料。  

## 目錄 Table of Contents
- [2.1 功能性需求 Functional Needs](#21-功能性需求-functional-needs)
  - [目錄 Table of Contents](#目錄-table-of-contents)
  - [2.1.1 預約登記](#211-預約登記)
  - [2.1.2 預約登記取消或變更](#212-預約登記取消或變更)
  - [2.1.3 預約情況查詢](#213-預約情況查詢)
  - [2.1.4 帳戶建立與驗證](#214-帳戶建立與驗證)
  - [2.1.5 預約登記管理](#215-預約登記管理)
  - [2.1.6 系統設定](#216-系統設定)

---

使用者分為借用者與管理者，使用者可登記預約並管理自己登記的預約內容，管理者可管理所有場地及物品的預約資料。

### 2.1.1 預約登記 Reservation

使用者借用時需填寫表單。  

場地借用表單（* 代表必填）：  
Space reservation form (* means required):

- 姓名*  
  Name*
- 申請單位*（預填「個人」）  
  d* (pre-fill "individual")
- 申請人系級*  
  t
- 電子信箱*  
  Email*
- 借用原因/用途*  
  t*
- 場地借用資訊*（可加入多筆資料）
  - 場地別*
  - 預約時間範圍*（起始～結束，以時段為單位）
- 備註  
  Notes

物品借用表單（`*` 代表必填）：  
Item reservation form (`*` means required):

- 姓名*  
  Name*
- 申請單位*（預填「個人」）  
  d* (pre-fill "individual")
- 申請人系級  
  t
- 電子信箱*  
  Email*
- 借用原因/用途*  
  t*
- 物品借用資訊*（可加入多筆資料）  
  Item reservation information* ()
  - 物品種類*  
    Item category
  - 物品編號
    Item code
  - 借用數量*
    Reserve quantity
  - 借用時間範圍*（起始～結束，以時段為單位）
- 備註  
  Notes

場地可供預約的時段如下：  
Below are the time slots of which spaces are available for reservation:

- 08:00~12:00
- 13:00~17:00
- 18:00~22:00

物品可供預約的時段如下：  
Below are the time slots of which items are available for reservation:

- 12:01~12:00(d+1)

填寫表單後，寄送電子郵件進行通知或驗證：若需要驗證且尚未驗證，則寄送驗證信，驗證通過才算預約成功；若無需驗證或驗證完成，則直接完成預約。流程圖如下所示：

![Flow chart After submission of the reservation form](/docs/images/2.1.1.1.drawio.png)

### 2.1.2 預約登記取消或變更

### 2.1.3 預約情況查詢

### 2.1.4 帳戶建立與驗證

### 2.1.5 預約登記管理

### 2.1.6 系統設定

