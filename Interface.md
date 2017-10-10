# 目录
- [概述](#概述)
- [管理员登录](#login)
- [管理员注销](#logout)
- [管理员修改密码](#updatePassword)
- [管理员添加管理员](#addAdmin)
- [管理员启用/禁用管理员](#enabled)
- [管理员修改指定管理员角色](#editAdminPermission)
- [管理员获取可编辑的管理员列表](#getAdminList)
- [管理员获取可操作的角色列表](#getRoleList)
- [管理员删除管理员](#delAdmin)
- [添加辅警账号信息](#addPolice)
- [编辑辅警账号信息](#editPolice)
- [删除辅警账号信息](#delPolice)
- [获取可操作的辅警账号列表](#getPoliceList)
- [获取辅警上传记录信息](#getUploadInfo)
- [获取辅警上传记录的统计信息](#statistics)
- [获取权限内的所有派出所信息](#getPCS)
- [获取权限内的所有县区信息](#getXQ)
- [获取权限内的所有市局信息](#getSJ)

## 概述
### 请求参数的组装
#### POST请求
将参数以键值对的形式存入data对象中，使用JSON.stringify()序列化对象并以data作为键传给服务端，例如登录请求需要提交username与password，组装后以表单的形式提交给服务端{data:{"username":"admin","password":"admin"}}
#### GET请求
直接将参数以键值对的形式提交给服务端，如果参数是对象需要先进行序列化操作，例如获取可操作的管理员接口可提供筛选条件filter，组装后的形式为{filter:JSON.stringify(filter)}
### 返回参数描述
#### 模板

```
{
    "code": 200,          //结果码，200为请求成功
    "message": "登录成功", //结果描述
    "data": {}            //放置此次请求的响应数据，详细结构看接口文档
}
```
## 接口文档
### 总体描述
如果接口不带业务数据返回将没有返回参数描述，其请求结果查看code与message，如果请求带有分页参数，其模板如下
```
{
    "pageCurrent": 1,    //当前页
    "pageSize": 30,      //每页条数
    "totalCount": 100    //数据总数
}
```
### login
#### 概述
登录系统接口，POST请求，请求地址 /login
#### 请求参数

参数 | 说明 | 类型/可空
------------ | ------------- | -------------
username | 登录用户名 | String/不可空
password | 登录密码 | String/不可空

### logout
#### 概述
登录系统，将会消除存有的seesion，请求地址 /logout

### updatePassword
#### 概述
用户修改密码，POST请求，请求地址 /updatePassword
#### 请求参数

参数 | 说明 | 类型/可空
------------ | ------------- | -------------
username | 登录用户名 | String/不可空
password | 登录密码 | String/不可空
newpassword | 新密码 | String/不可空

### addAdmin
#### 概述
向系统添加一个不带角色的管理员，默认密码123456，POST请求，请求地址 /system/admin/addAdmin
#### 请求参数

参数 | 说明 | 类型/可空
------------ | ------------- | -------------
username | 用户名 | String/不可空
realname | 真实姓名 | String/不可空

### enabled
#### 概述
启用或禁用一个管理员用户，POST请求，请求地址 /system/admin/enabled
#### 请求参数

参数 | 说明 | 类型/可空
------------ | ------------- | -------------
userCode | 管理员标识 | String/不可空
isEnabled | 是否启用 | Boolean/不可空


### editAdminPermission
#### 概述
修改管理员的角色，POST请求，请求地址 /system/admin/edit
#### 请求参数

参数 | 说明 | 类型/可空
------------ | ------------- | -------------
userCode | 管理员标识 | String/不可空
roleCode | 角色标识 | String/不可空

### getAdminList
#### 概述
获取可编辑的管理员列表，GET请求，请求地址 /system/admin/list
#### 请求参数

参数 | 说明 | 类型/可空
------------ | ------------- | -------------
filter | 筛选项 | Object/可空
pagination | 分页项 | Object/可空

#### 筛选项描述

参数 | 说明 | 类型
------------ | ------------- | -------------
username | 用户名 | String
realname | 真实姓名 | String

#### 返回参数模板

```
{
  "code": 200,
  "message": "获取管理员信息成功",
  "data": {
    "rows": [
      {
        "userCode": 8,                                          //管理员唯一标识
        "username": "admin2",                                   //管理员用户名
        "password": "d033e22ae348aeb5660fc2140aec35850c4da997", //密码
        "roleCode": 499,                                        //角色标识
        "isUse": 1,                                             //是否启用 0->未启用
        "realname": "测试用户",                                  //真实姓名
        "description": "德安县公安局丰林派出所",                   //角色描述
        "roleResource": "C-360426706000"                        //角色资源代码
      }
    ],
	//如果启用了分页则有下面的数据
    "pagination": {
      "pageCurrent": 1,
      "pageSize": 1,
      "totalCount": 1000
    }
  }
}
```

### getRoleList
#### 概述
获取管理员可操作的角色列表，GET请求，请求地址 /system/admin/roleList
#### 请求参数

参数 | 说明 | 类型/可空
------------ | ------------- | -------------
filter | 筛选项 | Object/可空
pagination | 分页项 | Object/可空

#### 筛选项描述

参数 | 说明 | 类型
------------ | ------------- | -------------
nickName | 角色名称 | String
description | 角色描述 | String

#### 返回参数模板

```
{
  "code": 200,
  "message": "获取可选角色信息成功",
  "data": {
    "rows": [
      {
        "roleCode": 45,                       //角色唯一标识
        "nickName": "分县局管理员",            //角色名称
        "description": "九江市公安局八里湖分局",//角色描述
        "roleResource": "B-360401"           //角色资源代码
      }
    ],
	//如果启用了分页则有下面的数据
    "pagination": {
      "pageCurrent": 1,
      "pageSize": 1,
      "totalCount": 1000
    }
  }
}
```

### delAdmin
#### 概述
删除管理员，POST请求，请求地址 /system/admin/del
#### 请求参数

参数 | 说明 | 类型/可空
------------ | ------------- | -------------
userCode | 管理员标识 | String/不可空

### addPolice
#### 概述
添加辅警账号信息，POST请求，请求地址 /system/police/addPolice
#### 请求参数

参数 | 说明 | 类型/可空
------------ | ------------- | -------------
username | 辅警用户名 | String/不可空
password | 辅警密码 | String/不可空
name | 辅警真实姓名 | String/不可空
idcard | 辅警身份证号 | String/不可空
policeno | 辅警警号 | String/不可空

### delPolice
#### 概述
删除辅警账号信息，POST请求，请求地址 /system/police/delPolice
#### 请求参数

参数 | 说明 | 类型/可空
------------ | ------------- | -------------
policeId | 辅警唯一标识 | String/不可空

### editPolice
#### 概述
编辑辅警账号信息，POST请求，请求地址 /system/police/editPolice
#### 请求参数

参数 | 说明 | 类型/可空
------------ | ------------- | -------------
data | 修改的辅警账号信息 | String/不可空

#### 修改的辅警账号信息描述

参数 | 说明 | 类型/可空
------------ | ------------- | -------------
username | 辅警用户名 | String/可空
password | 辅警密码 | String/可空
name | 辅警真实姓名 | String/可空
idcard | 辅警身份证号 | String/可空
policeno | 辅警警号 | String/可空

### getPoliceList
#### 概述
获取可操作的辅警账号列表，GET请求，请求地址 /system/police/list
#### 请求参数

参数 | 说明 | 类型/可空
------------ | ------------- | -------------
filter | 筛选项 | Object/可空
pagination | 分页项 | Object/可空

#### 筛选项描述

参数 | 说明 | 类型
------------ | ------------- | -------------
username | 辅警用户名 | String
password | 辅警密码 | String
name | 辅警真实姓名 | String
idcard | 辅警身份证号 | String
policeno | 辅警警号 | String

#### 返回参数模板

```
{
  "code": 200,
  "message": "获取可操作辅警列表成功",
  "data": {
    "rows": [
      {
        "policeId": 2958,       //辅警警号标识
        "username": "026606",   //辅警用户名
        "password": "111111",   //辅警密码
        "name": "梁广达",        //辅警真实姓名
        "idcard": "3608729374618234", //辅警身份证号
        "policeno": "026606"    //辅警警号
      }
    ],
    //如果启用了分页则有下面的数据
    "pagination": {
      "pageCurrent": 1,
      "pageSize": 1,
      "totalCount": 1000
    }
  }
}
```

### getUploadInfo
#### 概述
获取速采上传信息，GET请求，请求地址 /system/upload/list
#### 请求参数

参数 | 说明 | 类型/可空
------------ | ------------- | -------------
filter | 筛选项 | Object/可空
pagination | 分页项 | Object/可空

#### 筛选项描述
查看下面的参数模板，都可以拿来筛选

#### 返回参数模板

```
{
  "code": 200,
  "message": "获取上传信息成功",
  "data": {
    "rows": [
      {
        "uploadId": 23,                                                            //上传数据的唯一标识
        "username": "02150601",                                                    //上传辅警的用户名
        "name": "陈少建",                                                           //上传辅警的真实姓名
        "personCount": 2,                                                           //上传数据的人头数
        "type": "自住",                                                             //上传类型
        "scsj": "2017-9-30 12:00:00",                                               //速采时间
        "scrq": "2017年9月30号",                                                     //速采日期
        "policeno": "021506",                                                       //警号
        "pcsmc": "九江市公安局濂溪区分局妙智派出所",                                    //派出所名称
        "ssxqmc": "九江市公安局濂溪区分局",                                            //县区名称
        "sssjmc": "九江市公安局",                                                     //市局名称
        "qdzxx": "江西省九江市濂溪区九江市濂溪区十里大道1788号怡溪苑1788号17栋2单元401室"  //全地址名称
      }
    ],
    //如果启用了分页则有下面的数据
    "pagination": {
      "pageCurrent": 1,
      "pageSize": 1,
      "totalCount": 1000
    }
  }
}
```


### statistics
#### 概述
速采上传信息的数据统计，GET请求，请求地址 /system/upload/statistics
#### 请求参数

参数 | 说明 | 类型/可空
------------ | ------------- | -------------
type | 数据统计类型 | String/可空

#### 数据统计类型描述
值 | 说明 
------------ | ------------- 
date | 根据采集日期统计 
xq | 根据县区统计 
sj | 根据市局统计
pcs | 根据派出所统计 
name | 根据真实姓名统计，默认选项 

#### 返回参数模板

```
//根据真实姓名的统计
{
  "code": 200,
  "message": "获取统计数据成功",
  "data": [
    {
      "personCount": 130, //采集人口数
      "dataCount": 131,  //数据采集量
      "xText": "付璐"    //统计的X轴描述
    },
    {
      "personCount": 45,
      "dataCount": 45,
      "xText": "刘铭"
    },
    {
      "personCount": 25,
      "dataCount": 25,
      "xText": "雷绪康"
    },
    {
      "personCount": 57,
      "dataCount": 57,
      "xText": "黄潇"
    }
  ]
}
//根据速采日期的统计
{
  "code": 200,
  "message": "获取统计数据成功",
  "data": [
    {
      "personCount": 99,
      "dataCount": 119,
      "xText": "2017年9月30号"
    },
    {
      "personCount": 44,
      "dataCount": 28,
      "xText": "2017年10月01号"
    },
    {
      "personCount": 40,
      "dataCount": 51,
      "xText": "2017年10月02号"
    },
    {
      "personCount": 99,
      "dataCount": 92,
      "xText": "2017年10月03号"
    }
  ]
}
//其他的统计可以自己测试
```

### getPCS
#### 概述
获取权限内的派出所，GET请求，请求地址 /system/upload/pcsList

#### 返回参数模板

```
{
  "code": 200,
  "message": "获取派出所信息成功",
  "data": [
    {
      "code": "360401021000",                   //派出所代码
      "name": "九江市公安局八里湖分局西二路派出所"  //派出所名称
    },
    {
      "code": "360401022000",
      "name": "九江市公安局八里湖分局七里湖派出所"
    },
    {
      "code": "360401023000",
      "name": "九江市公安局八里湖分局长江路派出所"
    },
    {
      "code": "360401026000",
      "name": "九江市公安局八里湖分局滨兴派出所"
    },
    {
      "code": "360401027000",
      "name": "九江市公安局八里湖分局官牌夹派出所"
    }
  ]
}
```

### getXQ
#### 概述
获取权限内的县区，GET请求，请求地址 /system/upload/xqList

#### 返回参数模板

```
{
  "code": 200,
  "message": "获取县区信息成功",
  "data": [
    {
      "code": "360401",             //县区代码
      "name": "九江市公安局八里湖分局" //县区名称
    },
    {
      "code": "360402",
      "name": "九江市公安局濂溪区分局"
    },
    {
      "code": "360403",
      "name": "九江市公安局浔阳分局"
    },
    {
      "code": "360421",
      "name": "九江县公安局"
    },
    {
      "code": "360423",
      "name": "武宁县公安局"
    },
    {
      "code": "360424",
      "name": "修水县公安局"
    }
  ]
}
```

### getSJ
#### 概述
获取权限内的市局，GET请求，请求地址 /system/upload/sjList

#### 返回参数模板

```
{
  "code": 200,
  "message": "获取市局信息成功",
  "data": [
    {
      "code": "360400000000", //市局代码
      "name": "九江市公安局"   //市局名称
    }
  ]
}
```