# 学生端api设计文档

    说明：学生端API接口以`/s`作为模块标识，所有的学生端接口url都以`/s`开头，例如学生端绑定微信的请求接口：`/s/bind`。  
    返回数据统一为**JSON**格式。

## update log

> api接口改动说明。

### 2017-03-08 1:14 api更改日志

1、更换所有课时相关字段：id改为classID;  
2、更换所有老师姓名字段：teacher改为name;  
3、更换所有广告相关字段：id改为adID;  
4、部分业务添加classID字段

### 2017-03-10 api更改说明

1、更换/index[lessonList]、/restClass接口的allNum字段为allClass，以对应数据库字段名称
2、更换/index[lessonList]、/restClass接口的classID字段为lessonID，修正错误

## api

### 绑定微信：/bind

> 学生系统信息与教师的微信信息绑定接口。

请求URL：/s/bind  
请求方法：post  
访问权限：所有用户

提交数据：

    {
        name:       //string，学生的姓名
        grade:      //string，学生年级
        phone:      //string，学生手机号
    }

返回数据：

    {
        code:       //int，0代表成功, 非0代表失败
        msg :       //strin，成功或者失败说明
    }

### 主页信息：/index

> 获取学生主页相关信息

请求URL：/s/index  
请求方法：get  
访问权限：已绑定学生用户

URL参数：无

返回数据：

    {
        code:       //int，0代表成功, 非0代表失败
        msg :       //string，成功或者失败说明
        data:{      //object，返回信息
            restClass:        //int，剩余课时
            lessonList:[      //array，剩余详细列表
                {
                    lessonID:   //int，课程id
                    name:       //string，教师姓名
                    rest:       //int，剩余值
                    subject:    //string，科目
                    allClass :    //int，总课时数
                },
                {
                    ...
                }
            ],
            attendClass:[     //array，本周带上课列表
                {
                    classID:             //int,课程本次排课ID
                    subject:             //string，科目
                    name：               //string，老师姓名
                    date:                //string，上课日期，例如：2017-03-06
                    timeslot:            //string，上课时间段，例如："8:00-12:00","19:00-21:00"
                    classNum:            //int，本次课时数
                },
                {
                    ...
                }
            ],
            ad:[        //string，推荐/广告列表
                {
                    adID:     //int，广告ID
                    image:    //string，广告图片url
                },
                {
                    ...
                }
            ]             
        }
    }

### 课程信息: /restClass

> 获取学生全部剩余的课时

请求URL：/s/restClass  
请求方法：get  
访问权限：已绑定学生用户

URL参数：无

返回数据：

    {
        code:       //int，0代表成功, 非0代表失败
        msg :       //string，成功或者失败说明
        data:{      //object，返回信息
            restClass:      //int，总剩余课时
            lessonList:[    //array，剩余详细列表
                {
                    lessonID:   //int，课程ID
                    name:       //string，教师姓名
                    rest:       //int，剩余值
                    subject:    //string，科目
                    allClass :  //int，总课时数
                },
                {
                    ...
                }
            ]
        }
    }

### 已确认课时：/confirmed

> 获取学生当前所有已确认课时详细列表

请求URL：/s/comfired
请求方法：get  
访问权限：已绑定学生用户  

URL参数：无

返回数据：

    {
        code:       //int，0代表成功, 1代表未登录或未绑定微信，其他值为其他原因失败
        msg :       //string，返回信息说明
        data:[      //array，学生信息列表
            {
                classID:                  //int，此次排课的ID
                name：            //string，教师姓名
                date:                //string，上课日期，例如：2017-03-06
                timeslot:            //string，上课时间段，例如："8:00-12:00","19:00-21:00"
                classNum:            //int，本次课时数
            },
            {
                ...
            }
        ]
    }

### 未确认课时：/unconfirmed

> 获取学生当前所有未确认课时详细列表

请求URL：/s/confirmed  
请求方法：get  
访问权限：已绑定学生用户  

URL参数：无

返回数据：

    {
        code:       //int，0代表成功, 1代表未登录或未绑定微信，其他值为其他原因失败
        msg :       //string，返回信息说明
        data:[      //array，学生信息列表
            {
                classID:             //int，该课时ID
                name：               //string，教师姓名
                date:                //string，上课日期，例如：2017-03-06
                timeslot:            //string，上课时间段，例如："8:00-12:00","19:00-21:00"
                classNum:            //int，本次课时数
                state:               //int，确认状态，，1教师未发起，2教师已发起学生未确认，3已确认成功, 4学生已拒绝确认
            },
            {
                ...
            }
        ]
    }

### 确认课时：/confirmClass

请求URL：/s/confirmClass  
请求方法：post  
访问权限：已绑定学生用户  

URL参数：无

返回数据：

    {
        code:       //int，0代表成功, 1代表未登录或未绑定微信，其他值为其他原因失败
        msg :       //string，返回信息说明
        data:{
            classID:     //int，待确认课时id
        }
    }

### 本周待上课：/attendClass

> 获取学生本周待上课列表

请求URL：/s/attendClass  
请求方法：get  
访问权限：已绑定学生用户  

URL参数：无

返回数据：

    {
        code:       //int，0代表成功, 1代表未登录或未绑定微信，其他值为其他原因失败
        msg :       //string，返回信息说明 
        data:[      //array，本周带上课列表
                {
                    classID:             //int，待上课课时id
                    subject:             //string，科目
                    name：               //string，老师姓名
                    date:                //string，上课日期，例如：2017-03-06
                    timeslot:            //string，上课时间段，例如："8:00-12:00","19:00-21:00"
                    classNum:            //int，本次课时数
                },
                {
                    ...
                }
            ]
    }


### 我要推荐：/recommend

> 学生提交推荐信息接口

请求URL：/s/recommend  
请求方法：post  
访问权限：已绑定学生用户  

提交参数：

    {
        message:    //string，推荐信息内容，选填
    }

返回数据：

    {
        code:       //int，0代表成功, 1代表未登录或未绑定微信，其他值为其他原因失败
        msg :       //string，返回信息说明 
    }

### 我要续费：/renew

> 学生提交续费信息接口

请求URL：/s/renew  
请求方法：post  
访问权限：已绑定学生用户  

提交参数：

    {
        message:    //string，推荐信息内容，选填
    }

返回数据：

    {
        code:       //int，0代表成功, 1代表未登录或未绑定微信，其他值为其他原因失败
        msg :       //string，返回信息说明 
    }

### 广告详情：/ad

> 通过此接口获取广告详情

请求URL：/s/ad?adID=xx  
请求方法：get  
访问权限：已绑定学生用户  

URL参数：

    {
        adID:       //int，广告id
    }

返回数据：

    {
        code:       //int，0代表成功, 1代表未登录或未绑定微信，其他值为其他原因失败
        msg :       //string，返回信息说明 
        data:{
            title:        //string，广告页标题
            content:      //string，广告图文详情，html内容
            type:         //string，广告类型，是推荐广告还是续费广告，recommend推荐，renew续费
        }    
    }