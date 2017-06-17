## gb-feedback-web

###  获取是否可以提问时间 接口
   path: `/question/canAsk`
   Method: `GET`

###### 返回json
    
    {
        "code":200,
        "msg":{
            "canAsk":true,
            "tip":"当前时间不可提问"
        }
    }
    
#### canAsk: true为可以提问，false为不能提问

## pc-web api

### 获取我的问题列表 接口
   path: `/question/list/my`
   Method: `GET`
   
###### Url参数

	pageNo[必填]:     [int]分页，默认是1
	pageSize[可选]:   [int]最多返回多少数据，默认是10
	beginDate[可选]:  [String]起始日期 比如:"2016-01-01"
	endTime[可选]:    [String]截止日期 比如:"2016-01-01"
    status[可选]:     [int]问题状态   -1:全部(默认值) 0:待跟进,1:跟进中,2:被打回,4:被关闭,5:已解决
	   
   返回Json
   
   	{
        code: 200,
        msg: {
            pageNo: 1,
            totalCount: 10,
            objs: [
                {
                    questionId: 1111,
                    title: "标题",
                    lastReply: "最新的回复",
                    lastReplyTime: "2016-05-10 14:12:30",
                    status: 1,  //0:待跟进,1:跟进中,2:被打回,4:被关闭,5:已解决
                    addTime: "2016-05-10 14:12:30"
                    creator: {
                        userId: 222,
                        name: "灿灿",
                        employeeNumber: "0010303"
                        phone: "138 3838 3838",
                        city: "上海",
                        xu: {
                              id: 1,
                              name: "基础系统技术"
                        },
                        elephantUid: 1111 //大象Uid     
                    }
                },
                {
                    questionId: 1111,
                    title: "标题",
                    lastReply: "最新的回复",
                    lastReplyTime: "2016-05-10 14:12:30",
                    status: 1,  //0:待跟进,1:跟进中,2:被打回,4:被关闭,5:已解决
                    addTime: "2016-05-10 14:12:30"
                    creator: {
                        userId: 222,
                        name: "灿灿",
                        employeeNumber: "0010303"
                        phone: "138 3838 3838",
                        city: "上海",
                        xu: {
                               id: 1,
                               name: "基础系统技术"
                        },
                        elephantUid: 1111 //大象Uid       
                    }
                },
                {
                    questionId: 1111,
                    title: "标题",
                    lastReply: "最新的回复",
                    lastReplyTime: "2016-05-10 14:12:30",
                    status: 1,  //0:待跟进,1:跟进中,2:被打回,4:被关闭,5:已解决
                    addTime: "2016-05-10 14:12:30"
                    creator: {
                        userId: 222,
                        name: "灿灿",
                        employeeNumber: "0010303"
                        phone: "138 3838 3838",
                        city: "上海",
                        xu: {
                                id: 1,
                                name: "基础系统技术"
                        },
                        elephantUid: 1111 //大象Uid
                    }
                }
            ]
        }
    }

    
### 获取分类树 接口
   path: `/type/ask/tree`
   path: `/type/seen/tree`
   Method: `GET`
   
   返回Json
   
   	{
        code: 200,
        msg: 
        [
            {
                typeId: 1,
                parentTypeId: 0,
                typeName: "name-1",
                leadingAnswer: null,
                children: [
                    {
                        typeId: 101,
                        parentTypeId: 1,
                        typeName: "name-101",
                        leadingAnswer: "灿灿灿灿灿灿灿灿灿灿灿灿灿灿灿灿灿灿灿灿灿灿灿灿灿灿灿灿灿灿灿灿灿灿灿灿灿灿灿灿灿灿",
                        children: [ ]
                    },
                    {
                        typeId: 102,
                        parentTypeId: 1,
                        typeName: "name-102",
                        leadingAnswer: "灿灿灿灿灿灿灿灿灿灿灿灿灿灿灿灿灿灿灿灿灿灿灿灿灿灿灿灿灿灿灿灿灿灿灿灿灿灿灿灿灿灿",
                        children: [ ]
                    }
                ]
            },
            
            more....    
        ]
    }
   
       
### 通过问题编号查询 问题详情 接口
   path: `/question/{questionId}/detail`
   Method: `GET`
   
###### Url参数

	questionId[必填]:     [int]问题id
	   
   返回Json
   
   	{
        code: 200,
        msg: {
            questionId: 1,
            typeId: 22222,
            allTypeName: "分类全路径名",
            content: "啊啊啊啊啊 这是内容!!",
            status: 1, //0:待跟进,1:跟进中,2:被打回,4:被关闭,5:已解决
            addTime: "2016-05-10 20:18:43",
            creator: {
                userId: 222,
                name: "灿灿",
                employeeNumber: "0010303"
                phone: "138 3838 3838",
                city: "上海",
                xu: {
                    id: 1,
                    name: "基础系统技术"
                },
                elephantUid: 1111 //大象Uid
            },
            attachments: [
                {
                    fileId: "11111112222",
                    fileName: "测试附件",
                    mimeType: "type",
                    url: "http://apollo.51ping.com/cloud/download/crm/11111112222/ZXhwaXJlZFRpbWU9MTQ2MzEzMDM1MDkxNCZmbj3mtYvor5XpmYTku7Ymaz0vZG93bmxvYWQvY3JtLzExMTExMTEyMjIy.65110aa0cb78fde8baff147a057d34d1",
                    thumbUrl: "http://apollo.51ping.com/cloud/download/crm/11111112222_1/ZXhwaXJlZFRpbWU9MTQ2MzEzMDM1MDkxNCZmbj3mtYvor5XpmYTku7ZfdGh1bWImaz0vZG93bmxvYWQvY3JtLzExMTExMTEyMjIy.df9c21df21acd456bd546652e7969365"
                },
                {
                    fileId: "11111112222",
                    fileName: "测试附件",
                    mimeType: "type",
                    url: "http://apollo.51ping.com/cloud/download/crm/11111112222/ZXhwaXJlZFRpbWU9MTQ2MzEzMDM1MDkxNCZmbj3mtYvor5XpmYTku7Ymaz0vZG93bmxvYWQvY3JtLzExMTExMTEyMjIy.65110aa0cb78fde8baff147a057d34d1",
                    thumbUrl: "http://apollo.51ping.com/cloud/download/crm/11111112222_1/ZXhwaXJlZFRpbWU9MTQ2MzEzMDM1MDkxNSZmbj3mtYvor5XpmYTku7ZfdGh1bWImaz0vZG93bmxvYWQvY3JtLzExMTExMTEyMjIy.5f4ea7ee5c7ad2a3fcdc262f2013df02"
                }
            ],
            businessOwner: {
                userId: 222,
                name: "灿灿",
                employeeNumber: "0010303"
                phone: "138 3838 3838",
                city: "上海",
                xu: {
                    id: 1,
                    name: "基础系统技术"
                },
                elephantUid: 1111 //大象Uid
            },
            operationOwner:{
                userId: 222,
                name: "灿灿",
                employeeNumber: "0010303"
                phone: "138 3838 3838",
                city: "上海",
                xu: {
                    id: 1,
                    name: "基础系统技术"
                },
                elephantUid: 1111 //大象Uid
            }
            currentOwner: {
                userId: 222,
                name: "灿灿",
                employeeNumber: "0010303"
                phone: "138 3838 3838",
                city: "上海",
                xu: {
                    id: 1,
                    name: "基础系统技术"
                },
                elephantUid: 1111 //大象Uid
            },
            lastReplyOwner: {
                userId: 222,
                name: "灿灿",
                employeeNumber: "0010303"
                phone: "138 3838 3838",
                city: "上海",
                xu: {
                    id: 1,
                    name: "基础系统技术"
                },
                elephantUid: 1111 //大象Uid
            },
            replyModelList: [
                {
                    questionId: 1,
                    content: "这里是回复内容",
                    creator: {
                        userId: 222,
                        name: "灿灿",
                        employeeNumber: "0010303"
                        phone: "138 3838 3838",
                        city: "上海",
                        xu: {
                            id: 1,
                            name: "基础系统技术"
                        },
                        elephantUid: 1111 //大象Uid
                    },
                    addTime: "2016-05-10 20:18:43",
                    attachments: [
                        {
                            fileId: "11111112222",
                            fileName: "测试附件",
                            mimeType: "type",
                            url: "http://apollo.51ping.com/cloud/download/crm/11111112222/ZXhwaXJlZFRpbWU9MTQ2MzEzMDM1MDkxNCZmbj3mtYvor5XpmYTku7Ymaz0vZG93bmxvYWQvY3JtLzExMTExMTEyMjIy.65110aa0cb78fde8baff147a057d34d1",
                            thumbUrl: "http://apollo.51ping.com/cloud/download/crm/11111112222_1/ZXhwaXJlZFRpbWU9MTQ2MzEzMDM1MDkxNCZmbj3mtYvor5XpmYTku7ZfdGh1bWImaz0vZG93bmxvYWQvY3JtLzExMTExMTEyMjIy.df9c21df21acd456bd546652e7969365"
                        },
                        {
                            fileId: "11111112222",
                            fileName: "测试附件",
                            mimeType: "type",
                            url: "http://apollo.51ping.com/cloud/download/crm/11111112222/ZXhwaXJlZFRpbWU9MTQ2MzEzMDM1MDkxNCZmbj3mtYvor5XpmYTku7Ymaz0vZG93bmxvYWQvY3JtLzExMTExMTEyMjIy.65110aa0cb78fde8baff147a057d34d1",
                            thumbUrl: "http://apollo.51ping.com/cloud/download/crm/11111112222_1/ZXhwaXJlZFRpbWU9MTQ2MzEzMDM1MDkxNSZmbj3mtYvor5XpmYTku7ZfdGh1bWImaz0vZG93bmxvYWQvY3JtLzExMTExMTEyMjIy.5f4ea7ee5c7ad2a3fcdc262f2013df02"
                        }
                    ],
                },
                {
                    questionId: 1,
                    content: "这里是回复内容",
                    creator: {
                        userId: 222,
                        name: "灿灿",
                        employeeNumber: "0010303"
                        phone: "138 3838 3838",
                        city: "上海",
                        xu: {
                            id: 1,
                            name: "基础系统技术"
                        },
                        elephantUid: 1111 //大象Uid
                    },
                    addTime: "2016-05-10 20:18:43",
                    attachments: [
                        {
                            fileId: "11111112222",
                            fileName: "测试附件",
                            mimeType: "type",
                            url: "http://apollo.51ping.com/cloud/download/crm/11111112222/ZXhwaXJlZFRpbWU9MTQ2MzEzMDM1MDkxNCZmbj3mtYvor5XpmYTku7Ymaz0vZG93bmxvYWQvY3JtLzExMTExMTEyMjIy.65110aa0cb78fde8baff147a057d34d1",
                            thumbUrl: "http://apollo.51ping.com/cloud/download/crm/11111112222_1/ZXhwaXJlZFRpbWU9MTQ2MzEzMDM1MDkxNCZmbj3mtYvor5XpmYTku7ZfdGh1bWImaz0vZG93bmxvYWQvY3JtLzExMTExMTEyMjIy.df9c21df21acd456bd546652e7969365"
                        },
                        {
                            fileId: "11111112222",
                            fileName: "测试附件",
                            mimeType: "type",
                            url: "http://apollo.51ping.com/cloud/download/crm/11111112222/ZXhwaXJlZFRpbWU9MTQ2MzEzMDM1MDkxNCZmbj3mtYvor5XpmYTku7Ymaz0vZG93bmxvYWQvY3JtLzExMTExMTEyMjIy.65110aa0cb78fde8baff147a057d34d1",
                            thumbUrl: "http://apollo.51ping.com/cloud/download/crm/11111112222_1/ZXhwaXJlZFRpbWU9MTQ2MzEzMDM1MDkxNSZmbj3mtYvor5XpmYTku7ZfdGh1bWImaz0vZG93bmxvYWQvY3JtLzExMTExMTEyMjIy.5f4ea7ee5c7ad2a3fcdc262f2013df02"
                        }
                    ],
                },
                
                more......
            ]
        }
    }
    
### 问题详情页 添加回复 接口
    path: `/question/{questionId}/reply/add`
    Method: `POST`
    
###### Url参数
 
 	questionId[必填]:     [int]问题id
  
###### Post Body
    
    {
        content : "啦啦"      //回复内容
        anonymous: 0         //是否匿名,0:不是,1:是
        attachments:[
            {
               fileId : 附件云Id
               fileName : 文件名称
            },
            ......more.......
        ]
    }
 	  
 返回Json
    
    {
         code: 200,
         msg: null
    }   
        
        
         
### 创建问题 接口
   path: `/question/create`
   Method: `POST`
   
###### Post Body

    {
            
            typeId :111  // 分类id
            content : "飞机和第三个发货金色" 
            attachments:[
                {
                   fileId : 附件云Id
                   fileName : 文件名称
                },
                ......more.......
            ]
    }
      
   返回Json
   
    {
        code: 200,
        msg: null
    }     

### 补充问题接口（新）

    path:`/question/supplementNew?questionId=xx&typeId=XX`
    method:post

###### url参数

    questionId[必填]:     [int]问题id
    typeId[选填]:     [int]改变问题分类的分类ID

###### postBody

    {
       
      
       content : "飞机和第三个发货金色" 
       attachments:[
           {
              fileId : 附件云Id
              fileName : 文件名称
           },
           ......more.......
       ] 
    }
    

###### 返回json

    {
    "code":200,
    "msg":[
        {
            "id":1,
            "questionId":4010,
            "content":"重新打开了",
            "creator":{
                "userId":6076,
                "name":"洪国钦",
                "ad":"guoqin.hong",
                "employeeNumber":"0020260",
                "phone":"13000000000",
                "email":"mtwxl01@163.com",
                "city":"上海",
                "xu":{
                    "id":6,
                    "name":"品牌战略事业部"
                },
                "elephantUid":610203,
                "organizationId":100089,
                "organizationName":"战略合作BD",
                "organizationFullName":"集团\到店餐饮事业群\品牌战略事业部\战略大客户\战略合作BD",
                "loginId":1085945,
                "anonymous":false,
                "sales":true
            },
            "status":1,
            "addTime":"2016-07-18 12:50:46",
            "updateTime":"2016-07-18 12:50:46",
            "attachments":[

            ]
        }
    ]
    }

                 
### 获取补充问题接口

    path:`/question/{questionId}/supplement`
    method:get

###### url参数

    questionId[必填]:     [int]问题id

###### 返回json

    {
    "code":200,
    "msg":[
        {
            "id":1,
            "questionId":4010,
            "content":"重新打开了",
            "creator":{
                "userId":6076,
                "name":"洪国钦",
                "ad":"guoqin.hong",
                "employeeNumber":"0020260",
                "phone":"13000000000",
                "email":"mtwxl01@163.com",
                "city":"上海",
                "xu":{
                    "id":6,
                    "name":"品牌战略事业部"
                },
                "elephantUid":610203,
                "organizationId":100089,
                "organizationName":"战略合作BD",
                "organizationFullName":"集团\到店餐饮事业群\品牌战略事业部\战略大客户\战略合作BD",
                "loginId":1085945,
                "anonymous":false,
                "sales":true
            },
            "status":1,
            "addTime":"2016-07-18 12:50:46",
            "updateTime":"2016-07-18 12:50:46",
            "attachments":[

            ]
        }
    ]
    }

### 修改问题 状态 接口
   path: `/question/{questionId}/status`
   Method: `POST`
   
###### Url参数

    questionId[必填]:     [int]问题id
    status[选填]  :       [int] 0:待跟进,1:跟进中,2:被打回,4:被关闭,5:已解决
    
###### Post Body (选填)
    
    {
        content : "啦啦"      //回复内容
        anonymous: 0         //是否匿名,0:不是,1:是
        attachments:[
            {
               fileId : 附件云Id
               fileName : 文件名称
            },
            ......more.......
        ]
    }
          
   返回Json
   
    {
        code: 200,
        msg: null
    }           


### 修改问题 分类 接口
   path: `/question/{questionId}/type`
   Method: `PUT`
   
###### Url参数

    questionId[必填]:     [int]问题id
    typeId[必填]  :       [int] 分类id
    
      
   返回Json
   
    {
        code: 200,
        msg: null
    }           


### 管理页面问题列表 接口
   path: `/question/search`
   Method: `GET`
   
###### Url参数

    typeIds[可选]     [array]分类ids
    buIds[可选]       [array]BU ids     
    questionId[可选]  [int]问题Id  
    creatorId[可选]   [int]提问人UserId
    beginTime[可选]   [String]开始时间 "2016-01-01 12:00"
    endTime[可选]     [String]结束时间 "2017-01-01 12:00"
    businessOwnerId[可选]  [int]业务负责人UserId  
    currentOwnerId[可选]   [int]当前责任人UserId
	pageNo[必填]:     [int]分页，默认是1
	pageSize[可选]:   [int]最多返回多少数据，默认是10
    status[可选]:     [int]问题状态   -1:全部(默认值) 0:待跟进,1:跟进中,2:被打回,4:被关闭,5:已解决
	   
   返回Json
   
   	{
        code: 200,
        msg: {
            pageNo: 1,
            totalCount: 10,
            objs: [
                {
                    questionId: 1,
                    typeId: 22222,
                    allTypeName: "分类全路径名",
                    content: "啊啊啊啊啊 这是内容!!",
                    status: 1,
                    addTime: "2016-05-10 20:18:43",
                    creator: {
                        userId: 222,
                        name: "灿灿",
                        employeeNumber: "0010303"
                        phone: "138 3838 3838",
                        city: "上海",
                        xu: {
                            id: 1,
                            name: "基础系统技术"
                        },
                        elephantUid: 1111 //大象Uid
                    },
                    businessOwner: {
                        userId: 222,
                        name: "灿灿",
                        employeeNumber: "0010303"
                        phone: "138 3838 3838",
                        city: "上海",
                        xu: {
                            id: 1,
                            name: "基础系统技术"
                        },
                        elephantUid: 1111 //大象Uid
                    },
                    currentOwner: {
                        userId: 222,
                        name: "灿灿",
                        employeeNumber: "0010303"
                        phone: "138 3838 3838",
                        city: "上海",
                        xu: {
                            id: 1,
                            name: "基础系统技术"
                        },
                        elephantUid: 1111 //大象Uid
                    }
                },
                more.......
            ]
        }
    }


### 获取所有Bu信息 接口
   path: `/xu/all`
   Method: `GET`
   
#### 返回Json
   
    {
        code: 200,
        msg: [
           {
                id: 1,
                name:交易后台
           },
           {
                id: 2,
                name: 结婚
           },
           more.....
        ]
    }

### 获取所有Bu信息 接口
   path: `/xu/xg`
   Method: `GET`
   
#### 返回Json
   
    {
        "code":200,
        "msg":[
            {
                "typeId":1,
                "parentTypeId":0,
                "typeName":"到店餐饮事业部",
                "leadingAnswer":null,
                "level":1,
                "firstOwnerId":0,
                "secondOwnerId":0,
                "children":[
                    {
                        "typeId":1,
                        "parentTypeId":1,
                        "typeName":"餐饮推广事业部",
                        "leadingAnswer":null,
                        "level":2,
                        "firstOwnerId":0,
                        "secondOwnerId":0,
                        "children":[
    
                        ],
                        "leaf":true
                    },
                    {
                        "typeId":2,
                        "parentTypeId":1,
                        "typeName":"北中国事业部",
                        "leadingAnswer":null,
                        "level":2,
                        "firstOwnerId":0,
                        "secondOwnerId":0,
                        "children":[
    
                        ],
                        "leaf":true
                    },
                    {
                        "typeId":3,
                        "parentTypeId":1,
                        "typeName":"CBD事业部",
                        "leadingAnswer":null,
                        "level":2,
                        "firstOwnerId":0,
                        "secondOwnerId":0,
                        "children":[
    
                        ],
                        "leaf":true
                    },
                    {
                        "typeId":4,
                        "parentTypeId":1,
                        "typeName":"南中国事业部",
                        "leadingAnswer":null,
                        "level":2,
                        "firstOwnerId":0,
                        "secondOwnerId":0,
                        "children":[
    
                        ],
                        "leaf":true
                    },
                    {
                        "typeId":5,
                        "parentTypeId":1,
                        "typeName":"大客户部",
                        "leadingAnswer":null,
                        "level":2,
                        "firstOwnerId":0,
                        "secondOwnerId":0,
                        "children":[
    
                        ],
                        "leaf":true
                    },
                    {
                        "typeId":6,
                        "parentTypeId":1,
                        "typeName":"品牌战略事业部",
                        "leadingAnswer":null,
                        "level":2,
                        "firstOwnerId":0,
                        "secondOwnerId":0,
                        "children":[
    
                        ],
                        "leaf":true
                    },
                    {
                        "typeId":7,
                        "parentTypeId":1,
                        "typeName":"到餐其他",
                        "leadingAnswer":null,
                        "level":2,
                        "firstOwnerId":0,
                        "secondOwnerId":0,
                        "children":[
    
                        ],
                        "leaf":true
                    }
                ],
                "leaf":false
            },
            {
                "typeId":2,
                "parentTypeId":0,
                "typeName":"到店综合事业部",
                "leadingAnswer":null,
                "level":1,
                "firstOwnerId":0,
                "secondOwnerId":0,
                "children":[
                    {
                        "typeId":8,
                        "parentTypeId":2,
                        "typeName":"家装事业部",
                        "leadingAnswer":null,
                        "level":2,
                        "firstOwnerId":0,
                        "secondOwnerId":0,
                        "children":[
    
                        ],
                        "leaf":true
                    },
                    {
                        "typeId":9,
                        "parentTypeId":2,
                        "typeName":"结婚事业部",
                        "leadingAnswer":null,
                        "level":2,
                        "firstOwnerId":0,
                        "secondOwnerId":0,
                        "children":[
    
                        ],
                        "leaf":true
                    },
                    {
                        "typeId":10,
                        "parentTypeId":2,
                        "typeName":"丽人事业部",
                        "leadingAnswer":null,
                        "level":2,
                        "firstOwnerId":0,
                        "secondOwnerId":0,
                        "children":[
    
                        ],
                        "leaf":true
                    },
                    {
                        "typeId":11,
                        "parentTypeId":2,
                        "typeName":"亲子事业部",
                        "leadingAnswer":null,
                        "level":2,
                        "firstOwnerId":0,
                        "secondOwnerId":0,
                        "children":[
    
                        ],
                        "leaf":true
                    },
                    {
                        "typeId":13,
                        "parentTypeId":2,
                        "typeName":"教育培训事业部",
                        "leadingAnswer":null,
                        "level":2,
                        "firstOwnerId":0,
                        "secondOwnerId":0,
                        "children":[
    
                        ],
                        "leaf":true
                    },
                    {
                        "typeId":14,
                        "parentTypeId":2,
                        "typeName":"休闲娱乐事业部",
                        "leadingAnswer":null,
                        "level":2,
                        "firstOwnerId":0,
                        "secondOwnerId":0,
                        "children":[
    
                        ],
                        "leaf":true
                    },
                    {
                        "typeId":15,
                        "parentTypeId":2,
                        "typeName":"KTV事业部",
                        "leadingAnswer":null,
                        "level":2,
                        "firstOwnerId":0,
                        "secondOwnerId":0,
                        "children":[
    
                        ],
                        "leaf":true
                    }
                ],
                "leaf":false
            }
        ]
    }

### 获取某分类所有兄弟节点的业务责任人包括本节点业务责任人 接口
       path: `/type/{typeId}/businessOwners`
       Method: `GET`

#### 返回Json

        {
            code: 200,
            msg: [
                         {
                                typeId:111,
                                typeName:"cancan",
                                   {
                                       userId: 222,
                                       name: "灿灿",
                                       employeeNumber: "0010303"
                                       phone: "138 3838 3838",
                                       city: "上海",
                                       xu: {
                                           id: 1,
                                           name: "基础系统技术"
                                       },
                                       elephantUid: 1111 //大象Uid
                                   }
                         },
                         {      typeId:111,
                                typeName:"cancan",
                                   {
                                       userId: 222,
                                       name: "灿灿",
                                       employeeNumber: "0010303"
                                       phone: "138 3838 3838",
                                       city: "上海",
                                       xu: {
                                           id: 1,
                                           name: "基础系统技术"
                                       },
                                       elephantUid: 1111 //大象Uid
                                   }
                         },
               more.....
            ]
        }

### 转签责任人接口
           path: `/question/{questionId}/owner/{userId}`
           Method: `POST`

###### Url参数

        questionId[必填]  [int] 用户Id
        userId[必填]      [int] 用户Id

###### Post Body (选填)
    
    {
        content : "啦啦"      //回复内容
        anonymous: 0         //是否匿名,0:不是,1:是
        attachments:[
            {
               fileId : 附件云Id
               fileName : 文件名称
            },
            ......more.......
        ]
    }        


#### 返回Json

            {
                code: 200,
                msg: null
            }


### 模糊搜索用户接口
           path: `/gbUser/search`
           Method: `GET`

###### Url参数

        limit[选填]  [int] 结果限制数
        offset[选填]   [int] 结果偏移查询量
        keyword[选填]   ［String］关键字


#### 返回Json

            "code":200,
            "msg":[
            {
            "userId":62240,
            "name":"宋亚飞",
            "employeeNumber":"0001255",
            "phone":"13000000000",
            "email":"yafei.song@dianping.com",
            "city":"上海",
            "xu":{
            "id":28,
            "name":"MT到店"
            },
            "elephantUid":616641,
            "organizationId":100623,
            "organizationName":"销售四组"
            }
            ]
            }
            
            

### 页面打点接口
           path: `/log/record`
           Method: `POST`

###### Post Body
    
        {
            actionType[必填]  [String] 埋点类型(比如,页面埋点/功能埋点/外链埋点等)
            item[必填]        [String] 具体埋点的元素信息
        }

#### 返回Json

        {
            code: 200,
            msg: null
        }

### 页面打点接口(批量)
           path: `/log/records`
           Method: `POST`

###### Post Body
        
        [
            {
                actionType[必填]  [String] 埋点类型(比如,页面埋点/功能埋点/外链埋点等)
                item[必填]        [String] 具体埋点的元素信息
            },
            {
                actionType[必填]  [String] 埋点类型(比如,页面埋点/功能埋点/外链埋点等)
                item[必填]        [String] 具体埋点的元素信息
            }
        ]

#### 返回Json

        {
            code: 200,
            msg: null
        }





### 分类模板接口
           path: `/type/${typeId}/template`
           Method: `GET`

#### 返回Json

        {
            code: 200,
            msg: [
                    {
                        typeId: 5555552,
                        fieldKey: "问题描述",
                        fieldValue: "灿灿是模板",
                        required: 0,
                        validRegex: "测试",
                        sequence: 1
                    }
                ]
        }

## 问题打分接口
   url:`/question/{questionId}/score`
   method:`post`
### url参数  
   questionId:`问题Id`
   requestBody:`{
                 		"questionId":1,
                 		"creator":{
                     		"userId":9980,
                     		"name":"华贝",
                     		"employeeNumber":"0015420",
                     		"phone":"13000000000",
                     		"email":"bei.hua@dianping.com",
                     		"city":"上海",
                     		"xu":null,
                     		"elephantUid":612277,
                     		"organizationId":101965,
                     		"organizationName":"基础系统技术组"
                 		},
                 		"score":4,
                 		"content":"不错",
                		"addTime":"2016-05-31 16:00:41",
                 		"updateTime":"2016-05-31 16:00:41"
                }`
### 返回内容
   responseBody:`{
                 "code":200,
                 "msg":{
                 "questionId":1,
                 "creator":{
                 "userId":9980,
                 "name":"华贝",
                 "employeeNumber":"0015420",
                 "phone":"13000000000",
                 "email":"bei.hua@dianping.com",
                 "city":"上海",
                 "xu":null,
                 "elephantUid":612277,
                 "organizationId":101965,
                 "organizationName":"基础系统技术组"
                 },
                 "score":4,
                 "content":"不错",
                 "addTime":"2016-05-31 16:00:41",
                 "updateTime":"2016-05-31 16:00:41"
                 }
                 }`

## 公告接口
   url:`notice`
   method:`get`
### 返回内容
   responseBody:`{
                 code: 200,
                 msg: {
                 notice: "内容让肖老师配置"
                 }
                 }`

### 问题历史接口
   url:`/question/{questionId}/history`
   method:`get`

##### url参数
   questionId:`问题Id`
   显示效果：六个字段拼接即可
   requestBody:`{
                code: 200,
                msg: [
                    {
                        date: "2016-07-29 18:45:18",
                        operator: "闫福华",
                        operate: "创建了问题",
                        oldvalue: "",
                        to: "",
                        newvalue: ""
                    },
                    {
                        date: "2016-07-29 18:45:18",
                        operator: "系统",
                        operate: "分配给业务处理人",
                        oldvalue: "",
                        to: "",
                        newvalue: "『赵东雪』"
                    },
                    {
                        date: "2016-08-01 15:41:09",
                        operator: "系统",
                        operate: "分配给业务处理人",
                        oldvalue: "",
                        to: "",
                        newvalue: "『李泽敏』"
                    }
                ]
                }

### 绑定问题标签 接口
      path: `/question/tag/modify`
      Method: `POST`

##### Post Body

       {
           "questionId":4368,
           "addIds":[130],
           "deleteIds":[123]
       }

####返回Json

       {
           code: 200,
           msg: null
       }

### 管理标签 接口
         path: `/question/tag/manage`
         Method: `POST`

##### Post Body

          {
          	  "questionCreatorId":2,
              "tagNames":["chenyun"],
              "deleteIds":[123]
          }

#####返回Json

          {
              code: 200,
              msg: null
          }

### 问题标签列表 接口
      path:`/question/tag/{questionId}/list`
      Method:`get`
##### url参数
      questionId:`问题Id`
      requestBody:{
                  code: 200,
                  msg: [
                      {
                          id: 142,
                          questionId: 4368,
                          tagId: 132,
                          tagName: "ee",
                          creator: 17896,
                          status: 1,
                          addTime: "2016-08-25 15:35:32",
                          updateTime: "2016-08-25 15:35:32"
                      },
                      {
                          id: 175,
                          questionId: 4368,
                          tagId: 133,
                          tagName: "AAA",
                          creator: 1,
                          status: 1,
                          addTime: "2016-08-26 10:03:44",
                          updateTime: "2016-08-26 10:03:44"
                      }
                      ]
                   }

### 标签列表 接口
         url:`/question/tag/list`
         例：question/tag/list?userId=2
         method:`get`
##### url参数
         userId:`提问人userId`
         {
            code: 200,
            msg: [
                {
                    id: 130,
                    tagName: "eee",
                    creator: 0,
                    addTime: "2016-08-25 15:33:52",
                    updateTime: "2016-08-25 16:09:21"
                },
                {
                    id: 232,
                    tagName: "chenyun2",
                    creator: 36822,
                    addTime: "2016-09-06 16:11:10",
                    updateTime: "2016-09-06 16:11:10"
                }
                ]
         }