<div id="top"></div>

<br />
<div align="center">
  <h1 align="center">MongoDB</h1>
</div>

### General

- Maximum size of document is 16MB

### Indexes

- Sort
  ```js
  db.test.createIndex({ a: 1, b: -1 });
  // sort({a: 1, b: -1}) or  sort({a: -1, b: 1}) 👌
  // sort({a: 1, b: 1}) and sort({a: -1, b: -1}) ❌
  ```
- Compound indexes

  - Compound indexes can support queries that match on the prefix of the index fields.

    ```js
    db.products.createIndex({ "item": 1, "location": 1, "stock": 1 } )

    👌 { item: 1 } // prefix
    👌 { item: 1, location: 1 } // prefix
    👌 { item: 1, location: 1, stock: 1 }
    ⚠️ { item: 1, stock: 1 }
    ❌ { location: 1}
    ❌ { stock: 1 }
    ❌ { location: 1, stock: 1 }
    ```

### Aggregate

- All aggregation pipelines are executed sequentially, including `$facet`.

### Project

- The same with `select`

### Lookup

- Is a `left outer join`.
- Use with `$unwind`

  ```js
  // Input
  {
    "item": [
      {
        "name": "Ngoc"
      },
      {
        "name": "Ngoc1"
      }
    ]
  }
  // Aggregate
  db.classes.aggregate([
    {
      $lookup: {
        //...
        as: "item"
      },
    },
    {
      $unwind: "$item"
    }
  ]);

  // Out put
  {
    "item":{
        "name": "Ngoc"
      }
  }

  {
    "item":{
        "name": "Ngoc1"
      }
  }
  ```

- Using The MongoDB Lookup with an Array.

  ```js
  // Input
  db.classes.insertMany([
    {
      _id: 1,
      title: "Reading is ...",
      enrollmentlist: ["giraffe2", "pandabear", "artie"],
      days: ["M", "W", "F"],
    },
    {
      _id: 2,
      title: "But Writing ...",
      enrollmentlist: ["giraffe1", "artie"],
      days: ["T", "F"],
    },
  ]);

  db.members.insertMany([
    { _id: 1, name: "artie", joined: new Date("2016-05-01"), status: "A" },
    { _id: 2, name: "giraffe", joined: new Date("2017-05-01"), status: "D" },
    { _id: 3, name: "giraffe1", joined: new Date("2017-10-01"), status: "A" },
    { _id: 4, name: "panda", joined: new Date("2018-10-11"), status: "A" },
    { _id: 5, name: "pandabear", joined: new Date("2018-12-01"), status: "A" },
    { _id: 6, name: "giraffe2", joined: new Date("2018-12-01"), status: "D" },
  ]);

  // Aggregate
  db.classes.aggregate([
    {
      $lookup: {
        from: "members",
        localField: "enrollmentlist",
        foreignField: "name",
        as: "enrollee_info",
      },
    },
  ]);

  // Output
  {
    "_id" : 1,
    "title" : "Reading is ...",
    "enrollmentlist" : [ "giraffe2", "pandabear", "artie" ],
    "days" : [ "M", "W", "F" ],
    "enrollee_info" : [
        { "_id" : 1, "name" : "artie", "joined" : ISODate("2016-05-01T00:00:00Z"), "status" : "A" },
        { "_id" : 5, "name" : "pandabear", "joined" : ISODate("2018-12-01T00:00:00Z"), "status" : "A" },
        { "_id" : 6, "name" : "giraffe2", "joined" : ISODate("2018-12-01T00:00:00Z"), "status" : "D" }
    ]
  }
  {
    "_id" : 2,
    "title" : "But Writing ...",
    "enrollmentlist" : [ "giraffe1", "artie" ],
    "days" : [ "T", "F" ],
    "enrollee_info" : [
        { "_id" : 1, "name" : "artie", "joined" : ISODate("2016-05-01T00:00:00Z"), "status" : "A" },
        { "_id" : 3, "name" : "giraffe1", "joined" : ISODate("2017-10-01T00:00:00Z"), "status" : "A" }
    ]
  }
  ```

- Using The MongoDB Lookup with $mergerObjects

  ```js
  // Input
  db.orders.insertMany([
    { _id: 1, item: "almonds", price: 12, quantity: 2 },
    { _id: 2, item: "pecans", price: 20, quantity: 1 },
  ]);

  db.items.insertMany([
    { _id: 1, item: "almonds", description: "almond clusters", instock: 120 },
    { _id: 2, item: "bread", description: "raisin and nut bread", instock: 80 },
    { _id: 3, item: "pecans", description: "candied pecans", instock: 60 },
  ]);

  // Aggregate
  db.orders.aggregate([
    {
      $lookup: {
        from: "items",
        localField: "item", // field in the orders collection
        foreignField: "item", // field in the items collection
        as: "fromItems",
      },
    },
    {
      $replaceRoot: {
        newRoot: {
          $mergeObjects: [{ $arrayElemAt: ["$fromItems", 0] }, "$$ROOT"],
        },
      },
    },
    { $project: { fromItems: 0 } },
  ]);

  // Output
  {
    _id: 1,
    item: 'almonds',
    description: 'almond clusters',
    instock: 120,
    price: 12,
    quantity: 2
  },
  {
    _id: 2,
    item: 'pecans',
    description: 'candied pecans',
    instock: 60,
    price: 20,
    quantity: 1
  }
  ```

### Ref

- [Compound Indexes](https://www.mongodb.com/docs/manual/core/index-compound/)
- [MongoDB Lookup Aggregations](https://hevodata.com/learn/mongodb-lookup)
- [Are MongoDB $facet sub-pipelines asynchronous and executed in parallel?](https://www.mongodb.com/community/forums/t/are-mongodb-facet-sub-pipelines-asynchronous-and-executed-in-parallel/9005)
- [Nghệ thuật index mongodb: 5 kế sách có thể các hạ chưa biết](https://viblo.asia/p/nghe-thuat-index-mongodb-5-ke-sach-co-the-cac-ha-chua-biet-Do754bnXZM6)

<p align="right">(<a href="#top">Back to top</a>)</p>
