**mongoDB学习笔记**
=====
> ## **1.lean()方法**
lean属性的作用：转换mongoose查询结果类型，从MongooseDocuments转换为JS Object，从而便于我们修改查询结果。

mongoose查询：
```
Model.findOne({});
Model.save();
```
以上2种查询返回的数据实际上是MongooseDocuments对象（mongoose自己封装的一个对象），并且这个对象会对数据进行实时查询以保证其符合预定义的model。所以添加其它model中没有的属性时是无法添加成功的。

要想添加成功有2种方法：

1.查询时添加lean，
```
Model.findOne({}).lean();
Model.findOne({lean:true},function(err,result){});
Model.findOne({}).lean().exec(function(err,result){});
```
2.将查询结果转为object，
```
result.toObject();
```
