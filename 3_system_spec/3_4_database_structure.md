# 3.4 資料庫結構 database struture
使用者分為借用者與管理者，使用者可登記預約並管理自己登記的預約內容，管理者可管理所有場地及物品的預約資料。  

## 目錄 Table of Contents
- [3.4 資料庫結構 database struture](#34-資料庫結構-database-struture)
  - [目錄 Table of Contents](#目錄-table-of-contents)
    - [items](#items)
    - [spaces](#spaces)
    - [reservations](#reservations)
    - [item\_reserved\_time](#item_reserved_time)
    - [space\_reserved\_time](#space_reserved_time)
    - [users](#users)
    - [system\_settings](#system_settings)

MongoDB

Database: utsa

Collections:

### items
- _id: ObjectId
- name: Object
  - zh-tw: String, 中文名稱
  - en: String, English name
- quantity: Int32, 開放預約數量
- item_codes: Array\<String\>, 物品編號/條碼
- exception_time: Array\<Object\>, 例外時段(不套用整體 quantity 規則)
  - start_time: Date, 開始時間
  - end_time: Date, 結束時間
  - quantity: Int32, 時段開放預約數量
### spaces
- _id: ObjectId
- name: Object
  - zh-tw: String, 中文名稱
  - en: String, English name
- open: Int32, 是否開放預約(整體規則), 1: 開放預約; 0: 不開放預約
- exception_time: Array\<Object\>, 例外時段(不套用整體 open 規則)
  - start_time: Date, 開始時間
  - end_time: Date, 結束時間
  - open: Int32, 時段是否開放預約, 1: 開放預約; 0: 不開放預約
### reservations
  - _id: ObjectId
  - status: Int32, wait_for_verify/new/update
  - history: Array\<Object\>
    - submit_timestamp: Date
    - server_timestamp: Timestamp
    - type: Int32, 同 status
  - user_id: ObjectId, 系統使用者 _id
  - organization: String, 預約單位(如某某社團)
  - name: String, 預約人姓名
  - department_grade: String, 預約人系級
  - email: String, 預約負責人聯絡用電子信箱
  - verify_code: String, 匿名使用者預約驗證碼
  - usage: String, 用途
  - notes: String, 備註
  - verified: Int32, 是否已預約, 1: 是; 0: 否
  - item_reservations: Array\<Object\>, 物品預約時段資料
    - item_id: ObjectId, 物品 _id
    - start_datetime: Date, 預約時段開始時間
    - end_datetime: Date, 預約時段結束時間
    - quantity: Int32, 時段借用數量
  - space_reservations: Array\<Object\>, 場地預約時段資料
    - space_id: ObjectId, 場地 _id
    - start_datetime: Date, 預約時段開始時間
    - end_datetime: Date, 預約時段結束時間
### item_reserved_time
- _id: ObjectId
- start_datetime: Date, 時段開始時間
- end_datetime: Date, 時段結束時間
- item_id: ObjectId, 物品 _id
- reserved_quantity: Int32, 時段被預約數量
- reservations: Array\<ObjectId\>, 預約紀錄 _id
### space_reserved_time
- _id: ObjectId
- start_datetime: Date, 時段開始時間
- end_datetime: Date, 時段結束時間
- space_id: ObjectId, Int32, 場地 _id
- reserved: 此時段是否被預約
- reservations: Array\<ObjectId\>, 預約紀錄 _id
### users
- _id: ObjectId
- name: String, 姓名
- email: String, 電子郵件
- email_verified: Int32, 電子郵件是否完成驗證, 1: True; 2: False
- organizations: Array\<String\>, 所加入的組織(社團、科系、行政部門等)
- reservations: Array\<ObjectId\>, 預約紀錄 _id
- passkeys: Array\<Object\>, 通行密鑰
    - name: String, 名稱
    - passkey: String, 通行密鑰公鑰
- password_hash: String, 密碼雜湊值
- otp_key: String, 生成一次性驗證碼所用的 secret key, 可用於生成 time-based OTP 和 counter-based OTP
- otp_count: Int32, counter-based OTP 次數紀錄
### system_settings
- item_code_required: Int32, 物品編號必填, 1: Enable; 0: Disable
- verify_before_reserve: Int32, 預約表單送出前需通過驗證, 1: Enable; 0: Disable
- anonymous_user_reserve: Int32, 開放匿名使用者預約, 1: Enable; 0: Disable