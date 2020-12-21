## 前言

mgo 是一个方便的 golang 操作 mongodb 的库，但是它对 mongodb 的最新特性的支持不足。  
偏偏老项目是用 mgo 写的，如果要迁移到官方的 mongo-driver，官方的 mongo-driver 的封装更原始，用法差异较大，迁移成本会比较高。这时候我发现了一个 qmgo 库，是基于官方 mongo-driver
的封装，但是语法上更接近 mgo，适合我进行简单粗暴的迁移。  
下面我记录一下迁移过程中使用的方法，是我自己摸索出来的，如果有问题欢迎指正。

## 批量替换大法

- 批量替换 **github.com/globalsign/mgo/bson** 为 **go.mongodb.org/mongo-driver/bson**
- 批量替换 **bson.NewObjectId()** 为 **primitive.NewObjectID()**
- 批量替换 **bson.ObjectId** 为 **primitive.ObjectID**
- 批量替换 **.Find(bson.M** 为 **.Find(c, bson.M**
- 批量替换 **.Find(search** 为 **.Find(c, search**
- 批量替换 **.Insert(** 为 **.InsertOne(c,**
- 批量替换 **.Update(** 为 **.UpdateOne(c,**
- 批量替换 **.RemoveAll(** 为 **.RemoveAll(c,**
- 批量替换 **.Remove(** 为 **.Remove(c,**
- 批量替换 **errors.Is(err, mgo.ErrNotFound)** 为 **qmgo.IsErrNoDocuments(err)**
- 批量替换 **.EnsureIndex(mgo.Index{** 为
  **.CreateOneIndex(c, options.IndexModel{**

## 复制粘贴大法

- 没有 context 的地方，到处复制粘贴 c := context.Background()
- 如果是 gin 的接口方法内，可以直接用 gin 的 c \*gin.Context（但是如果多协程操作需要 Copy 一下 gin 的 Context）

## 函数替代大法

- 替代 bson.IsObjectIdHex()方法：

```
//判断是不是ObjectID的Hex
func IsObjectIDHex(hex string) bool {
	_, err := primitive.ObjectIDFromHex(hex)
	if err != nil {
		return false
	}
	return true
}
```

- 替代 bson.ObjectIdHex()方法：

```
//转换string为primitive.ObjectID
func ObjectIDHex(hex string) primitive.ObjectID {
	id, err := primitive.ObjectIDFromHex(hex)
	if err != nil {
		panic(fmt.Sprintf("Invalid input to ObjectIDHex: %+v", hex))
	}
	return id
}
```

这些函数可以直接找个辅助方法的包，或者自己的数据库的包，放在里面，然后用批量替换大法，就解决了。

## 总结

至此，项目在两小时内完成了迁移，并且顺利跑起来了。  
感谢 qmgo 的作者，替我做好了轮子。  
有机会我也做一些轮子开源出来。
