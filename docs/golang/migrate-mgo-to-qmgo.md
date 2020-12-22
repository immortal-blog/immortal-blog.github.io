# Migrate mgo to qmgo

## What it qmgo

Mgo is a convenient library for golang to operate mongodb, but it does not
support the latest features of mongodb. My old project is written in mgo. If I
want to migrate to the official mongo-driver, its syntax is more primitive, the
usage difference is large, and the migration cost will be higher.  
I found a library called qmgo, which is based on the official mongo-driver
package, but it is closer to mgo in terms of syntax and suitable for simple and
rude migrations.  
Below I will record the methods used in the migration process, which I found out
by myself. If you have any questions, please correct me.

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
