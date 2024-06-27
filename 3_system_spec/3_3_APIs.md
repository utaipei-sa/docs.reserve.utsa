# 3.3 應用程式介面 APIs
使用者分為借用者與管理者，使用者可登記預約並管理自己登記的預約內容，管理者可管理所有場地及物品的預約資料。  

## 目錄 Table of Contents
- [3.3 應用程式介面 APIs](#33-應用程式介面-apis)
  - [目錄 Table of Contents](#目錄-table-of-contents)

### GET `/` : 指引訊息
- response
  - status: 200 OK  
    content: application/json  
    ```json
    {
      "title": "UTSA reservation system", 
      "hello": "world"
    }
    ```

### GET `/items` : 取得物品搜尋清單
- parameters  
  - `item_id` (query)  
    物品 _id  
    string, optional  
    format: Object ID `/^[a-fA-F0-9]{24}$/`  
    example: "65f48bbc6f21a65d302a6147"  
- response
  - status: 200 OK  
    content: application/json  

    if item_id was given, it'll return an object. Otherwise, it'll return an array.

    if item_id === undefined: 
    - `data`: Array\<Object\>
      - `_id`: Object ID, 物品 _id
      - `name`: Object, 物品名稱
        - `zh-tw`: string, 物品中文名稱
        - `en`: string, item English name
      - `quantity`: integer, 物品數量
      - `exception_time`: Array\<Object\>
        - ......

    if item_id !== undefined: 
    - `_id`: Object ID, 物品 _id
    - `name`: Object, 物品名稱
      - `zh-tw`: string, 物品中文名稱
      - `en`: string, item English name
    - `quantity`: integer, 物品數量
    - `exception_time`: Array\<Object\>
      - ......

    example: 
    item_id: undefined
    ```json
    {
      "data": [
        {
          "_id": "65f48bbc6f21a65d302a6147",
          "name": {
            "zh-tw": "塑膠椅",
            "en": "Plastic Chairs"
          },
          "quantity": 30,
          "exception_time": []
        },
        {
          "_id": "65f48bbc6f21a65d302a6148",
          "name": {
            "zh-tw": "長桌",
            "en": "Tables"
          },
          "quantity": 2,
          "exception_time": []
        }
      ]
    }
    ```

    item_id: "65f48bbc6f21a65d302a6147"
    ```json
    {
      {
        "_id": "65f48bbc6f21a65d302a6147",
        "name": {
          "zh-tw": "塑膠椅",
          "en": "Plastic Chairs"
        },
        "quantity": 30,
        "exception_time": []
      }
    }
    ```

  - status: 404 Not found
    content: application/json
    - `error_code`: string, 錯誤代碼
    - `message`: string, 提示訊息

    example:
    ```json
    {
      "error_code": "R_ID_NOT_FOUND", 
      "message": "Item ID not found"
    }
    ```

### GET `/spaces` : 取得場地搜尋清單
- parameters  
  - `space_id` (query)  
    場地 _id  
    string, optional  
    format: Object ID `/^[a-fA-F0-9]{24}$/`  
    example: "65f48bbc6f21a65d302a6149"  
- response
  - status: 200 OK  
    content: application/json  

    if space_id was given, it'll return an object. Otherwise, it'll return an array.

    if space_id === undefined: 
    - `data`: Array\<Object\>
      - `_id`: string, 場地 _id
      - `name`: Object, 場地名稱
        - `zh-tw`: string, 場地中文名稱
        - `en`: string, space English name
      - `open`: integer, 是否開放預約, 1: 可預約, 0: 不可預約
      - `exception_time`: Array\<Object\>
        - ......

    if space_id !== undefined: 
    - `_id`: string, 場地 _id
    - `name`: Object, 場地名稱
      - `zh-tw`: string, 場地中文名稱
      - `en`: string, space English name
    - `open`: integer, 是否開放預約, 1: 可預約, 0: 不可預約
    - `exception_time`: Array\<Object\>
      - ......

    example: 
    space_id: undefined
    ```json
    {
      "data": [
        {
          "_id": "65f48bbc6f21a65d302a6149",
          "name": {
            "zh-tw": "學生活動中心",
            "en": "Student Activity Center"
          },
          "open": 1,
          "exception_time": []
        },
        {
          "_id": "65f48bbc6f21a65d302a614a",
          "name": {
            "zh-tw": "勤樸樓B1小舞台",
            "en": "Cin-Pu Building B1 Stage"
          },
          "open": 1,
          "exception_time": []
        }
      ]
    }
    ```

    space_id: "65f48bbc6f21a65d302a6149"
    ```json
    {
      {
        "_id": "65f48bbc6f21a65d302a6149",
        "name": {
          "zh-tw": "學生活動中心",
          "en": "Student Activity Center"
        },
        "open": 1,
        "exception_time": []
      }
    }
    ```

  - status: 404 Not found
    content: application/json
    - `error_code`: string, 錯誤代碼
    - `message`: string, 提示訊息

    example:
    ```json
    {
      "error_code": "R_ID_NOT_FOUND", 
      "message": "Space ID not found"
    }
    ```

### POST `/reserve` : 新增預約資料
- parameters  
  - reservation (body)  
    新增預約紀錄  
    required  
    content: application/json  
    schema:  
    - `submit_datetime`: 用戶端表單送出時間  
      string, optional  
      format: ISO date-time string `/^(\d{4})-(\d{2})-(\d{2})T(\d{2}):(\d{2}):(\d{2})(\.\d*)?\+08:?00$/`  
      example: "2024-04-26T14:06:23.286+0800" 
    - `name`: 登記人姓名  
      string, required  
    - `department_grade`: 系級  
      string, required
    - `organization`: 借用單位(社團/單位等)  
      string, required
    - `email`: 聯絡用電子信箱  
      string, required  
      format: email `/^[\w-.\+]+@([\w-]+\.)+[\w-]{2,4}$/`  
      example: "u11116000@go.utaipei.edu.tw"
    - `reason`: 借用原因/用途  
      string, required
    - `note`: 備註  
      string, optional
    - `space_reservations`: 空間預約資料  
      array\<object\>, required ( `Union(space_reservations, item_reservations)` )
      - `space_id`: 場地 _id  
        string, required  
        format: Object ID `/^[a-fA-F0-9]{24}$/`  
        example: "65f48bbc6f21a65d302a6149" 
      - `start_datetime`: 預約時段開始時間  
        string, required  
        format: `/^(\d{4})-(\d{2})-(\d{2})T(\d{2}):(\d{2})$/`  
        example: "2024-04-29T13:00"
      - `end_datetime`: 預約時段結束時間
        string, required  
        format: `/^(\d{4})-(\d{2})-(\d{2})T(\d{2}):(\d{2})$/`  
        example: "2024-04-29T17:00"
    - `item_reservations`: 場地預約資料  
      array\<Object\>, required ( `Union(space_reservations, item_reservations)` )
      - `item_id`: 物品 _id  
        string, required  
        format: Object ID `/^[a-fA-F0-9]{24}$/`  
        example: "652038af1b2271aa002c0a09"
      - `item_code`: 物品編號  
        string, optional (required if the admin set to required)
      - `start_datetime`: 預約時段開始時間  
        string, required  
        format: `/^(\d{4})-(\d{2})-(\d{2})T(\d{2}):(\d{2})$/`  
        example: "2024-04-29T13:00"
      - `end_datetime`: 預約時段結束時間
        string, required  
        format: `/^(\d{4})-(\d{2})-(\d{2})T(\d{2}):(\d{2})$/`  
        example: "2024-04-29T17:00"
      - `quantity`: 預約物品數量  
        integer, required  
        example: 15
    
    example: 
    ```json
    {
      "submit_datetime": "2024-04-26T14:31:23.486+0800",
      "name": "王小名",
      "department_grade": "資科二",
      "organization": "學生會",
      "email": "u123456789@go.utaipei.edu.tw",
      "reason": "舉辦迎新大會",
      "space_reservations": [
        {
          "space_id": "65253db7053c98f0bd1593db",
          "start_datetime": "2024-01-23T18:00",
          "end_datetime": "2024-01-25T22:00"
        }
      ],
      "item_reservations": [
        {
          "item_id": "652038af1b2271aa002c0a09",
          "item_code": "ABC-123",
          "start_datetime": "2024-01-23T18:00",
          "end_datetime": "2024-01-25T22:00",
          "quantity": 15
        }
      ],
      "note": "無"
    }
    ```
- response
  - status: 200 OK  
    content: application/json  
    ```json
    {
      "code": "R_SUCCESS", 
      "message": "Success!"
    }
    ```
  - status: 404 Not Found  
    content: application/json
    - `error_code`: string, 錯誤代碼
    - `message`: string, 提示訊息

    example:
    ```json
    {
      "error_code": "R_ID_NOT_FOUND",
      "message": "Space ID not found"
    }
    ```

### GET `/reserve/{reservation_id}` : 取得預約資料
- parameters  
  - `reservation_id` (path)  
    預約紀錄 ID  
    string, required   
    format: Object ID `/^[a-fA-F0-9]{24}$/`  
    example: "652038af1b2271aa002c0a09"
- response
  - status: 200 OK  
    content: application/json  
    schema:
    - `submit_datetime`: string, 用戶端表單送出時間  
      format: ISO date-time string
    - `server_datetime`: string, 伺服器端資料建立時間  
      format: ISO date-time string
    - `verified`: integer, 是否完成驗證，1:是 0:否
    - `name`: string, 登記人姓名  
    - `department_grade`: string, 系級  
    - `organization`: string, 借用單位(社團/單位等)  
    - `email`: string, 聯絡用電子信箱   
      format: email `/^[\w-.\+]+@([\w-]+\.)+[\w-]{2,4}$/`  
    - `reason`: string, 借用原因/用途  
    - `note`: string, 備註  
    - `space_reservations`: array\<object\>, 空間預約資料  
      - `space_id`: string, 場地 _id  
        format: Object ID `/^[a-fA-F0-9]{24}$/`  
      - `start_datetime`: string, 預約時段開始時間  
        format: `/^(\d{4})-(\d{2})-(\d{2})T(\d{2}):(\d{2})$/`  
      - `end_datetime`: string, 預約時段結束時間  
        format: `/^(\d{4})-(\d{2})-(\d{2})T(\d{2}):(\d{2})$/`  
    - `item_reservations`: array\<Object\>, 場地預約資料  
      - `item_id`: string, 物品 _id  
        format: Object ID `/^[a-fA-F0-9]{24}$/`  
      - `item_code`: string, 物品編號  
      - `start_datetime`: string, 預約時段開始時間  
        format: `/^(\d{4})-(\d{2})-(\d{2})T(\d{2}):(\d{2})$/`  
      - `end_datetime`: string, 預約時段結束時間
        format: `/^(\d{4})-(\d{2})-(\d{2})T(\d{2}):(\d{2})$/`  
      - `quantity`: integer, 預約物品數量  

    example:
    ```json
    {
      "submit_datetime": "2024-04-26T14:36:24.593+0800",
      "server_datetime": "2024-04-26T14:36:25.286Z",
      "name": "王小名",
      "department_grade": "資科二",
      "organization": "學生會",
      "email": "u123456789@go.utaipei.edu.tw",
      "reason": "舉辦迎新大會",
      "note": "無",
      "space_reservations": [
        {
          "space_id": "65253db7053c98f0bd1593db",
          "start_datetime": "2024-01-23T18:00",
          "end_datetime": "2024-01-25T22:00"
        }
      ],
      "item_reservations": [
        {
          "item_id": "652038af1b2271aa002c0a09",
          "item_code": "ABC-123",
          "start_datetime": "2024-01-23T18:00",
          "end_datetime": "2024-01-25T22:00",
          "quantity": 15
        }
      ]
    }
    ```
  - status: 404 Not found  
    content: application/json  
    - `error_code`: string, 錯誤代碼
    - `message`: string, 提示訊息

    example:
    ```json
    {
      "error_code": "R_ID_NOT_FOUND", 
      "message": "Reservation ID not found"
    }
    ```

### PUT `/reserve/{reservation_id}` : 更改預約資料
- parameters  
  - `reservation_id` (path)  
    預約紀錄 ID  
    string, required  
    format: Object ID `/^[a-fA-F0-9]{24}$/`  
    example: "652038af1b2271aa002c0a09"
  - request body (body)  
    更新的預約紀錄  
    required  
    content: application/json  
    schema:  
    - (Almost the same as GET /reserve)
- response
  - status: 200 OK  
    content: application/json  

    example: 
    ```json
    {
      "title": "UTSA reservation system", 
      "hello": "world"
    }
    ```

  - status: 404 Not found
    content: application/json
    - `error_code`: string, 錯誤代碼
    - `message`: string, 提示訊息

    example:
    ```json
    {
      "error_code": "R_ID_NOT_FOUND", 
      "message": "Reservation ID not found"
    }
    ```

### DELETE `/reserve/{reservation_id}` : 刪除預約資料
- parameters  
  - `reservation_id` (path)  
    預約紀錄 ID  
    string, required  
    format: Object ID `/^[a-fA-F0-9]{24}$/`  
    example: "652038af1b2271aa002c0a09"
- response
  - status: 200 OK  
    content: application/json  
    
    If the server found the reservation data and delete it, the following content will be returned:
    ```json
    {
      "code": "R_SUCCESS",
      "message": "Delete success!"
    }
    ```

    If the reservation data not exist, the following content will be returned:
    ```json
    {
      "code": "R_ID_NOT_FOUND",
      "message": "Reservation ID not found"
    }
    ```

### GET `/item_available_time` : 查詢特定時段物品可預約數量
- parameters  
  - `intervals` (query)  
    是否切分成各時段進行回傳  
    string, optional(false by dafault)  
    format: true/false
    example: false
  - `item_id` (query)  
    物品 _id  
    string, required  
    format: Object ID `/^[a-fA-F0-9]{24}$/`  
    example: "652038af1b2271aa002c0a09"
  - `start_datetime` (query)  
    查詢起始時間  
    string, optional  
    format: `/^(\d{4})-(\d{2})-(\d{2})T(\d{2}):(\d{2})$/`  
    example: "2024-04-29T13:00"  
  - `end_datetime` (query) 查詢結束時間  
    string, optional  
    format: `/^(\d{4})-(\d{2})-(\d{2})T(\d{2}):(\d{2})$/`  
    example: "2024-04-29T17:00"  
- response
  - status: 200 OK  
    content: application/json  

    if intervals is `false`:  
    - `available_quantity`: integer, 可預約數量 

    example:
    ```json
    {
      "available_quantity": 5
    }
    ```

    if intervals is `true`:  
    - `intervals`: Array\<Object\>, 各時段可否預約資料
      - `start_datetime`: string, 時段開始時間  
        format: `/^(\d{4})-(\d{2})-(\d{2})T(\d{2}):(\d{2})$/`
      - `end_datetime`: string, 時段結束時間  
        format: `/^(\d{4})-(\d{2})-(\d{2})T(\d{2}):(\d{2})$/`
      - `available_quantity`: integer, 可預約數量

    example:
    ```json
    {
      "intervals": [
        {
          "start_datetime": "2024-05-01T13:00",
          "end_datetime": "2024-05-02T12:00",
          "available_quantity": 20
        },
        {
          "start_datetime": "2024-05-02T13:00",
          "end_datetime": "2024-05-03T12:00",
          "available_quantity": 5
        },
      ]
    }
    ```
  - status: 404 Not found
    content: application/json
    - `error_code`: string, 錯誤代碼
    - `message`: string, 提示訊息

    example:
    ```json
    {
      "error_code": "R_ID_NOT_FOUND", 
      "message": "Item ID not found"
    }
    ```

### GET `/space_available_time` : 查詢特定時段場地是否可預約
- parameters  
  - `intervals` (query)  
    是否切分成各時段進行回傳  
    string, optional(false by dafault)  
    format: true/false
    example: false
  - `space_id` (query)  
    場地 _id  
    string, required  
    format: Object ID `/^[a-fA-F0-9]{24}$/`  
    example: "652038af1b2271aa002c0a09"
  - `start_datetime` (query)  
    查詢起始時間  
    string, optional  
    format: `/^(\d{4})-(\d{2})-(\d{2})T(\d{2}):(\d{2})$/`  
    example: "2024-04-29T13:00"  
  - `end_datetime` (query) 查詢結束時間  
    string, optional  
    format: `/^(\d{4})-(\d{2})-(\d{2})T(\d{2}):(\d{2})$/`  
    example: "2024-04-29T17:00"  
- response
  - status: 200 OK  
    content: application/json  

    if intervals is `false`:  
    - `available`: integer, 是否可預約, 1:可 0:不可 

    example:
    ```json
    {
      "available": 0
    }
    ```

    if intervals is `true`:  
    - `intervals`: Array\<Object\>, 各時段可否預約資料
      - `start_datetime`: string, 時段開始時間  
        format: `/^(\d{4})-(\d{2})-(\d{2})T(\d{2}):(\d{2})$/`
      - `end_datetime`: string, 時段結束時間  
        format: `/^(\d{4})-(\d{2})-(\d{2})T(\d{2}):(\d{2})$/`
      - `available`: integer, 是否可預約, 1:可 0:不可

    example:
    ```json
    {
      "intervals": [
        {
          "start_datetime": "2024-05-02T13:00",
          "end_datetime": "2024-05-02T17:00",
          "available": 0
        },
        {
          "start_datetime": "2024-05-02T18:00",
          "end_datetime": "2024-05-02T22:00",
          "available": 1
        },
      ]
    }
    ```
  - status: 404 Not found
    content: application/json
    - `error_code`: string, 錯誤代碼
    - `message`: string, 提示訊息

    example:
    ```json
    {
      "error_code": "R_ID_NOT_FOUND", 
      "message": "Space ID not found"
    }
    ```

### POST `/verify/{reservation_id}` : 進行預約驗證
- parameters
  - `reservation_id` (path)  
    預約紀錄 _id  
    string, required  
    format: Object ID `/^[a-fA-F0-9]{24}$/`  
    example: "652038af1b2271aa002c0a09"
- response
  - status: 200 OK  
    content: application/json  
    - `message`: string, 提示訊息

    example:
    ```json
    {
      "message": "success!"
    }
    ```

  - status: 404 Not found
    content: application/json
    - `error_code`: string, 錯誤代碼
    - `message`: string, 提示訊息

    example:
    ```json
    {
      "error_code": "R_ID_NOT_FOUND", 
      "message": "Reservation ID not found"
    }
    ```
