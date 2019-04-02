# loopback-sandbox

A repository for reproducing [LoopBack community issues][wiki-issues].

[wiki-issues]: https://github.com/strongloop/loopback/wiki/Reporting-issues

## Steps to reproduce

Having two models: 

`ModelA`
```
{
  "name": "ModelA",
  "base": "PersistedModel",
  "idInjection": true,
  "options": {
    "validateUpsert": true
  },
  "properties": {},
  "validations": [],
  "relations": {},
  "acls": [],
  "methods": {}
}
```

and `ModelB`
```
{
  "name": "ModelB",
  "base": "PersistedModel",
  "idInjection": true,
  "options": {
    "validateUpsert": true
  },
  "properties": {},
  "validations": [],
  "relations": {
    "modelA": {
      "type": "belongsTo",
      "model": "ModelA",
      "foreignKey": "modelaId"
    }
  },
  "acls": [],
  "methods": {}
}
```

Notice that `ModelB`has a **belongsTo** relation with `ModelA`.

1. Create one `ModelA`

```
{
    "_id" : ObjectId("5ca3b7a61076971a4308c4ce")
}
```

2. Create one `ModelB` just setting `modelaId`

```
{
  "modelaId": "5ca3b7a61076971a4308c4ce",
}
```

result will include `ObjectId` which I consider is correct:

```
{
    "_id" : ObjectId("5ca3b7f81076971a4308c4cf"),
    "modelaId" : ObjectId("5ca3b7a61076971a4308c4ce")
}
```

3. Create another `ModelB` but this time including `modelA` related model.

```
{
    "modelaId": "5ca3b7a61076971a4308c4ce",
    "modelA": {
    	"id": "5ca3b7a61076971a4308c4ce"
	}
}
```

the result will have `modelaId` as string wich will have the query filter issue from [#3593](https://github.com/strongloop/loopback/issues/3593)
