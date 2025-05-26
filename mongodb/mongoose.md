Mongoose 是 Node.js 的 MongoDB ODM（对象文档模型），提供了 schema-based 数据建模方式。以下是 Mongoose 的基本操作：

# 1. 安装 Mongoose
```
npm install mongoose
```
# 2. 连接数据库
```
const mongoose = require('mongoose');

mongoose.connect('mongodb://localhost:27017/mydatabase', {
    useNewUrlParser: true,
    useUnifiedTopology: true
}).then(() => console.log('数据库连接成功'))
  .catch(err => console.error('数据库连接失败', err));
```
# 3. 定义 Schema 和 Model
```
const userSchema = new mongoose.Schema({
    name: String,
    age: Number,
    email: { type: String, unique: true }
});

const User = mongoose.model('User', userSchema);
```
# 4. 增加数据（Create）
```
const addUser = async () => {
    const user = new User({ name: '张三', age: 25, email: 'zhangsan@example.com' });
    await user.save();
    console.log('用户已添加:', user);
};

addUser();
```
# 1. 查询数据（Read）
```
const findUsers = async () => {
    const users = await User.find(); // 查询所有用户
    console.log(users);
};

findUsers();
```
```
按条件查询：
const findUser = async () => {
    const user = await User.findOne({ name: '张三' });
    console.log(user);
};

findUser();
```
# 6. 更新数据（Update）
```
const updateUser = async () => {
    const user = await User.findOneAndUpdate(
        { name: '张三' }, 
        { age: 26 }, 
        { new: true }
    );
    console.log('更新后的用户:', user);
};

updateUser();
```
# 7. 删除数据（Delete）
```
const deleteUser = async () => {
    const result = await User.deleteOne({ name: '张三' });
    console.log('删除结果:', result);
};

deleteUser();
```
# 8. 关闭数据库连接
```
mongoose.connection.close();
```
这些是 Mongoose 的基础操作，适用于常见的增删改查需求。如果有更复杂的需求，比如分页、索引、关联查询等，可以使用 Mongoose 提供的更高级 API。