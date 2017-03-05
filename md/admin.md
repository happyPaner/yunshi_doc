# 后台管理系统API文档

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
                name:       //string，学生姓名
                phone:      //string，学生手机号
                grade:      //string，学生年级
            }
        ]
    }