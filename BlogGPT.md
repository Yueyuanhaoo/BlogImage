如果你想通过纯 JavaScript 动态加载 Markdown 文件并展示内容，可以实现一个通用的方案，支持加载多个 Markdown 文件，动态生成文章列表以及显示内容。以下是完整的扩展实现：

------

### 功能目标

1. 动态读取 Markdown 文件列表。
2. 支持单击文章标题显示文章内容。
3. 使用 `marked.js` 渲染 Markdown 文件为 HTML。

------

### 完整实现代码

#### 1. HTML 结构

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Markdown Blog</title>
    <script src="https://cdn.jsdelivr.net/npm/marked/marked.min.js"></script>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
        }
        header {
            background-color: #007bff;
            color: white;
            padding: 1rem;
            text-align: center;
        }
        #blog-list {
            width: 25%;
            float: left;
            background: #f4f4f4;
            padding: 1rem;
            box-sizing: border-box;
            height: 100vh;
            overflow-y: auto;
        }
        #content {
            width: 75%;
            float: left;
            padding: 1rem;
            box-sizing: border-box;
        }
        .post-title {
            color: #007bff;
            cursor: pointer;
            margin-bottom: 10px;
            display: block;
        }
        .post-title:hover {
            text-decoration: underline;
        }
        .markdown-body {
            line-height: 1.6;
        }
    </style>
</head>
<body>
    <header>
        <h1>Markdown Blog</h1>
    </header>
    <div id="blog-list"></div>
    <div id="content">
        <h2>欢迎来到我的博客</h2>
        <p>点击左侧的文章标题查看内容。</p>
    </div>
    <script src="script.js"></script>
</body>
</html>
```

------

#### 2. JavaScript 文件 (`script.js`)

```javascript
// 博客文章配置
const blogPosts = [
    { title: "第一篇文章", file: "posts/first-post.md" },
    { title: "第二篇文章", file: "posts/second-post.md" },
    { title: "第三篇文章", file: "posts/third-post.md" }
];

// 初始化页面
function initBlog() {
    const blogList = document.getElementById("blog-list");
    const content = document.getElementById("content");

    // 动态生成文章列表
    blogPosts.forEach(post => {
        const postLink = document.createElement("a");
        postLink.textContent = post.title;
        postLink.href = "#";
        postLink.className = "post-title";
        postLink.onclick = () => loadMarkdown(post.file);
        blogList.appendChild(postLink);
    });

    // 加载 Markdown 内容
    async function loadMarkdown(file) {
        try {
            const response = await fetch(file);
            if (!response.ok) throw new Error("文件加载失败");
            const markdown = await response.text();
            content.innerHTML = marked(markdown);
        } catch (error) {
            content.innerHTML = `<p style="color: red;">无法加载文章内容：${error.message}</p>`;
        }
    }
}

// 初始化博客
initBlog();
```

------

#### 3. 示例 Markdown 文件

在项目的 `posts` 文件夹中创建 Markdown 文件，例如 `first-post.md`：

**`posts/first-post.md`**

~~~markdown
# 第一篇文章

欢迎来到我的博客！这是第一篇文章。

## 小节标题
Markdown 是一种轻量级的标记语言，适用于编写简单的文本内容。

- 它支持列表。
- 它支持标题。
- 它支持代码块。

```javascript
console.log("Hello, Markdown!");
~~~

希望你喜欢！

```
---

### 目录结构
确保项目的目录结构如下：


my-blog/ │ ├── index.html        # 主页面 ├── script.js         # JavaScript 逻辑 ├── posts/            # 存放 Markdown 文件 │   ├── first-post.md │   ├── second-post.md │   └── third-post.md ├── styles.css        # （可选）自定义样式表

---

### 功能解析
1. **文章列表动态生成**：
   - 通过 JavaScript 遍历 `blogPosts` 配置，动态生成左侧的文章列表。
   - 每篇文章对应一个超链接，点击时加载文章内容。

2. **Markdown 渲染**：
   - 使用 `marked.js` 将 Markdown 转换为 HTML。
   - 动态更新 `#content` 容器的内容。

3. **异步加载文件**：
   - 使用 `fetch` 异步加载 Markdown 文件，提供良好的用户体验。
   - 错误处理：如果文件未找到或加载失败，显示错误信息。

---

### 扩展功能（可选）
1. **分页功能**：
   - 如果文章很多，可以在 `blogPosts` 中实现分页逻辑，动态加载文章列表。

2. **高亮代码**：
   - 引入 [highlight.js](https://highlightjs.org/) 为代码块添加语法高亮：
     ```html
     <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/highlight.js/11.8.0/styles/default.min.css">
     <script src="https://cdnjs.cloudflare.com/ajax/libs/highlight.js/11.8.0/highlight.min.js"></script>
     <script>
         document.addEventListener('DOMContentLoaded', (event) => {
             document.querySelectorAll('pre code').forEach((el) => {
                 hljs.highlightElement(el);
             });
         });
     </script>
     ```

3. **添加评论功能**：
   - 使用第三方评论工具（如 [Disqus](https://disqus.com/) 或 [Giscus](https://giscus.app/)）在文章内容下添加评论功能。

4. **搜索功能**：
   - 实现简单的搜索框，允许用户根据标题或关键字查找文章。

---

通过这种实现，你可以轻松维护博客内容，只需上传新的 Markdown 文件，无需手动修改 HTML 代码。
```