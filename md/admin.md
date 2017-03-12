# 后台管理系统API文档

    说明：后台管理API接口以`/admin`作为模块标识，所有的后台管理接口url都以`/admin`开头。  
    返回数据统一为**JSON**格式。

## 用户 User

> 当前登陆用户的信息管理

### 登录 /login

> 后端管理用户登录接口

请求URL：/admin/user/login     
请求方法：post  
访问权限：所有用户  

提交数据：

    {
        name:       //string，必填，用户名
        password:   //string，必填，用户密码
    }

返回数据：

    {
        code:       //int，0代表成功, 非0代表失败
        msg :       //string，成功或者失败说明
    }

### 退出 /logout

> 后台退出接口

请求URL：/admin/user/logout     
请求方法：get  
访问权限：后台已登录管理员  

提交数据：无

返回数据：

    {
        code:       //int，0代表成功, 非0代表失败
        msg :       //string，成功或者失败说明
    }

### 获取资料 /info

> 获取已登录员工用户的资料

请求URL：/admin/user/info     
请求方法：get  
访问权限：后台已登录管理员  

提交数据：无

返回数据：

    {
        code:       //int，0代表成功, 非0代表失败
        msg :       //string，成功或者失败说明
        data:{
            name:       //string，姓名
            phone:      //string，手机号
            roler:      //string，职位角色
            createTime: //int，创建时的UNIX时间戳，10位，以秒为单位
        }
    }

### 修改资料 /update

> 已登录用户修改自己的资料

请求URL：/admin/user/update   
请求方法：put  
访问权限：已登录用户  

提交数据：

    {
        name:       //string，选填，用户名
        phone:      //string，选填，手机号码
    }

返回数据:

    {
        code:       //int，0代表成功, 非0代表失败
        msg :       //string，成功或者失败说明
    }

### 修改密码 /password

> 修改登陆密码

请求URL：/admin/user/password   
请求方法：put  
访问权限：已登录用户  

提交数据：

    {
        oldpassword:     //string，必填，旧密码   
        password:        //string，必填，新密码
        repassword:      //string，必填，确认新密码
    }

返回数据:

    {
        code:       //int，0代表成功, 非0代表失败
        msg :       //string，成功或者失败说明
    }

## 员工组织 Staff

### 查询员工 /

> 获取员工信息

请求URL：/admin/staff   
请求方法：get  
访问权限：已登录用户  

提交数据：

    {
        staffID:    //int，选填，要查询的员工ID，值为空时返回所有员工信息
        roler:      //int，选填，根据职位角色查询员工列表
    }

返回数据:

    {
        code:       //int，0代表成功, 非0代表失败
        msg :       //string，成功或者失败说明
        data: [
            {
                name:       //string，员工姓名
                phone:      //string，手机号码
                roler:      //string，员工职位角色
                createTime: //int，创建时的UNIX时间戳，10位，以秒为单位
            },
            {
                ...
            }
        ]
    }

### 添加员工 /add

> 超级管理员或上级主管添加普通员工管理员

请求URL：/admin/staff/add   
请求方法：post  
访问权限：上级主管  

提交数据：

    {
        name:       //string，必填，用户名
        password:   //string，必填，用户密码
        repassword: //string，必填，确认密码
        phone:      //string，必填，手机号码
        roler:      //int，必填，员工角色（职位）代码，通过/user/roler接口获得全部角色
    }

返回数据:

    {
        code:       //int，0代表成功, 非0代表失败
        msg :       //string，成功或者失败说明
    }

### 更改员工 /update

> 超级管理员或上级主管更改普通员工管理员信息

请求URL：/admin/staff/update   
请求方法：put  
访问权限：所有用户  

提交数据：

    {
        name:       //string，选填，用户名
        phone:      //string，选填，手机号码
        roler:      //int，选填，员工角色（职位）代码，通过/user/roler接口获得全部角色，只有超级管理员或者总经理具有更改权限
    }

返回数据:

    {
        code:       //int，0代表成功, 非0代表失败
        msg :       //string，成功或者失败说明
    }

### 删除员工 /delete

> 删除下属员工

请求URL：/admin/staff/delete   
请求方法：delete  
访问权限：上级主管  

提交数据：

    {
        staffID:   //int，必填，要删除的员工ID
    }

返回数据:

    {
        code:       //int，0代表成功, 非0代表失败
        msg :       //string，成功或者失败说明
    }

## 员工角色 Roler

### 获取角色 /

> 从同此接口获取所有员工角色列表及角色代码

请求URL：/admin/roler   
请求方法：get  
访问权限：所有用户  

提交参数：无

返回数据:

    {
        code:       //int，0代表成功, 非0代表失败
        msg :       //string，成功或者失败说明
        data:[      //array
            {
                code:       //int，角色代码
                name:       //string，角色名称
            },
            {
                ...
            }
        ]
    }

## 学生 Student

> 学员、资源信息管理

## 教师 Teacher

> 注册教师管理

### 教师列表 /list

> 获取员工信息列表

请求URL：/admin/teacher/list   
请求方法：get  
访问权限：已登录用户  

提交数据：

    {
        page:           //int，选填，页码
    }

返回数据:

    {
        code:       //int，0代表成功, 非0代表失败
        msg :       //string，成功或者失败说明
        data: {
            page:       //int，当前页码
            allPage:    //int，总页码数
            list: [
                {
                    teacherID:  //int，教师ID
                    name:       //string，教师姓名
                    age:        //int，年龄
                    teachAge:   //int，教龄
                    subject:    //int，教学科目
                    grade:      //string，教授年级
                    classType:  //string，辅导类型，一对一/一对多/...
                    zone:       //string，代课区域
                    preprice:   //int，单课时费
                    phone:      //string，手机号码
                    state:      //int，教师审核状态
                },
                {
                    ...
                }
            ]
        }
    }

### 教师信息 /info

> 获取员工信息列表

请求URL：/admin/teacher/info   
请求方法：get  
访问权限：已登录用户  

提交数据：

    {
        teacherID:           //int，必填，教师ID
    }

返回数据:

    {
        code:       //int，0代表成功, 非0代表失败
        msg :       //string，成功或者失败说明
        data: {
            teacherID:      //int，教师ID
            name:           //string，教师姓名
            phone:          //string，手机号码
            sex:            //string，教师性别，男，女
            age:            //int，年龄
            photo:          //string，照片地址
            teachAge:       //int，教龄
            subject:        //int，教学科目
            postShcool:     //string，任教学校
            grade:          //string，教授年级
            address:        //string，住址
            hometown:       //string，原籍
            classType:      //string，辅导类型，一对一/一对多/...
            preprice:       //int，每节课价格
            freeTime:       //string，空闲时间
            zone:           //string，授课区域
            infoFrom:       //string，信息来源
            contact:        //string，公司接待人员姓名
            createTime:     //int，创建时的UNIX时间戳，10位，以秒为单位

            recommendTeacher:   //string，可推荐老师
            assessment:         //string，机构评价
            state:              //int，审核状态，1审核未通过，2审核通过，3已签约，4加入黑名单
            reviewer:           //string，审核人姓名
            reviewTime:         //int，审核时间，审核时的UNIX时间戳，10位，以秒为单位         
        }
        
    }

### 添加教师 /add

> 超级管理员或上级主管添加普通员工管理员

请求URL：/admin/teacher/add   
请求方法：post  
访问权限：上级主管  

提交数据：

    {
        name:           //string，必填，教师姓名
        phone:          //string，必填，手机号码
        sex:            //string，教师性别，男/女
        age:            //int，年龄
        photo:          //string，照片地址
        teachAge:       //int，教龄
        subject:        //int，教学科目
        postShcool:     //string，任教学校
        grade:          //string，教授年级
        address:        //string，现在住址
        hometown:       //string，原籍
        classType:      //string，辅导类型，一对一/一对多/...
        preprice:       //int，必填，每节课价格
        freeTime:       //string，空闲时间
        zone:           //string，授课区域
        org:            //string，老师代课机构
        teachIntro:     //string，教学简介
        infoFrom:       //string，信息来源
        contactID:      //string，必填，公司接待人员ID

        recommendTeacher:   //string，可推荐老师
        assessment:         //string，机构评价
        state:              //int，审核状态，1审核未通过，2审核通过，3已签约，4加入黑名单
        reviewer:           //string，审核人姓名
        reviewTime:         //int，审核时间，审核时的UNIX时间戳，10位，以秒为单位
    }

返回数据:

    {
        code:       //int，0代表成功, 非0代表失败
        msg :       //string，成功或者失败说明
    }

### 更改教师 /update

> 超级管理员或上级主管更改普通员工管理员信息

请求URL：/admin/teacher/update   
请求方法：put  
访问权限：所有用户  

提交数据：

    {
        name:           //string，教师姓名
        phone:          //string，手机号码
        sex:            //string，教师性别，男/女
        age:            //int，年龄
        photo:          //string，照片地址
        teachAge:       //int，教龄
        subject:        //int，教学科目
        postShcool:     //string，任教学校
        grade:          //string，教授年级
        address:        //string，住址
        hometown:       //string，原籍
        classType:      //string，辅导类型，一对一/一对多/...
        preprice:       //int，每节课价格
        freeTime:       //string，空闲时间
        zone:           //string，授课区域
        org:            //string，老师代课机构
        teachIntro:     //string，教学简介
    }

返回数据:

    {
        code:       //int，0代表成功, 非0代表失败
        msg :       //string，成功或者失败说明
    }

### 删除教师 /delete

> 删除教师

请求URL：/admin/teacher/delete   
请求方法：delete  
访问权限：上级主管  

提交数据：

    {
        teacherID:   //int，必填，要删除的员工ID
    }

返回数据:

    {
        code:       //int，0代表成功, 非0代表失败
        msg :       //string，成功或者失败说明
    }

## 订单 Order

### 订单列表 /list

> 获取订单列表，订单与课程是一对多关系，一个订单可能会有多个课程，每个课程对应一个老师

请求URL：/admin/order/list   
请求方法：get  
访问权限：已登录用户  

提交数据：

    {
        page:      //int，选填，页码数，值为空时代表第一页
    }

返回数据:

    {
        code:       //int，0代表成功, 非0代表失败
        msg :       //string，成功或者失败说明
        data: {
            page:           //int，当前页码
            allPage:        //int，总页码数
            list:[
                {
                    orderID:        //int，订单ID
                    name:           //string，学生姓名
                    age:            //int，学生年龄
                    phone:          //string，学生手机号
                    amount:         //int，缴费金额
                    state:          //int，状态，1已提交未被确认，2已被财务确认，3订单被取消
                    createTime:     //int，订单创建时间，10位UNIX时间戳
                    lesson: [
                        {
                            lessonID:       //int，课程ID
                            name:           //string，教师姓名
                            teacheAge:      //int，教龄
                            phone:          //string，联系电话
                            subject:        //string，科目
                            allClass:       //int，所有课时
                            preprice:       //int，单课时费
                            classType:      //string，代课类型，一对一
                        },
                        {
                            ...
                        }
                    ]
                }
            ]
        }
    }


### 订单信息 /info

> 获取某订单详情

请求URL：/admin/order/info   
请求方法：get  
访问权限：已登录用户  

提交数据：

    {
        orderID:    //int，必填，订单ID
    }

返回数据:

    {
        code:       //int，0代表成功, 非0代表失败
        msg :       //string，成功或者失败说明
        data: {
            orderID:        //int，订单ID
            name:           //string，学生姓名
            age:            //int，学生年龄
            phone:          //string，学生手机号
            amount:         //int，缴费金额
            state:          //int，状态，1已提交未被确认，2已被财务确认，3订单被取消
            createTime:     //int，订单创建时间，10位UNIX时间戳
            lesson: [
                {
                    lessonID:       //int，课程ID
                    name:           //string，教师姓名
                    teacheAge:      //int，教龄
                    phone:          //string，联系电话
                    subject:        //string，科目
                    allClass:       //int，所有课时
                    preprice:       //int，单课时费
                    classType:      //string，代课类型，一对一
                },
                {
                    ...
                }
            ]
        }
    }

### 添加订单 /add

> 超级管理员或上级主管添加普通员工管理员

请求URL：/admin/staff/add   
请求方法：post  
访问权限：上级主管  

提交数据：

    {
        name:       //string，必填，用户名
        password:   //string，必填，用户密码
        repassword: //string，必填，确认密码
        phone:      //string，必填，手机号码
        roler:      //int，必填，员工角色（职位）代码，通过/user/roler接口获得全部角色
    }

返回数据:

    {
        code:       //int，0代表成功, 非0代表失败
        msg :       //string，成功或者失败说明
    }

### 更改订单 /update

### 删除订单 /delete