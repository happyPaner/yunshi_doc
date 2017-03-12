# 教师端API设计文档

    说明：教师端API接口以`/t`作为模块标识，所有的教师端接口url都以`/t`开头，例如教师端绑定微信的请求接口：`/t/bind`。  
    返回数据统一为**JSON**格式。

## update log

> api接口改动说明。

## api

### 绑定微信：/bind

> 教师系统信息与教师的微信信息绑定接口。

请求URL：/t/bind  
请求方法：post  
访问权限：所有用户

提交数据：

    {
        name:       //string，教师姓名
        phone:      //string，教师的手机号
    }

返回数据：

    {
        code:       //int，0代表成功, 非0代表失败
        msg :       //string，成功或者失败说明
    }

### 首页数据：/index

> 教师端首页所需要的数据

请求URL：/t/index  
请求方法：get  
访问权限：已绑定教师用户 

url参数：无

返回数据：

    {
        code:       //int，0代表成功, 1代表未登录或未绑定微信，其他值为其他原因失败
        msg :       //string，成功或者失败说明
        data:{      //object，首页所需数据
            confirmedNum:         //int，已确认课时总数
            confirmedList: [        //array，已确认课时的学生列表
                {
                    name:                   //string，确认课时的学生姓名
                    num :                   //int，该学生确认课时数
                },
                {
                    ...
                }
            ],
            attendClass: [          //array，待上课列表
                {
                    classID:                  //int，该课时ID
                    name：               //string，学生姓名
                    date:                //string，上课日期，例如：2017-03-06
                    timeslot:            //string，上课时间段，例如："8:00-12:00","19:00-21:00"
                    classNum:            //int，本次课时数
                },
                {
                    ...
                }
            ],
            unConfirmedList:[         //array,未确认课时
                {
                    classID:                  //int，该课时ID
                    name：               //string，学生姓名
                    date:                //string，上课日期，例如：2017-03-06
                    timeslot:            //string，上课时间段，例如："8:00-12:00","19:00-21:00"
                    classNum:            //int，本次课时数
                    state:               //int，确认状态，1教师未发起，2教师已发起学生未确认，3学生已确认，4学生已拒绝
                },
                {
                    ...
                }
            ]
        } 
    }

### 学生列表：/student

> 获取教师当前学生信息列表

请求URL：/t/student  
请求方法：get  
访问权限：已绑定教师用户  

URL参数：无

返回数据：

    {
        code:       //int，0代表成功, 1代表未登录或未绑定微信，其他值为其他原因失败
        msg :       //string，返回信息说明
        data:[      //array，学生信息列表
            {
                name:           //string，学生姓名
                phone:          //string，学生手机号
                grade:          //string，学生年级
                lessionID:      //int，课程ID
                subject:        //string，教授科目
                weekClass:      //int，每周课时数
                type:           //string，辅导方式不，一对一，一对多等
                target:         //string，辅导目标，拔高，冲刺等
            },
            {
                ...
            }
        ]
    }

### 已确认课时汇总：/confirmedInfo

> 获取教师当前所有已确认课时详细列表

请求URL：/t/confirmedInfo
请求方法：get  
访问权限：已绑定教师用户  

URL参数：无

返回数据：

    {
        code:       //int，0代表成功, 1代表未登录或未绑定微信，其他值为其他原因失败
        msg :       //string，返回信息说明
        data:{      //object，已确认课时汇总信息
            confirmedNum:           //int，本月已确认课时数
            confirmedList: [        //array，已确认课时的学生列表
                {
                    name:                   //string，确认课时的学生姓名
                    num :                   //int，该学生确认课时数
                },
                {
                    ...
                }
            ],
        }
    }

### 已确认课时：/confirmed

> 获取教师当前所有已确认课时详细列表

请求URL：/t/comfired
请求方法：get  
访问权限：已绑定教师用户  

URL参数：无

返回数据：

    {
        code:       //int，0代表成功, 1代表未登录或未绑定微信，其他值为其他原因失败
        msg :       //string，返回信息说明
        data:[      //array，学生信息列表
            {
                classID:                  //int，此次排课的ID
                name：               //string，学生姓名
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

> 获取教师当前所有未确认课时详细列表

请求URL：/t/confirmed
请求方法：get  
访问权限：已绑定教师用户  

URL参数：无

返回数据：

    {
        code:       //int，0代表成功, 1代表未登录或未绑定微信，其他值为其他原因失败
        msg :       //string，返回信息说明
        data:[      //array，学生信息列表
            {
                classID:             //int，该课时ID
                name：               //string，学生姓名
                date:                //string，上课日期，例如：2017-03-06
                timeslot:            //string，上课时间段，例如："8:00-12:00","19:00-21:00"
                classNum:            //int，本次课时数
                state:               //int，确认状态，0已确认成功，1教师未发起，2教师已发起学生未确认，3学生已拒绝确认
            },
            {
                ...
            }
        ]
    }

### 发起确认：/confirmClass

> 教师同过此接口向学生发起确认课时请求，需在改课时完成之后发起确认，否则拒绝发起

请求URL：/t/confirmClass  
请求方法：post  
访问权限：已绑定教师用户  

提交参数：

    {
        classID:         //int，待确认课时id
    }

返回数据：

    {
        code：      //int，0代表成功，1代表未登录或未绑定微信，其他值未其他错误
        msg :       //string，返回状态说明
    }

### 发起排课：/planClass

> 教师通过此接口对学生发起排课

请求URL：/t/planClass
请求方法：post  
访问权限：已绑定教师用户  

提交参数：

    {
        lessonID:       //int，该课程的课程ID，不是课时的ID，课程与课时是两个概念
        date:           //int，timestamp，10位UNIX时间戳，单位秒
        timeslot:       //string，上课时间段，例如："8:00-12:00","19:00-21:00"
        classNum:       //int，本次课时数
        type:           //int, 1正常上课，2加课，3调课
    }

返回参数:

    {
        code：      //int，0代表成功，1代表未登录或未绑定微信，其他值未其他错误
        msg :       //string，返回状态说明
    }