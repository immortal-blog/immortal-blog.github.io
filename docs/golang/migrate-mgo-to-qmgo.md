# Migrate mgo to qmgo

## Why migrate to qmgo

- Mgo is a convenient library for golang to operate mongodb, but it does not support the latest features of mongodb,
  such as Transaction.
- My old project is written in mgo. If I want to migrate to the official mongo-driver, its syntax is more primitive, the
  usage difference is large, and the migration cost will be higher.
- I found a library called qmgo, which is based on the official mongo-driver package, but it is closer to mgo in terms
  of syntax and suitable for simple and rude migrations.

Below I will record the methods used in the migration process, which I found out by myself. If you have any questions,
please correct me.

## Batch replacement

- Batch replace `github.com/globalsign/mgo/bson` with `go.mongodb.org/mongo-driver/bson`
- Batch replace `bson.NewObjectId()` with `primitive.NewObjectID()`
- Batch replace `bson.ObjectId` with `primitive.ObjectID`
- Batch replace `.Find(bson.M` with `.Find(c, bson.M`
- Batch replace `.Find(search` to `.Find(c, search`
- Batch replace `.Insert(` to `.InsertOne(c,`
- Batch replace `.Update(` to `.UpdateOne(c,`
- Batch replace `.RemoveAll(` to `.RemoveAll(c,`
- Batch replace `.Remove(` to `.Remove(c,`
- Batch replace `errors.Is(err, mgo.ErrNotFound)` with `qmgo.IsErrNoDocuments(err)`
- Batch replace `.EnsureIndex(mgo.Index{` for
  `.CreateOneIndex(c, options.IndexModel{`

## Copy and paste

- Where there is no context, copy and paste everywhere `c := context.Background()`
- If it is in the interface method of gin, you can directly use gin's `c *gin.Context` (but if multi-coroutine operation
  needs to Copy gin's Context)

## Function substitution

- Replace the bson.IsObjectIdHex() method:

```
func IsObjectIDHex(hex string) bool {
	_, err := primitive.ObjectIDFromHex(hex)
	if err != nil {
		return false
	}
	return true
}
```

- Replace the bson.ObjectIdHex() method:

```
func ObjectIDHex(hex string) primitive.ObjectID {
	id, err := primitive.ObjectIDFromHex(hex)
	if err != nil {
		panic(fmt.Sprintf("Invalid input to ObjectIDHex: %+v", hex))
	}
	return id
}
```

These functions can be placed in the package of the auxiliary method or the package of the database operation, and then
replace the references in batches.

## Summary

At this point, the project completed the migration within two hours and started running smoothly.  
Thanks to the author of qmgo for making the wheels for me.   
If I have the opportunity, I will also open up some wheels.
