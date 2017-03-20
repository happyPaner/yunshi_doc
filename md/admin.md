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

### 查询员工 /info

> 获取员工信息

请求URL：/admin/staff/info   
请求方法：get  
访问权限：已登录用户  

提交数据：

    {
        staffID:    //int，选填，要查询的员工ID，值为空时返回所有员工信息
        roler:      //int，选填，根据职位角色查询员工列表，1超级管理员，2总经理，3销售主管，4售后主管，5销售专员，6售后专员，7财务人员
    }

返回数据:

    {
        code:       //int，0代表成功, 非0代表失败
        msg :       //string，成功或者失败说明
        data: [
            {
                staffID:    //int，员工ID
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
        staffID:    //int，必填，员工ID
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

> 学员、资源信息管理，资源与学生是一对一关系，所以统一进行管理。

### 学生/资源列表 /list

> 获取学生/资源列表

请求URL：/admin/student/list   
请求方法：get  
访问权限：已登录用户  

提交数据：

    {
        isDeal:     //int，选填，0未成交的，1已成交已签单，空为全部
        page:       //int，选填，页码
    }

返回数据：

    {
        code:       //int，0代表成功, 非0代表失败
        msg :       //string，成功或者失败说明
        data: {
            studentID:      //int，学生ID
            name:           //string，学生姓名
            phone:          //string，学生手机号
            school:         //string，学生学校
            grade:          //string，学生年级
            address:        //string，家庭住址
            parent:         //string，家长姓名
            parentPhone:    //string，家长手机号
            headmaster:     //string，售后班主任
            channel:        //string，资源渠道
            developer:      //string，开发者
            level:          //int，资源等级
            state:          //int，资源状态，1跟进中，2已约课，3待匹配，4待试课，5试课未缴费
        }
    }

### 学生/资源列表 /info

> 获取学生/资源列表

请求URL：/admin/student/info   
请求方法：get  
访问权限：已登录用户  

提交数据：

    {
        studentID:      //int，学生ID
    }

返回数据：

    {
        code:       //int，0代表成功, 非0代表失败
        msg :       //string，成功或者失败说明
        data: {
            studentID:      //int，学生ID
            name:           //string，学生姓名
            phone:          //string，学生手机号
            school:         //string，学生学校
            grade:          //string，学生年级
            address:        //string，家庭住址
            parent:         //string，家长姓名
            parentPhone:    //string，家长手机号
            headmasterID:   //int，售后班主任员工ID
            headmaster:     //string，售后班主任
            channel:        //string，资源渠道
            developerID:    //int，开发者员工ID
            developer:      //string，开发者
            level:          //int，资源等级
            state:          //int，资源状态，0已缴费正式成为学员，1跟进中，2已约课，3待匹配，4待试课，5试课未缴费
        }
    }

### 添加学生/资源 /add

> 获取学生/资源列表

请求URL：/admin/student/add   
请求方法：post  
访问权限：已登录用户  

提交数据：

    {
        name:           //string，学生姓名，必填
        phone:          //string，学生手机号，必填
        school:         //string，学生学校，必填
        grade:          //string，学生年级，必填
        address:        //string，家庭住址，选填
        parent:         //string，家长姓名，选填
        parentPhone:    //string，家长手机号，选填
        headmasterID:   //int，售后班主任员工ID，选填
        channel:        //string，资源渠道，选填
        developerID:    //int，开发者员工ID，选填
        level:          //int，资源等级，选填
        state:          //int，资源状态，0已缴费正式成为学员，1跟进中，2已约课，3待匹配，4待试课，5试课未缴费，必填
    }
    
返回数据：

    {
        code:       //int，0代表成功, 非0代表失败
        msg :       //string，成功或者失败说明
    }

### 修改学生/资源 /update

> 获取学生/资源列表

请求URL：/admin/student/update   
请求方法：put  
访问权限：已登录用户  

提交数据：

    {
        studetID:       //int，学生ID，必填
        name:           //string，学生姓名，选填
        phone:          //string，学生手机号，选填
        school:         //string，学生学校，选填
        grade:          //string，学生年级，选填
        address:        //string，家庭住址，选填
        parent:         //string，家长姓名，选填
        parentPhone:    //string，家长手机号，选填
        headmasterID:   //int，售后班主任员工ID，选填
        channel:        //string，资源渠道，选填
        developerID:    //int，开发者员工ID，选填
        level:          //int，资源等级，选填
        state:          //int，资源状态，0已缴费正式成为学员，1跟进中，2已约课，3待匹配，4待试课，5试课未缴费，选填
    }
    
返回数据：

    {
        code:       //int，0代表成功, 非0代表失败
        msg :       //string，成功或者失败说明
    }

### 删除学生/资源 /delete

> 获取学生/资源列表

请求URL：/admin/student/delete   
请求方法：delete  
访问权限：已登录用户  

提交数据：

    {
        studentID:      //int，学生ID
    }

返回数据：

    {
        code:       //int，0代表成功, 非0代表失败
        msg :       //string，成功或者失败说明
    }

## 资源跟进 Fellowup

> 对学生资源的跟进记录

### 跟进列表 /list

请求URL：/admin/fellowup/list   
请求方法：get  
访问权限：已登录用户  

提交参数：

    {
        studentID:      //int，学生ID，必填
    }

返回数据：

    {
        code:       //int，0代表成功, 非0代表失败
        msg :       //string，成功或者失败说明
        data: [
            {
                fellowupID:         //int，跟进记录ID
                staff:              //string，跟进员工姓名
                content:            //string，跟进记录
                createTime:         //int，跟进时间，10位UNIX时间戳，单位s
            },
            {
                ...
            }
        ]
    }

### 添加跟进记录 /add

请求URL：/admin/fellowup/add   
请求方法：post  
访问权限：已登录用户  

提交数据：

    {
        studentID:          //int，跟进记录的学生资源ID，必填
        staffID:            //string，跟进的ID，必填
        content:            //string，跟进记录，必填
    }

返回数据:

    {
        code:       //int，0代表成功, 非0代表失败
        msg :       //string，成功或者失败说明
    }

### 更改跟进记录 /update

请求URL：/admin/fellowup/add   
请求方法：put  
访问权限：已登录用户  

提交数据：

    {
        fellowupID:         //int，跟进记录的ID
        staffID:            //string，跟进的ID
        content:            //string，跟进记录
    }

返回数据:

    {
        code:       //int，0代表成功, 非0代表失败
        msg :       //string，成功或者失败说明
    }

### 删除跟进记录 /delete

请求URL：/admin/fellowup/delete   
请求方法：delete  
访问权限：已登录用户  

提交数据:

    {
        fellowupID:          //int，跟进记录的ID
    }

返回数据:

    {
        code:       //int，0代表成功, 非0代表失败
        msg :       //string，成功或者失败说明
    }

## 约课管理 Appoint

> 公司销售人员对学生资源的约课信息管理

### 约课列表 /list

> 查找某学生资源的约课列表

请求URL：/admin/appoint/list   
请求方法：get  
访问权限：已登录用户  

提交参数：

    {
        studentID:      //int，学生ID，必填
    }

返回数据：

    {
        code:       //int，0代表成功, 非0代表失败
        msg :       //string，成功或者失败说明
        data: [
            {
                appointID:          //int，跟进记录ID
                studentID:          //int，学生ID
                lessionID:          //int，课程ID，试课成功之后算作一次耗课
                teacherID:          //int，约课的教师ID
                teacher:            //string，约课的教师姓名
                subject:            //string，约课科目
                trialDate:          //string，试课日期
                trialTime:          //string，试课时间段，例如：8：00-9：00
                localtion:          //string，约课地点
                matcherID:          //int，匹课员工ID，必填
                matcher:            //string，匹课员工姓名
                state:              //int，1已约课未试课，2已成交，3已放弃试课，4试课未成交
                stateInfo:          //string，状态说明，例如放弃试课说明
                createTime:         //int，创建时间，10位UNIX时间戳，单位s
            },
            {
                ...
            }
        ]
    }

### 添加约课 /add

> 添加某学生资源的约课信息

请求URL：/admin/appoint/add   
请求方法：post  
访问权限：已登录用户  

提交参数：

    {
        studentID:          //int，学生ID，必填
        lessionID:          //int，课程ID，选填，试课成功之后算作一次耗课
        teacherID:          //int，约课的教师ID，必填
        subject:            //string，约课科目，必填
        trialDate:          //string，试课日期，必填
        trialTime:          //string，试课时间段，例如：8：00-9：00，必填
        localtion:          //string，约课地点，必填
        matcherID:          //int，匹课员工ID，必填
    }

返回数据：

    {
        code:       //int，0代表成功, 非0代表失败
        msg :       //string，成功或者失败说明
    }

### 更改约课 /update

> 更改学生资源约课信息记录

请求URL：/admin/appoint/update   
请求方法：put  
访问权限：已登录用户  

提交参数：

    {
        appointID:          //int，约课信息ID
        studentID:          //int，学生ID，必填
        lessionID:          //int，课程ID，选填，试课成功之后算作一次耗课
        teacherID:          //int，约课的教师ID，必填
        subject:            //string，约课科目，必填
        trialDate:          //string，试课日期，必填
        trialTime:          //string，试课时间段，例如：8：00-9：00，必填
        localtion:          //string，约课地点，必填
        matcherID:          //int，匹课员工ID，必填
        state:              //int，1已约课未试课，2已成交，3已放弃试课，4试课未成交
        stateInfo:          //string，状态说明，例如放弃试课说明
    }

返回数据：

    {
        code:       //int，0代表成功, 非0代表失败
        msg :       //string，成功或者失败说明
    }

### 删除约课 /delete

请求URL：/admin/appoint/delete   
请求方法：delete  
访问权限：已登录用户  

提交数据:

    {
        appointID:          //int，约课记录的ID
    }

返回数据:

    {
        code:       //int，0代表成功, 非0代表失败
        msg :       //string，成功或者失败说明
    }

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
        teacherID:      //int，教师ID
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
                    phone:          //string，学生手机号
                    amount:         //int，缴费金额
                    salerID:        //int，销售人员ID
                    saler:          //string，销售人员姓名
                    state:          //int，状态，1已提交未被确认，2已被财务确认，3订单被取消
                    createTime:     //int，订单创建时间，10位UNIX时间戳
                    lesson: [
                        {
                            lessonID:       //int，课程ID
                            name:           //string，教师姓名
                            teachAge:      //int，教龄
                            phone:          //string，联系电话
                            subject:        //string，科目
                            allClass:       //int，所有课时
                            consumedClass:  //int，已消耗的课时
                            preprice:       //int，单课时费
                            classType:      //string，代课类型，一对一
                            state:          //int，课程状态
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
            studentID:      //int，学生ID
            name:           //string，学生姓名
            phone:          //string，学生手机号
            amount:         //int，缴费金额
            saler:          //string，销售者姓名
            state:          //int，状态，1已提交未被确认，2已被财务确认，3订单被取消
            createTime:     //int，订单创建时间，10位UNIX时间戳
            lesson: [
                {
                    lessonID:       //int，课程ID
                    teacherID:       //int，教师ID
                    name:           //string，教师姓名
                    teacheAge:      //int，教龄
                    phone:          //string，联系电话
                    subject:        //string，科目
                    allClass:       //int，所有课时
                    consumedClass:  //int，已消耗的课时数
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

> 添加学生的付款订单

请求URL：/admin/order/add   
请求方法：post  
访问权限：登录用户  

提交数据：

    {
        studentID:      //int，学生ID
        payMethod:      //string，付款方式，现金/微信/支付宝/银行转账/...
        amount:         //int，缴费金额
        salerID:        //int，销售员工ID
        lesson: [       //array，随订单一起确定的课程
            {
                teacherID:       //int，教师ID
                subject:        //string，科目
                allClass:       //int，总课时数
                preprice:       //int，单课时费
                weekClass:      //int，一周上几节课
                firstClassDate: //string，第一次上课日期，2017-3-18
                startDate:      //string，课程开始日期，2017-3-18
                endDate:        //string，课程结束日期，2017-3-18
                classType:      //string，代课类型，一对一
                target:         //string，辅导目标，拔高/冲刺/...
                parentDemand:   //string，家长期望
                studentDemand:  //string，学生期望  
                program:        //string，辅导方案
            },
            {
                ...
            }
        ]
    }
返回数据:

    {
        code:       //int，0代表成功, 非0代表失败
        msg :       //string，成功或者失败说明
    }

### 删除订单 /delete

> 删除订单并删除当前订单的课程

请求URL：/admin/order/delete   
请求方法：delete  
访问权限：销售人员  

提交数据：

    {
        orderID:    //int，订单ID
    }

返回数据:

    {
        code:       //int，0代表成功, 非0代表失败
        msg :       //string，成功或者失败说明
    }

### 财务确认订单 /confirm

> 财务人员确认订单

请求URL：/admin/order/confirm   
请求方法：put  
访问权限：公司财务  

提交数据：

    {
        orderID:    //int，订单ID
    }

返回数据:

    {
        code:       //int，0代表成功, 非0代表失败
        msg :       //string，成功或者失败说明
    }

## 课程 Lesson

### 课程列表 /list

> 根据条件获取信息

请求URL：/admin/lesson/list   
请求方法：get  
访问权限：已登录用户  

提交数据:

    {
        studentID:      //int，学生ID，选填，当与orderID同时出现时以学生ID为主
        orderID:        //int，订单ID，选填
        page:           //int，页码，选填，值为空为第1页
        num:            //int，每页的条数，选填，默认为10
    }

返回数据:

    {
        code:       //int，0代表成功, 非0代表失败
        msg :       //string，成功或者失败说明
        data: [
            {
                lessonID:           //int，课程ID
                orderID:            //int，订单ID
                teacherID:          //int，教师ID
                teacher:            //string，教师姓名
                studentID:          //string，学生姓名
                student:            //string，学生姓名
                subject:            //string，辅导科目
                allClass:           //int，本次课程全部课时数
                consumedClass:      //int，已经消耗的课时数
                weekClass:          //int，一周上几节课
                firstClassDate:     //string，第一天上课的日期
                startDate:          //string，课程开始日期
                endDate:            //string，课程终止日期
                address:            //string，日常上课地点
                type:               //string，本次辅导方式，一对一/...
                target:             //string，辅导目标
                parentDemand:       //string，家长预期
                studentDemand:      //string，学生预期
                program:            //string，辅导方案
                preprice:           //int，一节课单价
                state:              //int，课程状态，1正常上课，2停课状态，3已经结课
                stateInfo:          //string，课程状态说明
                createTime:         //int，创建时间
            },
            {
                ...
            }
        ]
    }

### 课程信息 /info

> 获取某课程详情

请求URL：/admin/lesson/info   
请求方法：get  
访问权限：已登录用户  

提交数据：

    {
        lessonID:    //int，必填，订单ID
    }

返回数据：

    {
        code:       //int，0代表成功, 非0代表失败
        msg :       //string，成功或者失败说明
        data: {
            lessonID:           //int，课程ID
            orderID:            //int，订单ID
            teacherID           //int，教师ID
            teacher:            //string，教师姓名
            studentID:          //string，学生ID
            student:            //string，学生姓名
            subject:            //string，辅导科目
            allClass:           //int，本次课程全部课时数
            consumedClass:      //int，已经消耗的课时数
            weekClass:          //int，一周上几节课
            firstClassDate:     //string，第一天上课的日期
            startDate:          //string，课程开始日期
            endDate:            //string，课程终止日期
            address:            //string，日常上课地点
            type:               //string，本次辅导方式，一对一/...
            target:             //string，辅导目标
            parentDemand:       //string，家长预期
            studentDemand:      //string，学生预期
            program:            //string，辅导方案
            preprice:           //int，一节课单价
            state:              //int，课程状态，1正常上课，2停课状态，3已经结课
            stateInfo:          //string，课程状态说明
            createTime:         //int，创建时间
        }
    }

### 添加课程 /add

> 获取某课程详情

请求URL：/admin/lesson/add   
请求方法：post  
访问权限：已登录用户  

提交数据：

    {
        orderID:            //int，订单ID，必填    
        studentID:          //int，学生ID，必填 
        teacherID:          //int，教师ID，必填 
        subject:            //string，辅导科目，必填 
        allClass:           //int，本次课程全部课时数，必填 
        weekClass:          //int，一周上几节课
        firstClassDate:     //string，第一天上课的日期
        startDate:          //string，课程开始日期
        endDate:            //string，课程终止日期
        address:            //string，日常上课地点
        type:               //string，本次辅导方式，一对一/...
        target:             //string，辅导目标
        parentDemand:       //string，家长预期
        studentDemand:      //string，学生预期
        program:            //string，辅导方案
        preprice:           //int，一节课单价
        stateInfo:          //string，课程状态说明
    }

返回数据：

    {
        code:       //int，0代表成功, 非0代表失败
        msg :       //string，成功或者失败说明
    }

### 更改课程 /update

> 获取某课程详情

请求URL：/admin/lesson/update   
请求方法：put  
访问权限：已登录用户  

提交数据：

    {
        lessonID:           //int，课程ID
        teacherID:          //int，教师ID
        subject:            //string，辅导科目
        allClass:           //int，本次课程全部课时数
        consumedClass:      //int，已经消耗的课时数
        weekClass:          //int，一周上几节课
        firstClassDate:     //string，第一天上课的日期
        startDate:          //string，课程开始日期
        endDate:            //string，课程终止日期
        address:            //string，日常上课地点
        type:               //string，本次辅导方式，一对一/...
        target:             //string，辅导目标
        parentDemand:       //string，家长预期
        studentDemand:      //string，学生预期
        program:            //string，辅导方案
        preprice:           //int，一节课单价
        state:              //int，课程状态，1正常上课，2停课状态，3已经结课
        stateInfo:          //string，课程状态说明
    }

返回数据：

    {
        code:       //int，0代表成功, 非0代表失败
        msg :       //string，成功或者失败说明
    }

### 删除课程 /delete

> 删除订单并删除当前订单的课程

请求URL：/admin/lesson/delete   
请求方法：delete  
访问权限：销售人员  

提交数据：

    {
        lessonID:    //int，课程ID
    }

返回数据:

    {
        code:       //int，0代表成功, 非0代表失败
        msg :       //string，成功或者失败说明
    }