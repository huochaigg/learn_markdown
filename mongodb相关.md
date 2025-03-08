# mongodb 相关
## 安装
```
brew tap mongodb/brew
brew install mongodb-community@5.0
```

## 启动
```
brew services start mongodb/brew/mongodb-community
```

## mongodb写入安全级别
https://www.cnblogs.com/phpfans/p/4852808.html


## 以下是一些常见的 MongoDB 数据库操作指令。这些指令可以通过 MongoDB Shell 或命令行工具进行操作。

### 1. 创建/切换数据库
在 MongoDB 中，数据库会在插入数据时自动创建，如果数据库不存在。使用 use 命令切换到一个数据库，若该数据库不存在，则会创建它。

```
use mydatabase
```

### 2. 查看当前数据库
查看当前正在使用的数据库。
```
db
```

### 3. 查看所有数据库
查看 MongoDB 中的所有数据库。
```
show databases
```

### 4. 创建/切换集合
使用 **`db.createCollection()`** 显式创建一个集合。如果集合已经存在，则不会重新创建。
```
db.createCollection("users")
```
或者，直接插入文档时，MongoDB 会自动创建集合。
```
db.users.insertOne({ name: "Alice", age: 25 })
```
### 5. 查看当前数据库的所有集合
查看当前数据库中的所有集合。
```
show collections
```
### 6. 插入文档
插入一个文档：
```
db.users.insertOne({ name: "Alice", age: 25 })
```
插入多个文档：
```
db.users.insertMany([
  { name: "Bob", age: 30 },
  { name: "Charlie", age: 35 }
])
```
### 7. 查询文档
查询所有文档：
```
db.users.find()
```
查询特定条件的文档：
```
db.users.find({ age: { $gt: 30 } })
```
查询特定字段：
```
db.users.find({ name: "Alice" }, { age: 1 })
```
限制查询结果的数量：
```
db.users.find().limit(5)
```
### 8. 更新文档
更新一个文档：
```
db.users.updateOne(
  { name: "Alice" },
  { $set: { age: 26 } }
)
```
更新多个文档：
```
db.users.updateMany(
  { age: { $lt: 30 } },
  { $set: { status: "young" } }
)
```
#### 常见操作符：
#### **1.修改字段值**
#### $set：设置字段的值（如果字段不存在，则新增）
```
db.users.updateOne({ name: "Alice" }, { $set: { age: 26 } })
```
#### $unset：删除字段
```
db.users.updateOne({ name: "Alice" }, { $unset: { age: "" } })
```
#### $rename：重命名字段
```
db.users.updateOne({ name: "Alice" }, { $rename: { "age": "years_old" } })
```
#### **2. 数值运算**
#### $inc：递增或递减数值字段
```
db.users.updateOne({ name: "Alice" }, { $inc: { age: 1 } })  // age +1
```
```
db.users.updateOne({ name: "Alice" }, { $inc: { age: -1 } }) // age -1
```
#### $mul：按倍数增加数值
```
db.users.updateOne({ name: "Alice" }, { $mul: { salary: 1.1 } }) // 工资增加 10%
```
#### $min：只有当新值小于当前值时才更新
简称至多为
```
db.users.updateOne({ name: "Alice" }, { $min: { age: 18 } })
```
#### $max：只有当新值大于当前值时才更新
简称至少为
```
db.users.updateOne({ name: "Alice" }, { $max: { age: 30 } })
```
**示例 1：更新 age 但仅在新值更小时**
```
db.users.updateOne(
  { name: "Alice" },
  { $min: { age: 20 } }
)
```
**解释**
* 如果 Alice 的 age 目前是 25，则 age 会更新为 20（因为 20 小于 25）。
* 如果 Alice 的 age 目前是 18，则 age 保持 18（因为 20 大于 18，不更新）。

**示例 2：仅当 price 降低时更新**
```
db.products.updateOne(
  { product: "Laptop" },
  { $min: { price: 800 } }
)
```
**解释**

* 如果 Laptop 的 price 目前是 1000，则更新为 800（因为 800 < 1000）。
* 如果 Laptop 的 price 目前是 750，则保持不变（因为 800 > 750）。


#### **3. 数组操作**
#### $push：向数组添加一个元素
```
db.users.updateOne({ name: "Alice" }, { $push: { hobbies: "swimming" } })
```
#### $addToSet：向数组添加一个唯一值（如果该值已存在，则不会添加）
```
db.users.updateOne({ name: "Alice" }, { $addToSet: { hobbies: "reading" } })
```
#### $pop：从数组的 开头（-1）或结尾（1） 删除一个元素
```
db.users.updateOne({ name: "Alice" }, { $pop: { hobbies: -1 } }) // 删除第一个元素
```
```
db.users.updateOne({ name: "Alice" }, { $pop: { hobbies: 1 } })  // 删除最后一个元素
```
#### $pull：从数组中删除指定元素
```
db.users.updateOne({ name: "Alice" }, { $pull: { hobbies: "swimming" } })
```
#### $pullAll：从数组中删除多个指定元素
```
db.users.updateOne({ name: "Alice" }, { $pullAll: { hobbies: ["swimming", "reading"] } })
```
#### **4. 字段合并**
#### $currentDate：将字段值设置为当前日期/时间
```
db.users.updateOne({ name: "Alice" }, { $currentDate: { lastUpdated: true } })
```
#### $bit：对数值字段执行按位运算
```
db.users.updateOne({ name: "Alice" }, { $bit: { age: { and: 5 } } }) // 按位与
```
#### **5. 条件更新**
#### $setOnInsert：仅在 插入（文档不存在时）时设置字段（通常与 upsert: true 一起使用）
```
db.users.updateOne(
  { name: "Alice" },
  { $setOnInsert: { age: 25, city: "New York" } },
  { upsert: true }
)
```
#### $cond（在 update 语句中使用 $set 搭配 $cond）
```
db.users.updateOne(
  { name: "Alice" },
  { $set: { age: { $cond: { if: { $gte: ["$age", 30] }, then: 30, else: "$age" } } } }
)
```
##### **✅ `$cond` 的两种语法**

`$cond` 在 MongoDB 中有 **两种写法**：

1.  **对象语法（标准写法）**：
    
    ```
    {
      $cond: {
        if: <条件>,
        then: <true时的值>,
        else: <false时的值>
      }
    }
    ```
    
    ✅ **适用于清晰的 `if-then-else` 逻辑**
    
2.  **数组语法（简写）**：
    
    ```
    { $cond: [ <条件>, <true时的值>, <false时的值> ] }
    ```
    
    ✅ **适用于更简洁的表达**
    

* * *

##### **🚀 解析你的 `$cond` 语法**

你的 `aggregate` 语句：

```markdown
db.orders.aggregate([
  {
    $addFields: {
      discountedPrice: {
        $expr: {
          $cond: [
            { $gt: ["$discount", 0.2] },
            { $multiply: ["$price", { $subtract: [1, "$discount"] }] },
            "$price"
          ]
        }
      }
    }
  }
]);
```

##### **🔥 这里的 `$cond` 用的是** **数组语法**：

```markdown
$cond: [
  条件,       // $gt: ["$discount", 0.2]  --> 如果 discount > 0.2
  真值,       // $multiply: ["$price", { $subtract: [1, "$discount"] }]  --> 计算折扣价
  假值        // "$price"  --> 否则，价格不变
]
```

等价于：

```markdown
$cond: {
  if: { $gt: ["$discount", 0.2] },
  then: { $multiply: ["$price", { $subtract: [1, "$discount"] }] },
  else: "$price"
}
```

**💡 由于 `$cond` 语法允许使用数组的方式表达 `if-then-else`，所以没有 `if, then, else` 关键字，但逻辑是一样的。**

* * *

##### **🚀 什么时候用对象语法，什么时候用数组语法？**

| **语法** | **适用场景** | **优点** | **缺点** |
| --- | --- | --- | --- |
| **对象语法 `{ if, then, else }`** | **逻辑清晰、可读性强** | 适合复杂 `if-then-else` 结构 | 代码稍长 |
| **数组语法 `[ 条件, 真值, 假值 ]`** | **表达式简单时，代码更短** | 代码更紧凑 | 可能可读性较差 |

📌 **如果 `if-else` 逻辑比较复杂，建议用对象语法（清晰）。** 📌 **如果 `if-else` 逻辑很简单，数组语法更简洁。**

* * *

##### **🚀 再来一个示例**

假设你有一个 `students` 集合，字段如下：

```markdown
[
  { "_id": 1, "name": "Alice", "score": 85 },
  { "_id": 2, "name": "Bob", "score": 40 },
  { "_id": 3, "name": "Charlie", "score": 75 }
]
```

如果 **分数 ≥ 60**，则标记 `"Pass"`，否则标记 `"Fail"`。

##### **✅ 用对象语法**

```markdown
db.students.aggregate([
  {
    $project: {
      name: 1,
      score: 1,
      result: {
        $cond: {
          if: { $gte: ["$score", 60] },
          then: "Pass",
          else: "Fail"
        }
      }
    }
  }
]);
```

##### **✅ 用数组语法**

```markdown
db.students.aggregate([
  {
    $project: {
      name: 1,
      score: 1,
      result: { $cond: [ { $gte: ["$score", 60] }, "Pass", "Fail" ] }
    }
  }
]);
```
这个操作的意思是：如果 age 大于等于 30，则将其设为 30，否则保持不变。
总结
| 操作符 | 作用 |
|---|---|
| $set | 设置或新增字段 |
| $unset | 删除字段 |
| $rename | 重命名字段 |
| $inc | 递增/递减数值 |
| $mul | 按倍数增加数值 |
| $min/$max | 仅在新值更小时/更大时更新 |
| $push | 向数组添加元素 |
| $addToSet | 向数组添加唯一值 |
| $pop | 从数组的开头/结尾删除元素 |
| $pull | 从数组中删除指定元素 |
| $pullAll | 从数组中删除多个指定元素 |
| $currentDate | 设置当前时间 |
| $bit | 按位运算 |
| $setOnInsert | 仅在插入文档时设置字段（适用于 upsert: true） |

这些操作符可以组合使用，以满足复杂的更新需求。你可以根据自己的具体业务逻辑选择合适的更新方式。

### 9. 删除文档
删除一个文档：
```
db.users.deleteOne({ name: "Alice" })
```
删除多个文档：
```
db.users.deleteMany({ age: { $gt: 30 } })
```
### 10. 聚合查询 (Aggregation)
使用聚合进行复杂的查询和处理：
```
db.users.aggregate([
  { $match: { age: { $gt: 20 } } },
  { $group: { _id: "$age", total: { $sum: 1 } } }
])
```

#### **常见的聚合操作阶段**
#### $match
用于筛选文档，类似于 find 操作符，但它是在管道中的一个阶段。通常在管道的开头使用，以减少数据量。
```
db.collection.aggregate([
  { $match: { status: 'active' } }
]);
```
#### $group
按照指定的字段分组，并可以对每组数据进行聚合操作，如求和、平均数、最大值、最小值等。
```
db.collection.aggregate([
  { $group: { _id: "$category", total: { $sum: "$amount" } } }
]);
```
#### $project
用于重塑文档，选择、排除或计算新的字段。它常用于修改输出格式或生成新的计算字段。
```
db.collection.aggregate([
  { $project: { name: 1, age: 1, yearOfBirth: { $subtract: [2025, "$age"] } } }
]);
```
#### $sort
用于对结果进行排序，类似于 find().sort()。排序按指定字段升序或降序排列。
```
db.collection.aggregate([
  { $sort: { age: -1 } }  // 按年龄降序
]);
```
#### $limit
限制返回的文档数量，类似于 find().limit()。
```
db.collection.aggregate([
  { $limit: 5 }
]);
```
#### $skip
跳过指定数量的文档，通常与 $limit 一起使用进行分页。
```
db.collection.aggregate([
  { $skip: 10 },
  { $limit: 5 }
]);
```
#### $unwind
用于“展开”数组字段，将数组中的每个元素展开成独立的文档，通常用于处理数组字段。
```
db.collection.aggregate([
  { $unwind: "$items" }
]);
```
#### $lookup
用于进行集合之间的联接（类似 SQL 的 JOIN 操作）。可以从另一个集合中引入相关文档。
```
db.orders.aggregate([
  { $lookup: {
      from: "products",
      localField: "productId",
      foreignField: "_id",
      as: "productDetails"
    }
  }
]);
```
#### $addFields
向文档中添加或修改字段，或者使用现有字段计算新字段。
```
db.collection.aggregate([
  { $addFields: { totalAmount: { $multiply: ["$quantity", "$price"] } } }
]);
```
#### $count
用于计算文档的数量，并将结果以文档形式返回，字段名默认为 count。
```
db.collection.aggregate([
  { $match: { status: "active" } },
  { $count: "activeUsers" }
]);
```
**聚合管道例子**
假设我们有一个订单集合，每个文档包含订单的详细信息。我们希望计算每个客户的总购买金额，并排序。
```
db.orders.aggregate([
  { $group: { _id: "$customerId", totalSpent: { $sum: "$amount" } } },
  { $sort: { totalSpent: -1 } }
]);
```
#### **高级聚合操作**
#### $facet
允许在一个聚合管道中执行多个不同的聚合操作，每个子管道独立执行。
```
db.sales.aggregate([
  { $facet: {
      "total_sales": [{ $group: { _id: null, total: { $sum: "$amount" } } }],
      "avg_sales": [{ $group: { _id: null, avg: { $avg: "$amount" } } }]
    }
  }
]);
```
#### $bucket
用于将数据按指定的分段范围进行分桶操作（类似于 SQL 的分组）。
```
db.sales.aggregate([
  { $bucket: {
      groupBy: "$amount",
      boundaries: [0, 100, 500, 1000, 5000],
      default: "Other",
      output: { "count": { $sum: 1 } }
    }
  }
]);
```
#### $sample
从集合中随机选择指定数量的文档。
```
db.collection.aggregate([
  { $sample: { size: 3 } }
]);
```

#### **📌 常见 MongoDB 表达式操作符**

#### **🔹 1. 比较操作符**

| 操作符 | 作用 | 示例 |
| --- | --- | --- |
| `$eq` | 等于 | `{ $expr: { $eq: ["$price", 100] } }` |
| `$ne` | 不等于 | `{ $expr: { $ne: ["$status", "completed"] } }` |
| `$gt` | 大于 | `{ $expr: { $gt: ["$amount", 500] } }` |
| `$lt` | 小于 | `{ $expr: { $lt: ["$stock", 10] } }` |
| `$gte` | 大于等于 | `{ $expr: { $gte: ["$age", 18] } }` |
| `$lte` | 小于等于 | `{ $expr: { $lte: ["$discount", 0.2] } }` |

#### **🔹 2. 逻辑操作符**

| 操作符 | 作用 | 示例 |
| --- | --- | --- |
| `$and` | 逻辑 AND | `{ $expr: { $and: [{ $gte: ["$price", 100] }, { $lt: ["$price", 500] }] } }` |
| `$or` | 逻辑 OR | `{ $expr: { $or: [{ $eq: ["$status", "completed"] }, { $eq: ["$status", "shipped"] }] } }` |
| `$not` | 逻辑 NOT | `{ $expr: { $not: [{ $eq: ["$status", "pending"] }] } }` |

#### **🔹 3. 算术操作符**

| 操作符 | 作用 | 示例 |
| --- | --- | --- |
| `$add` | 加法 | `{ $expr: { $add: ["$price", "$tax"] } }` |
| `$subtract` | 减法 | `{ $expr: { $subtract: ["$total", "$discount"] } }` |
| `$multiply` | 乘法 | `{ $expr: { $multiply: ["$price", "$quantity"] } }` |
| `$divide` | 除法 | `{ $expr: { $divide: ["$total", "$quantity"] } }` |
| `$mod` | 取模 | `{ $expr: { $mod: ["$stock", 2] } }` |

#### **🔹 4. 字符串操作符**

| 操作符 | 作用 | 示例 |
| --- | --- | --- |
| `$concat` | 连接字符串 | `{ $expr: { $concat: ["$firstName", " ", "$lastName"] } }` |
| `$substr` | 截取字符串 | `{ $expr: { $substr: ["$title", 0, 5] } }` |
| `$toLower` | 转换为小写 | `{ $expr: { $toLower: "$name" } }` |
| `$toUpper` | 转换为大写 | `{ $expr: { $toUpper: "$name" } }` |

#### **🔹 5. 日期操作符**

| 操作符 | 作用 | 示例 |
| --- | --- | --- |
| `$dateToString` | 格式化日期 | `{ $expr: { $dateToString: { format: "%Y-%m-%d", date: "$createdAt" } } }` |
| `$year` | 获取年份 | `{ $expr: { $year: "$createdAt" } }` |
| `$month` | 获取月份 | `{ $expr: { $month: "$createdAt" } }` |
| `$dayOfMonth` | 获取日期 | `{ $expr: { $dayOfMonth: "$createdAt" } }` |
| `$hour` | 获取小时 | `{ $expr: { $hour: "$createdAt" } }` |
| `$minute` | 获取分钟 | `{ $expr: { $minute: "$createdAt" } }` |

#### **🔹 6. 数组操作符**

| 操作符 | 作用 | 示例 |
| --- | --- | --- |
| `$size` | 获取数组长度 | `{ $expr: { $size: "$tags" } }` |
| `$arrayElemAt` | 获取指定索引的元素 | `{ $expr: { $arrayElemAt: ["$items", 0] } }` |
| `$in` | 判断值是否在数组内 | `{ $expr: { $in: ["shipped", "$statusHistory"] } }` |

#### **🔹 7. 类型转换操作符**

| 操作符 | 作用 | 示例 |
| --- | --- | --- |
| `$toInt` | 转换为整数 | `{ $expr: { $toInt: "$price" } }` |
| `$toDouble` | 转换为双精度浮点数 | `{ $expr: { $toDouble: "$price" } }` |
| `$toString` | 转换为字符串 | `{ $expr: { $toString: "$orderId" } }` |

* * *
#### **📌 `$expr` 可以用于哪些操作？**

`$expr` 主要用于支持 **聚合表达式** 在以下 MongoDB 操作中：

| 操作 | 作用 | 示例 |
| --- | --- | --- |
| `$match` | 在 `aggregate` 管道中过滤数据 | `{ $match: { $expr: { $gt: ["$price", 100] } } }` |
| `find` | 在 `find` 查询中过滤数据 | `db.products.find({ $expr: { $gt: ["$price", 100] } })` |
| `$lookup` | 在 `pipeline` 中使用变量 | `{ $lookup: { from: "orders", let: { userIdVar: "$userId" }, pipeline: [{ $match: { $expr: { $eq: ["$customerId", "$$userIdVar"] } } }] } }` |
| `$set` / `$addFields` | 计算新字段 | `{ $set: { discountPrice: { $multiply: ["$price", 0.9] } } }` |
| `$project` | 计算并返回新字段 | `{ $project: { total: { $add: ["$price", "$tax"] } } }` |
| `$redact` | 基于逻辑条件筛选文档 | `{ $redact: { $cond: { if: { $gte: ["$sensitive", true] }, then: "$$PRUNE", else: "$$KEEP" } } }` |

#### **📌 示例：$expr 在 `find` 和 `aggregate` 中的应用**

##### **🔹 1. `find` 查询**

`$expr` 允许在 `find` 里执行比较操作：

```markdown
db.orders.find({
  $expr: { $gt: ["$amount", 50] }
});
```

🔹 **等价于：**

```markdown
SELECT * FROM orders WHERE amount > 50;
```

##### **🔹 2. `aggregate` 查询**

在 `aggregate` 中 `$match` 结合 `$expr` 使用：

```markdown
db.orders.aggregate([
  { $match: { $expr: { $gte: ["$amount", 50] } } }
]);
```

🔹 **等价于：**

```markdown
SELECT * FROM orders WHERE amount >= 50;
```

* * *

##### **📌 结论**

1.  **`$expr` 可以用于：**
    
    +   `$match`
    +   `find`
    +   `$lookup`
    +   `$set` / `$addFields`
    +   `$project`
    +   `$redact`
2.  **常见操作符分类：**
    
    +   **比较操作符** (`$eq`, `$gt`, `$lt` 等)
    +   **逻辑操作符** (`$and`, `$or`, `$not`)
    +   **算术操作符** (`$add`, `$subtract`, `$multiply` 等)
    +   **字符串操作符** (`$concat`, `$toLower`, `$toUpper` 等)
    +   **日期操作符** (`$year`, `$month`, `$dateToString` 等)
    +   **数组操作符** (`$size`, `$arrayElemAt`, `$in` 等)
    +   **类型转换** (`$toInt`, `$toString`)

🚀 **最终结论：**  
✅ `$expr` 允许 **在查询中使用表达式**，比普通 `$match` 更灵活，类似 SQL 的 `WHERE` 条件，特别适用于 `$lookup` 里的 `pipeline` 查询。

#### 聚合性能优化
使用 $match 和 $sort 时的顺序：通常建议将 $match 放在管道的开头，以减少数据量，然后再进行 $sort 操作。
避免过多的 $unwind：如果数组很大，展开操作可能会生成大量文档，导致性能问题。尽量避免不必要的 $unwind。
索引优化：确保对常用的查询字段（例如 $match 和 $sort 中使用的字段）创建索引，以提高查询性能。


### 11. 创建索引
为了加速查询，可以创建索引。

创建单字段索引：
```
db.users.createIndex({ name: 1 })
```
创建复合索引：
```
db.users.createIndex({ name: 1, age: -1 })
```
### 12. 删除集合
删除集合及其所有数据。

```
db.users.drop()
```
### 13. 删除数据库
删除整个数据库及其所有集合。
```
db.dropDatabase()
```
### 14. 查看索引
查看集合的所有索引。
```
db.users.getIndexes()
```
### 15. 删除索引
删除某个索引。
```
db.users.dropIndex({ name: 1 })
```
这些是 MongoDB 的一些常用指令，涵盖了基本的数据库操作、数据操作以及集合和索引管理等内容。

[跳转](./mongoose.md)