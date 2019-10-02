# BlueLion APIs
---

| ID | Date | Author | Content |
| ------ | ------ | ------ |------ |
| 1 | 26/09/2019 | Shan Gao | 1st draft |
---
## 1. 文档概述
| Prerequisite Param | Value | Description |
| ------ | ------ |------ |
| request_source_index| bluelion | |
|cryptos_key| 8wvu4JNclrnVYfaw |测试环境加密key |
|sign_key| evzAJZolMVdcb1kVI81ymNbPqWByxuhS| 测试环境签名key |
|app_id | 0ce9fabd44f7c819001f17f1b788f6bd |测试环境app_id |

所有接口请求需要先到Auth API进行授权，获取access_token

### 1.1 接口概述
| API Type | URL |Description |
| ------ | ------ | ------ |
| Auth API | http://{host}/auth/*   | Get AccessToken before requesting other APIs |
| User APIs | http://{host}/user/* | |
| Printer APIs | http://{host}/print/* |  |
| Order APIs | http://{host}/order/* | |
| Content APIs | http://{host}/content/* | |

### 1.2 协议概述
| Item | Description |
| ------ | ------ |
| 返回格式 | JSON | 
| 编码格式 | UTF-8 | 
| 请求方式 | POST | 
| 返回格式 | { "code":"1000", "msg":"", "data":""}| 

### 1.3 Public Params
```
{
    "params": {
        "device_info": { //设备信息
            "mac":"",  //mac 地址 type: String
            "imei":"", //设备号 type: String
            "imsi":"",  //sim卡号，type: String
            "os": 1, //操作系统， ANDROID：1， IOS: 2 type: int
        },
        "other_info": {
            "app_id":"xxx", // See above
        }
    },
    "sign":"xxxx",  // 签名算法见1.4.2
    "version": "1.0", //客户端当前版本
    "request_source_index": "xxx" //see above
}
```

### 1.4 Encryption and Sign
#### 1.4.1 Encryption
默认算法：AES128加密，填充类型为ECB/PKCS5Padding，加密后的数据做BASE64编码。
注：在接收到支付SDK接口返回的报文后，也需要按照加密的逆向方式进行解密

#### 1.4.2 Sign
服务器的签名算法为MD5，需要用上面未加密的Params JSON请求参数 + 定义好的签名KEY做MD5的hash；MD5的最终结果采用32位的小写为统一标准；

### 2 Authorize APIs
#### 2.1 Auth API
##### 2.1.2 Url
---
| Url | Method |
| ------ | ------ |
| /access_token | POST |

##### 3.1.3 Response
---
```
{
    "code": "1000",
    "msg":"",
    "data":{
        "access_token": "xxxxx"
    }
}
```
### 3 User APIs
#### 3.1 Login API
##### 3.1.2 Url
---
| Url | Method |
| ------ | ------ |
| /login | POST |
##### 3.1.2 Params
---
| Params | Description | isRequired |
| ------ | ------ |------ |
| account | User's email | Yes |
| password |  | Yes |
##### 3.1.3 Request
---
```
{
    params: {
        // public params
        "account": "xxx",
        "password": "xxx",
        "access_token":"" //Access Token from Auth Server
    }
}
```
##### 3.1.4 Response
---
```
{
    "code": "1000",
    "msg":"",
    "data":{
        "user_id": 1, 
        "user_name": "xxx",
        "email": "xxx@gmail.com",
        "token":""  //type: String
    }
}
```
#### 3.2 Register API
##### 3.2.1 Url
---
| Url | Method |
| ------ | ------ |
| /register | POST |
##### 3.2.2 Params
---
| Params | Description | isRequired |
| ------ | ------ |------ |
| account | Email | Yes |
| full_name |  | Yes |
| phone |  | No |
| password |  | Yes |
| access_token |  | Yes |
##### 3.2.3 Request
---
```
{
    params: {
        "account": "xxx",
        "full_name": "xxx",
        "phone": "xxx",
        "password": "xxx",
        "access_token":"" //Access Token from Auth Server
    }
}
```
##### 3.2.4 Response
---
```
{
    "code": "1000",
    "msg":"",
    "data":{
        "user_id": 1, 
        "user_name": "xxx",
        "email": "xxx@gmail.com",
        "token":""  //type: String
    }
}
```
#### 3.3 Logout API
##### 3.3.1 Url
---
| Url | Method |
| ------ | ------ |
| /logout | POST |
##### 3.3.2 Params
---
| Params | Description | isRequired |
| ------ | ------ |------ |
| account |  | Yes |
| token |  | Yes |
| access_token |  | Yes |
##### 3.3.3 Request
---
```
{
    params: {
        "account": "xxx",
        "token": "xxx",
        "access_token":"" //Access Token from Auth Server
    }
}
```
##### 3.3.4 Response
---
```
{
    "code": "1000",
    "msg":"",
    "data":{
    }
}
```
#### 3.4 Send Code API
##### 3.4.1 Url
---
| Url | Method |
| ------ | ------ |
| /send_code | POST |
##### 3.4.2 Params
---
| Params | Description | isRequired |
| ------ | ------ |------ |
| email |  | Yes |
| type | 1: forget password 2: modify password | Yes |
| account |  | No |
| token |  | No |
| access_token | | Yes |

##### 3.4.3 Request
---
```
{
    params: {
        "email": "xxx",
        "type": 1,   //type: int
        "account": "xxx",
        "token": "xxx",  // It is required only when modifing the password
        "access_token":"", //Access Token from Auth Server
    }
}
```
##### 3.4.4 Response
---
```
{
    "code": "1000",
    "msg":"",
    "data":{
    }
}
```
#### 3.5 Modify Password API
##### 3.5.1 Url
---
| Url | Method |
| ------ | ------ |
| /password/modify | POST |
##### 3.5.2 Params
---
| Params | Description | isRequired |
| ------ | ------ |------ |
| account |  | Yes |
| old_password |  | Yes |
| new_password |  | Yes |
| validation_code | Validation code will be sent to the user's email | Yes |
| token |  | Yes |
##### 3.5.3 Request
---
```
{
    params: {
        "account": "xxx",
        "old_password": "xxx",
        "new_password": "xxx",
        "access_token":"", //Access Token from Auth Server
        "token": "",
    }
}
```
##### 3.5.4 Response
---
```
{
    "code": "1000",
    "msg":"",
    "data":{
    }
}
```
#### 3.6 Forget Password API
##### 3.6.1 Url
---
| Url | Method |
| ------ | ------ |
| /password/forget | POST |
##### 3.6.2 Params
---
| Params | Description | isRequired |
| ------ | ------ |------ |
| email |  | Yes |
| new_password |  | Yes |
| validation_code | Validation code will be sent to the user's email | Yes |
| access_token |  | Yes |
##### 3.6.3 Request
---
```
{
    params: {
        "account": "xxx",
        "new_password": "xxx",
        "access_token":"", //Access Token from Auth Server
        "validation_code": "",
    }
}
```
##### 3.6.4 Response
---
```
{
    "code": "1000",
    "msg":"",
    "data":{
    }
}
```

#### 3.7 User Detail API
##### 3.7.1 Url
---
| Url | Method |
| ------ | ------ |
| /detail | POST |
##### 3.7.2 Params
---
| Params | Description | isRequired |
| ------ | ------ |------ |
| account |  | Yes |
| token |  | Yes |
| access_token |  | Yes |
##### 3.7.3 Request
---
```
{
    params: {
        "account": "xxx",
        "token": "xxx",
        "access_token":"" //Access Token from Auth Server
    }
}
```
##### 3.7.4 Response
---
```
{
    "code": "1000",
    "msg":"",
    "data":{
        "user_id": 11, //User Primary Key, type: int
        "email": "xxx@gmail.com",
        "full_name": "xxx",
        "company_name": "xxx",
        "phone": "xxxx",
        "address1": "xxxx",
        "address2": "xxxx",
        "post_code": "xxx"
    }
}
```

#### 3.8 Modify User Detail API
##### 3.8.1 Url
---
| Url | Method |
| ------ | ------ |
| /detail/update | POST |
##### 3.8.2 Params
---
| Params | Description | isRequired |
| ------ | ------ |------ |
| account | email | Yes |
| full_name |  | No |
| company_name |  | No |
| phone |  | No |
| address1 |  | No |
| address2 |  | No |
| token |  | Yes |
| access_token |  | Yes |
##### 3.8.3 Request
---
```
{
    params: {
        "account": "xxx",
        "full_name": "xxx",
        "company_name": "xxx",
        "phone": "xxxx",
        "address1": "xxxx",
        "address2": "xxxx",
        "post_code": "xxx",
        "token": "xxx",
        "access_token":"" //Access Token from Auth Server
    }
}
```
##### 3.8.4 Response
---
```
{
    "code": "1000",
    "msg":"",
    "data":{
        "user_id": 11, //User Primary Key, type: int
        "email": "xxx@gmail.com",
        "full_name": "xxx",
        "company_name": "xxx",
        "phone": "xxxx",
        "address1": "xxxx",
        "address2": "xxxx",
        "post_code": "xxx"
    }
}
```
### 4. Print APIs
#### 4.1 Printer Templates API
##### 4.1.1 Url
---
| Url | Method |
| ------ | ------ |
| /templates | POST |
##### 4.1.2 Params
---
| Params | Description | isRequired |
| ------ | ------ |------ |
| access_token |  | Yes |
##### 4.1.3 Request
---
```
{
    params: {
        "access_token":"" //Access Token got from Auth API
    }
}
```
##### 4.1.4 Response
---
```
{
    "code": "1000",
    "msg":"",
    "data":[
    {
        "template_id": 1,
        "template_name": "xxx"
    }
    ]
}
```


### 5. Order APIs
#### 5.1 Create Print Order API
##### 5.1.1 Url
---
| Url | Method |
| ------ | ------ |
| /create | POST |
##### 5.1.2 Params
---
| Params | Description | isRequired |
| ------ | ------ |------ |
| plate_number |  | Yes |
| create_date | DD-MM-YY | Yes |
| service_type |  | Yes |
| next_service |  | Yes |
| access_token |  | Yes |
##### 5.1.3 Request
---
```
{
    params: {
        "plate_number": "xxxx",  //type: String
        "create_date": "09-10-2019",  //type: String
        "service_type": 1 // type: int
        "next_service": "68000" // type: String
        "access_token":"" //Access Token got from Auth API
    }
}
```
##### 5.1.4 Response
---
```
{
    "code": "1000",
    "msg":"",
    "data":{}
}
```

#### 5.2 Print Order History API
##### 5.2.1 Url
---
| Url | Method |
| ------ | ------ |
| /history | POST |
##### 5.2.2 Params
---
| Params | Description | isRequired |
| ------ | ------ |------ |
| account |  | Yes |
| token |  | Yes |
| access_token |  | Yes |
##### 5.2.3 Request
---
```
{
    params: {
        "account": "xxxx",  //User's email, type: String
        "token": "xxxxx",  //type: String
        "access_token":"" //Access Token got from Auth API
    }
}
```
##### 5.2.4 Response
---
```
{
    "code": "1000",
    "msg":"",
    "data":[
    {
        "order_id": 1, //type: int
        "plate_number": "xxxx",  //type: String
        "create_date": "09-10-2019",  //type: String
        "service_type": 1 // type: int
        "next_service": "68000" // type: String
    }
    ]
}
```
### 6. Content APIs
#### 6.1 Init API
##### 6.1.1 Url
---
| Url | Method |
| ------ | ------ |
| /init | POST |
##### 6.1.2 Params
---
| Params | Description | isRequired |
| ------ | ------ |------ |
| is_test | 0: 不是 1:是 | Yes |
| access_token |  | Yes |
##### 6.1.3 Request
---
```
{
    params: {
        "is_test": 1, //type: int
        "access_token":"" //Access Token got from Auth API
    }
}
```
##### 6.1.4 Response
---
```
{
    "code": "1000",
    "msg":"",
    "data":{
        "app_update_info": { //app升级信息
            "lastest_version": "1.0",
            "download_url": "xxxx"
        },
        "forced_up": 0, //是否强制升级 0:否 1:是
        "init_pics": [ //开机图片列表
        {
            "pic_id":11, 
            "pic_title":"xxx",
            "pic_url": "xxxx"
        }
        ]
    }
}
```

#### 6.2 QA List API
##### 6.2.1 Url
---
| Url | Method |
| ------ | ------ |
| /qa_list | POST |
##### 6.2.2 Params
---
| Params | Description | isRequired |
| ------ | ------ |------ |
| show_num | 默认10 | No |
| access_token |  | Yes |

##### 6.2.3 Request
---
```
{
    params: {
        "show_num": 10 //type: int
        "access_token":"" //Access Token got from Auth API
    }
}
```
##### 6.2.4 Response
---
```
{
    "code": "1000",
    "msg":"",
    "data":{
        "message_total_num": 10, //type: int
        "message_infos": [ //help 内容列表
        {
            "id":11, 
            "title":"xxx",
            "content": "xxxx",
            "display_type": 0, //展现形式 0:普通， 1：弹框 type: int
        }
        ]
    }
}
```