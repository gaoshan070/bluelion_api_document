# BlueLion APIs
---

| ID | Date | Author | Content |
| ------ | ------ | ------ |------ |
| 1 | 26/09/2019 | Shan Gao | 1st draft |
---
## 1. Abstract
Note: Before requesting any API, Auth API should be requested first and get access token.

### 1.1 Auth API

### 1.2 User APIs
> Including Login, Register, Change password, Reset password, User detail

### 1.3 Printer APIs
> Including Printing templates, Printing adjust
### 1.4 Order APIs
> Including Submiting Order, Order history

### Public Params
```
{
    "device_info": {
        "mac":"",
        "imei":"",
        "imsi":""
    },
    "other_info": {
        "app_id":"",
        "access_token":""
    }
}
```

### User APIs
${api_host}/user
> /login

| Param | Method | Response |
| ------ | ------ | ------ |
| {"account":"xxxx", "password":"xxxx","token": "xxxx"} | POST | {"username":"xxx","token":"xxxxxxx"} |

```

```

> /register
> /password/change
> /password/forget

### Print APIs
${api_host}/print

> /templates

### Order APIs
${api_host}/order

> /create
> /history
