---
title: mongoDB-聚合查询
top: false
cover: false
toc: true
mathjax: true
date: 2020-05-13 08:47:50
password:
summary:
tags: mongoDB
categories: 数据库
---

# mongoDB聚合查询

​	写毕业设计的时候需要实现一个需求，需要根据月份显示订单的数量信息，实现下图的效果

![](Snipaste_2020-05-13_09-08-31.png)

​	上网查了下，需要使用mongoDB的`aggregate()`，聚合操作。

​	官方文档

​		英文：https://docs.mongodb.com/manual/reference/method/db.collection.aggregate/ 

​		中文：https://www.mongodb.org.cn/tutorial/19.html

​	英文文档更加详实。

​	结合了思否上的提问：https://segmentfault.com/q/1010000010073540，写了自己的查询语句

```
//表结构
const productSchema = new mongoose.Schema({
  woId: { type: String, required: true }, // 所属分类的id
  userId: { type: String, required: true },
  createDate: { type: Number, required: true },
  deadline: { type: Number, required: true },
  parentId: { type: String, required: true }, // 所属分类的id
  selfId: { type: String, required: true },
  serviceName: { type: String, required: true },
  cost: { type: Number, required: true }, // 价格 
  address: { type: String, required: true },
  status: { type: Number, default: 0 }, // 商品状态: 0:等待接收 1：订单派送中 3：拒接  3: 订单派送中  4：订单进行中   5 or 6：订单完成(不能操作)
  imgs: { type: Array, default: [] }, // n个图片文件名的json字符串
  detail: { type: String },
  serviceStaffId: { type: String, default: '' }
})
```

```
//查询语句
ProductModel.aggregate([
                { $match: { userId } },
                {
                    $group: {
                        _id: {
                            date: {
                                $month: { 
                                    date: {
                                        '$add': [ 
                                            new Date(0),
                                            '$createDate'
                                        ]
                                    }
                                }
                            },
                            'name': '$serviceName',
                        },
                        count: { $sum: 1 }
                    }
                }, {
                    $group: {
                        _id: '$_id.date',
                        name: {
                            $push: {
                                name: "$_id.name",
                                count: "$count"
                            }
                        }
                    }
                }
            ]).then(data => {
                res.send({ status: 0, data: data })
            }).catch(err => {
                console.log(err)
                res.send({ statu: 1, msg: '请稍后再试' })
            })
```

```
[
  {"_id":7,"name":[{"name":"搬运\\个人搬家","count":1}]},
  {"_id":5,"name":[{"name":"搬运\\公司搬运","count":1},{"name":"室外\\草地修建","count":1},     {"name":"活动策划\\公司团建","count":1},{"name":"搬运\\个人搬家","count":5}]},
  {"_id":8,"name":[{"name":"室内\\办公室打扫","count":1}]}
]
```

​	我的想法是根据表结构的`createDate` 这个字段（字段的类型是number，我将时间转成了毫秒数，这也是个坑），将同一月份的数据统计到一起（聚合）。

​	查询语句这一块`$match`是过滤条件，我是根据用户id查询数据。代码具体的细节我也是一知半解（原谅我的菜），说一下我踩的坑吧：

1. 日期这一块

   ![](Snipaste_2020-05-13_09-56-23.png)

   由于我的`createDate`是number类型，mongoDB不支持将它转成date类型，于是就利用$add操作符，把日期和时间戳相加，得到另一个日期解决。