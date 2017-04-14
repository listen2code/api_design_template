基于[从客户端的角度设计后端的接口](https://github.com/listen2code/article/tree/master/从客户端的角度设计后端的接口)设计的客户端接口模板。


# 公共信息

## 域名url
```
开发环境：http://www.online.com/api
测试环境：http://www.test.com/api
```

## code说明

| code | 说明 |
| --- | --- |
| 200 | 成功 |
| 401 | 登录过期 |

## 请求Path，http://www.xxx.com/api/ [path]

基础规范

| 操作行为 | Method | Path |
| --- | --- | --- |
| 查找 | GET | getXxx |
| 增加 | POST | addXxx/submitXxx |
| 修改 | POST | modifyXxx |
| 删除 | POST | delXxx |

示例（见名知意）
    
| 操作行为 | Method | Path |
| --- | --- | --- |
| 获取用户信息 | GET | getUserInfo |
| 增加收货地址 | POST | addAddress |
| 修改密码 | POST | modifyPwd |
| 删除收货地址 | POST | delAddress |
| 登陆 | GET | login |
| 发送短信验证码 | GET | sendSms |
| 订单支付 | POST | orderPay |

## 请求头参数

| 字段名称 | 说明 |
| --- | --- | 
| version | 客户端版本version，例：1.0.0 |
| token | 登陆成功后，server返回的登陆令牌token |
| os | 手机系统版本（Build.VERSION.RELEAS）例：4.4，4.5 |
| from | 请求来源，例：android/ios/h5 |
| screen | 手机尺寸，例：1080*1920 |
| model | 机型信息（Build.MODEL），例：Redmi Note 3 |
| channel | 渠道信息，例：com.wandoujia |
| net | APP当前网络状态，例：wifi，mobile；部分接口可以根据用户当前的网络状态，下发不同数据策略，如：wifi则返回高清图，mobile情况则返回缩略图 |
| appid | APP唯一标识，有的公司一套server服务多款APP时，需要区分开每个APP来源 |
| sign | 对请求参数进行计算的md5 |

## 应答参数格式

| 字段名称 | 说明 |
| --- | --- |
| code | 响应状态码，200：成功；非200：失败 |
| msg | 请求失败时的message|
| time | 服务端时间戳，单位：毫秒。用于同步时间 |
| data | 数据实体 |

* object类型数据

```
// json
{
  "code":200,
  "msg":"成功",
  "time":"1482213602000",
  "data": {
    "name":"张三",
    "age":"20"
  }
}  
     
// model.java
public class Model {
     public String name;
     public String age;
} 
```
* Array类型数据

```
// json
{
    "code":200,
    "msg":"成功",
    "time":"1482213602000",
    "data": {
        "list":["张三","李四"]
    }
}   
  
// model.java
public class Model {
    public List<String> list;
} 
```

* 分页类型数据

```
// json
{
    "code":200,
    "msg":"成功",
    "time":"1482213602000",
    "data": {
        "list":["张三","李四"],
        "total":"10"
    }
}   
  
// model.java
public class Model {
    public List<String> list;
    public String total;
} 
```
* 错误类型数据

```
// json
{
    "code":401,
    "msg":"登录过期",
    "time":"1482213602000",
    "data": {}
}   
```

## 备注
```json
1.请求，应答参数，一律使用驼峰命名，例：firstInviteTime
2.数据获取型接口使用GET，提交型接口使用POST
3.字段说明信息要清晰，完整，例：[1：待支付，2：待支付，3:已完成]
4.金额，时间字段要备注单位，例：(租车时间，单位：毫秒，优惠时长，单位：分钟)
5.boolean型数据，统一采用"isSelect","isCheck"，以"is"开头，数据返回，1:是，0:否
6.相同的业务字段，尽量不要使用不同的字段名称，例如：行程ID，不要出现"tripId"，"trip_id"多个版本。在进行字段命名的时候可以看下其他接口是否有相同字段
```


# 登录相关

## 登陆

```
通过账号+密码进行登录
```

### 接口
| URI | 方法 |
| --- | --- |
| /login | POST |

### 请求参数
| 名称 | 说明 |
| --- | --- |
| uName | 用户名/手机号码 |
| pwd | 密码(加密格式：md5(md5(pwd))) |

### 应答参数
| 名称 | 类型 | 说明 |
| --- | --- | --- |
| token | String | 登陆成功后返回token |

### 示例
```json
{
  "code":200,
  "msg":"成功",
  "time":"1482213602000",
  "data": {
    "token":"202cb962ac59075b964b07152d234b70"
  }
}
```

# 账号相关

## 忘记密码
```
根据手机号+短信验证码进行密码重置
```
### 接口
| URI | 方法 |
| --- | --- |
| /resetPwd | POST |

### 请求参数
| 名称 | 说明 |
| --- | --- |
| phone | 手机号码 |
| code | 短信验证码|
| pwd | 新密码(加密格式：md5(md5(pwd))) |

### 应答参数
| 名称 | 类型 | 说明 |
| --- | --- | --- |
| 无 |

### 示例
```json
{
  "code":200,
  "msg":"成功",
  "time":"1482213602000",
  "data": {}
}
```

## 修改密码
```
根据旧密码，重新设置新密码
```
### 接口
| URI | 方法 |
| --- | --- |
| /modifyPwd | POST |

### 请求参数
| 名称 | 说明 |
| --- | --- |
| oldPwd | 原密码(加密格式：md5(md5(oldPwd))) |
| newPwd | 新密码(加密格式：md5(md5(newPwd))) |

### 应答参数
| 名称 | 类型 | 说明 |
| --- | --- | --- |
| 无 |

### 示例
```json
{
  "code":200,
  "msg":"成功",
  "time":"1482213602000",
  "data": {}
}
```

