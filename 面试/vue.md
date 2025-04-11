# MVC 和 MVVM
## MVC
MVC 是一种传统的软件架构模式，主要用于组织代码，提高可维护性和可扩展性。

### MVC 组成：
* Model（模型）：数据层，负责处理数据逻辑（数据库、API、业务逻辑）。
* View（视图）：UI 层，负责显示数据（HTML、CSS）。
* Controller（控制器）：逻辑层，负责接收用户输入，并更新 Model 或 View。
### MVC 工作流程：
* 用户在 View（界面）上触发操作（如点击按钮）。
* Controller（控制器）接收用户输入，处理逻辑，并更新 Model（数据）。
* Model 更新后，通知 View 进行界面刷新。

或者
1.**用户访问 `/articles` 页面**（输入 URL）。
2.**Controller（`views.py`）接收请求**，查询数据库并获取文章列表。   
3.**Model（`models.py`）执行数据库查询**，返回数据。
4.**Controller 将数据传递给 View**，渲染 `article_list.html` 页面。   
5.**用户在浏览器看到文章列表**。

### MVC 示例（Web 开发）：
假设一个博客系统：

* Model：数据库中的文章数据。
* View：文章页面的 HTML 结构。
* Controller：处理用户请求（例如：点击“发布”按钮后，向数据库写入文章）。

### MVC 的问题：
Controller 代码容易膨胀，逻辑过多时会导致难以维护。
View 依赖 Model，数据变化时需要手动更新 UI。
不适合现代前端开发，前端 MVVM 模型逐渐取代了 MVC

## MVVM
MVVM（Model-View-ViewModel）是一种 前端架构模式，主要用于构建 前端 UI 和数据的分离，常见于 Vue、React 等前端框架。

### MVVM 组成：
* Model（模型）：数据层，负责存储和处理数据（与 MVC 相同）。
* View（视图）：UI 层，负责展示数据（HTML、CSS）。
* ViewModel（视图模型）：核心逻辑层，充当 View 和 Model 之间的桥梁，自动同步数据和视图。
### MVVM 工作流程：
* 用户操作 View（界面）。
* ViewModel 自动监听 View 的变化，并更新 Model。
* Model 变化后，ViewModel 自动同步更新 View，不需要手动 DOM 操作。
### MVVM 的优势：
* ✅ 数据绑定（双向绑定）：

Vue 使用 v-model 让数据和 UI 自动同步，开发者不需要手动更新 DOM。
React 采用单向数据流，但可以用 useState + onChange 实现双向绑定。
* ✅ 解耦 View 和 Model，更清晰的逻辑层。
* ✅ 前端开发更高效，适用于 SPA（单页应用）。


