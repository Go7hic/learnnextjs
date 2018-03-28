# learnnextjs
参考翻译 https://learnnextjs.com


Next.js是一个基于React的一个服务端渲染简约框架。它使用React语法，可以很好的实现代码的模块化，有利于代码的开发和维护。

Next.js带来了很多好的特性：

- 默认服务端渲染模式，以文件系统为基础的客户端路由

- 代码自动分隔使页面加载更快

- （以页面为基础的）简洁的客户端路由

- 以webpack的热替换为基础的开发环境

- 使用React的JSX和ES6的module，模块化和维护更方便

- 可以运行在Express和其他Node.js的HTTP 服务器上

- 可以定制化专属的babel和webpack配置

# 基础

## 开始

##### 安装设置
在命令行工具中依次执行下面语句：

// 在本地创建一个项目跟目录
```
$ mkdir hello-next 
```

// 切换到项目根目录
```
$ cd hello-next
```
// 用npm初始化项目
```
$ npm init -y
```

// 将react和next安装到本地依赖
```
$ npm install --save react react-dom next
```

// 创建文件夹 pages
```
$ mkdir pages
```
创建完文件夹之后，打开hello-next文件下的package.json文件，在 scripts 下添加一个script，如下：
```
{
  "scripts": {
    "dev": "next"
  }
}
```
至此，项目准备工作完成，运行下面这条命令开启项目服务器：
```
$ npm run dev
```
##### 404
命令执行完毕后，在浏览器中打开 http://localhost:3000，你会看到这样的页面：
![https://cloud.githubusercontent.com/assets/50838/24114002/ac4786f2-0dc4-11e7-8d84-b6f33c8f9aae.png](https://cloud.githubusercontent.com/assets/50838/24114002/ac4786f2-0dc4-11e7-8d84-b6f33c8f9aae.png)
这个页面是Next.js默认生成的404页面，而开启服务后访问的之所以是404页面，是因为我们还没有设置项目主页。

##### 创建第一个页面
Next.js是从服务器生成页面，再返回给前端展示。Next.js默认从 pages 目录下取页面进行渲染返回给前端展示，并默认取 pages/index.js 作为系统的首页进行展示。注意，pages 是默认存放页面的目录，路由的根路径也是pages目录。

在 pages/index.js 中创建一个React函数式组件：
```
const Index = () => (
  <div>
    <p>Hello Next.js</p>
  </div>
)

export default Index
```
Next.js默认使用Webpack构建项目，webpack的热部署功能一样能提升开发效率。创建完 pages/index.js 后，再访问 http://localhost:3000 即可看到设置好的页面。

##### Handling Errors 错误处理

默认情况下,next.js会跟踪这些错误并在浏览器中显示出来。这可以帮助您识别错误并快速修复它们。

一旦您修复了问题，页面将会立即出现，不会重新刷新加载。
## 页面跳转
现在我们知道如何创建一个简单的Next.js应用程序并运行它。我们的简单应用只有一个页面，但我们可以添加任意数量的页面。例如，我们可以通过向pages / about.js添加以下内容来创建“关于”页面：
```
export default () => (
  <div>
    <p>This is the about page</p>
  </div>
)
```
然后，我们可以通过http//localhost：3000/about 访问该页面

之后，我们需要连接这些页面。我们可以使用HTML“a”标签。但是，它不会执行客户端导航;它通过服务器导航到页面，这不是我们想要的。

为了支持客户端导航，我们需要使用Next.js的Link API，它通过 `next/link` 导出.

让我们看看怎么使用它：

##### 设置
为了跟上这一课，你需要有一个简单的Next.js应用程序。为此，请继续上一课的工作或下载以下示例应用程序:

```
git clone https://github.com/arunoda/learnnextjs-demo.git
cd learnnextjs-demo
git checkout getting-started
```

安装运行项目:
```
npm install
npm run dev
```

##### Using Link

现在我们将使用 `next/link`来链接我们的两个页面。 
将下面的代码添加到`pages/index.js`中.

```
// This is the Link API
import Link from 'next/link'

const Index = () => (
  <div>
    <Link href="/about">
      <a>About Page</a>
    </Link>
    <p>Hello Next.js</p>
  </div>
)

export default Index
```
在这里，我们已经导入 `next/link`  作为链接并使用它 像这样的：
```
<Link href="/about">
  <a>About Page</a>
</Link>
```
现在尝试打开 http://localhost:3000/

然后点击“about page”链接。它会导航到“关于”页面。
##### Client-Side History Support

当您点击“后退”按钮时，它将完全通过客户端将页面导航到索引页面; `next/link`会为您提供所有location.history 处理。您甚至不需要写一行客户端路由代码。
##### Styling a Link

大多数时候，我们可能想要设计我们的链接。我们可以这么做的：
```
<Link href="/about">
  <a style={{ fontSize: 20 }}>About Page</a>
</Link>
```
一旦我们添加了这个，你可以看到正确应用的样式。

但是下面这样做会出现什么问题呢：
```
<Link href="/about" style={{ fontSize: 20 }}>
  <a>About Page</a>
</Link>
```

实际上，next/link 的样式属性不起作用。这是因为 next/link只是一个只接受“href”和一些其他路由相关属性的包装组件。如果您需要对其进行设计，则需要对底层组件执行此操作

##### Link With a Button

假设我们需要为链接使用“button”而不是锚点。然后我们需要像这样编辑我们的导航代码:
```
<Link href="/about">
  <button>Go to About Page</button>
</Link>
```

就像一个button 一样，您可以将任何自定义的React组件或甚至一个div放置在链接中。 组件放置在链接中的唯一要求是它们应该接受onClick属性。
##### Link is Simple, but Powerful
在这里我们只查看了next / link的基本用法。有一些有趣的使用方法，我们将在即将到来的课程中了解它们。 同时，查看Next.js路由文档https://github.com/zeit/next.js#routing 。你会发现它很有用

## 组件共用

我们知道Next.js是关于页面的。我们可以通过导出React组件来创建页面，并将该组件放入pages目录中。然后它将有一个基于文件名的固定URL。

组件的设置跟React一样，通过export导出，通过import导入。

##### 安装设置
在本课中，我们需要一个简单的Next.js应用程序来玩。尝试下载以下示例应用程序：
```
git clone https://github.com/arunoda/learnnextjs-demo.git
cd learnnextjs-demo
git checkout navigate-between-pages
```
运行
```
npm install
npm run dev
```
##### 创建一个 header 组件

下面在 components 目录下，创建一个公用组件 Header，用于各个文件的头部导航，通过导航可以在页面见切换。
```
import Link from 'next/link'

const linkStyle = {
  marginRight: 15
}

const Header = () => (
  <div>
    <Link href="/">
      <a style={linkStyle}>Home</a>
    </Link>
    <Link href="/about">
      <a style={linkStyle}>About</a>
    </Link>
  </div>
)

export default Header
```
接下来，让我们导入这个组件并将它用在我们的页面中。对于index.js页面，它将如下所示：
```
import Header from '../components/Header'

export default () => (
  <div>
    <Header />
    <p>Hello Next.js</p>
  </div>
)
```

您也可以为about.js页面执行相同的操作.
此时，如果您在http：// localhost：3000 /上导航到您的应用程序，您将能够看到新的标题并在页面之间导航。
![https://cloud.githubusercontent.com/assets/50838/24333679/fa856f00-1279-11e7-931d-a5707e51a801.gif](https://cloud.githubusercontent.com/assets/50838/24333679/fa856f00-1279-11e7-931d-a5707e51a801.gif)

##### 组件目录
我们不需要将我们的组件放在一个特殊的目录中;该目录可以被命名为任何东西。唯一的特殊目录是pages目录。

您甚至可以在pages目录中创建Component。

这里我们没有这样做，因为我们不需要直接的URL到我们的Header组件。

##### 布局组件

在我们的应用中，我们将在各种页面中使用通用样式。为此，我们可以创建一个通用的布局组件，并将其用于每个页面。这是一个例子。

将这些内容添加到components/MyLayout.js中:

```
import Header from './Header'

const layoutStyle = {
  margin: 20,
  padding: 20,
  border: '1px solid #DDD'
}

const Layout = (props) => (
  <div style={layoutStyle}>
    <Header />
    {props.children}
  </div>
)

export default Layout
```
一旦我们完成了，我们可以在我们的页面中使用这个布局，如下所示:

```
mport Layout from '../components/MyLayout.js'

export default () => (
    <Layout>
       <p>This is the about page</p>
    </Layout>
)
```

#####  渲染子组件

如果删除{props.children}，布局将无法渲染我们放入Layout元素的内容，如下所示:
```
export default () => (
    <Layout>
       <p>This is the about page</p>
    </Layout>
)
```
这只是创建布局组件的一种方法。以下是创建Layout组件的其他一些方法:

```
import withLayout from '../lib/layout'

const Page = () => (
  <p>This is the about page</p>
)

export default withLayout(Page)

```
```
const Page = () => (
  <p>This is the about page</p>
)

export default () => (<Layout page={Page}/>)
```

```
const content = (<p>This is the about page</p>)

export default () => (<Layout content={content}/>)
```


## 创建动态页面

现在我们知道如何用多个页面创建一个基本的Next.js应用程序。为了创建一个页面，我们必须在磁盘上创建一个实际的文件。

但是，在真实应用程序中，我们需要动态创建页面以显示动态内容。有很多方法可以通过Next.js来实现。

我们开始使用 query strings 创建动态页面。

我们将创建一个简单的博客应用程序。在首页上显示所有文章的列表。

![https://cloud.githubusercontent.com/assets/50838/24542722/600b9ce8-161a-11e7-9f1d-7ed08ff394fd.png](https://cloud.githubusercontent.com/assets/50838/24542722/600b9ce8-161a-11e7-9f1d-7ed08ff394fd.png)

当您点击帖子标题时，您可以在自己的视图中看到单个帖子

![https://cloud.githubusercontent.com/assets/50838/24542721/5fdd9c26-161a-11e7-9b10-296d4cb6912d.png](https://cloud.githubusercontent.com/assets/50838/24542721/5fdd9c26-161a-11e7-9b10-296d4cb6912d.png)

##### 安装设置

首先，我们需要一个简单的Next.js应用程序来玩。尝试下载以下示例应用程序：
```
git clone https://github.com/arunoda/learnnextjs-demo.git
cd learnnextjs-demo
git checkout using-shared-components
```
运行：
```
npm install
npm run dev
```

添加此内容后，您将看到如下所示的网页:
![https://cloud.githubusercontent.com/assets/50838/24542722/600b9ce8-161a-11e7-9f1d-7ed08ff394fd.png](https://cloud.githubusercontent.com/assets/50838/24542722/600b9ce8-161a-11e7-9f1d-7ed08ff394fd.png)

接下来，点击第一个链接。你会得到一个404页面。

##### 通过  Query Strings 传递数据

我们通过查询字符串参数（查询参数）传递数据。在我们的例子中，它是“标题”查询参数。我们使用我们的PostLink组件执行此操作，如下所示
```
const PostLink = (props) => (
  <li>
    <Link href={`/post?title=${props.title}`}>
      <a>{props.title}</a>
    </Link>
  </li>
)
```

就这样，您可以通过查询字符串传递任何类型的数据。

###### 创建“post”页面
现在我们需要创建帖子页面来显示博客文章。为了做到这一点，我们需要从查询字符串中获取标题。让我们看看如何做到这一点.创建一个名为pages / post.js的文件并添加以下内容：
```
import Layout from '../components/MyLayout.js'

export default (props) => (
    <Layout>
       <h1>{props.url.query.title}</h1>
       <p>This is the blog post content.</p>
    </Layout>
)
```
现在打开页面就长这样：
![https://cloud.githubusercontent.com/assets/50838/24542721/5fdd9c26-161a-11e7-9b10-296d4cb6912d.png](https://cloud.githubusercontent.com/assets/50838/24542721/5fdd9c26-161a-11e7-9b10-296d4cb6912d.png)

下面是上述代码发生的事：

- 每个页面都会得到一个名为“URL”的道具，其中包含一些与当前URL相关的细节
- 在这种情况下，我们使用具有查询字符串参数的“查询”对象
- 因此，我们通过props.url.query.title获得标题。

让我们对我们的应用程序做一个简单的修改。将“pages / post.js”的内容替换为以下内容:
```
import Layout from '../components/MyLayout.js'

const Content = (props) => (
  <div>
    <h1>{props.url.query.title}</h1>
    <p>This is the blog post content.</p>
  </div>
)

export default () => (
    <Layout>
       <Content />
    </Layout>
)
```

##### 特殊属性 'url'
正如你所看到的那样，上面这段代码会抛出这样的错误:
![https://cloud.githubusercontent.com/assets/50838/24542720/5fd985a0-161a-11e7-8971-bc677906b1bf.png](https://cloud.githubusercontent.com/assets/50838/24542720/5fd985a0-161a-11e7-8971-bc677906b1bf.png)

这是因为，url prop只会暴露给页面的主要组件。这不是暴露在页面中使用的其他组件。但是，如果你需要，你可以像这样传递它:
```
export default (props) => (
    <Layout>
       <Content url={props.url} />
    </Layout>
)
```
现在我们已经学会了如何使用查询字符串来创建动态页面。这只是开。

动态页面可能需要更多信息才能呈现，而我们可能无法通过查询字符串将其全部传递。或者我们可能希望具有这样的明确网址：http//localhost：3000/blog/ hello-nextjs

## 用Route Masking 清理URL

在前一课中，我们学习了如何使用查询字符串创建动态页面。 有了这个，我们的一篇博客文章的链接如下所示：
```
http://localhost:3000/post?title=Hello%20Next.js
```
但是这网址看起来不太好看。 如果我们能g改成下面这样呢？
```
http://localhost:3000/p/hello-nextjs
```
![https://cloud.githubusercontent.com/assets/50838/24586820/b65be244-17c6-11e7-87fd-d6880152261e.png](https://cloud.githubusercontent.com/assets/50838/24586820/b65be244-17c6-11e7-87fd-d6880152261e.png)

##### 安装设置

```
git clone https://github.com/arunoda/learnnextjs-demo.git
cd learnnextjs-demo
git checkout create-dynamic-pages
```

```
npm install
npm run dev
```
###### Route Masking

这里，我们将使用称为 Route Masking 的 Next.js的独特功能。基本上，它会在浏览器上显示不同于您应用看到的实际网址的网址。

让我们将路由掩码添加到我们的博客文章URL。

对pages/index.js使用以下代码：
```
import Layout from '../components/MyLayout.js'
import Link from 'next/link'

const PostLink = (props) => (
  <li>
    <Link as={`/p/${props.id}`} href={`/post?title=${props.title}`}>
      <a>{props.title}</a>
    </Link>
  </li>
)

export default () => (
  <Layout>
    <h1>My Blog</h1>
    <ul>
      <PostLink id="hello-nextjs" title="Hello Next.js"/>
      <PostLink id="learn-nextjs" title="Learn Next.js is awesome"/>
      <PostLink id="deploy-nextjs" title="Deploy apps with Zeit"/>
    </ul>
  </Layout>
)
```

看看下面的代码块:
```
const PostLink = (props) => (
  <li>
    <Link as={`/p/${props.id}`} href={`/post?title=${props.title}`}>
      <a>{props.title}</a>
    </Link>
  </li>
)
```
在<Link>元素中，我们使用了另一个名为“as”的属性。这是我们需要在浏览器上显示的网址。您的应用看到的URL在“href”属性中提及。
  
##### History Awareness

正如您所见，路由屏蔽与浏览器历史记录非常吻合。 您只需为链接添加“as”道具即可。
##### Reload
点击打打开第一篇文章的页面，然后刷新。出现了一个404错误。这是因为在服务器上没有加载这样的页面。 服务器将尝试加载页面p/hello-nextjs，但我们只有两个页面：index.js和 post.js。

有了这个问题，我们不能在生产中运行这个应用程序。我们需要解决这个问题。


## 服务端支持Route Masking

在上一课中，我们学习了如何为我们的应用程序创建干净的URL。基本上，我们可以拥有这样的网址：

http://localhost:3000/p/my-blog-post

但它只适用于客户端导航。当我们重新加载页面时，它给了我们一个404页面。 

这是因为在pages目录中没有名为 p/my-blog-post 的实际页面。

![https://cloud.githubusercontent.com/assets/50838/24919433/475417bc-1f01-11e7-9fae-a17030d3d051.png](https://cloud.githubusercontent.com/assets/50838/24919433/475417bc-1f01-11e7-9fae-a17030d3d051.png)

我们可以使用 [Next.js custom server API](https://github.com/zeit/next.js#custom-server-and-routing) 轻松解决这个问题。

##### 创建自定义服务器
 ```
  git clone https://github.com/arunoda/learnnextjs-demo.git
  cd learnnextjs-demo
  git checkout clean-urls
  
  npm install
  npm run dev
```
现在我们将使用Express为我们的应用程序创建一个自定义服务器。这很简单：

安装 express
```
npm install --save express
```
然后在您的应用程序中创建一个名为server.js的文件并添加以下内容：
```
const express = require('express')
const next = require('next')

const dev = process.env.NODE_ENV !== 'production'
const app = next({ dev })
const handle = app.getRequestHandler()

app.prepare()
.then(() => {
  const server = express()

  server.get('*', (req, res) => {
    return handle(req, res)
  })

  server.listen(3000, (err) => {
    if (err) throw err
    console.log('> Ready on http://localhost:3000')
  })
})
.catch((ex) => {
  console.error(ex.stack)
  process.exit(1)
})
```
更改的 npm 脚本
```
{
  "scripts": {
    "dev": "node server.js"
  }
}
```
重新执行 npm run dev

##### Create our Custom Route

正如你所经历的那样，该应用程序将像以前一样工作，因为我们编写的自定义服务器类似于“next”二进制命令。

现在我们将添加一条自定义路线以匹配我们的博客文章网址。

使用新的路由，我们的server.js将如下所示：

```
const express = require('express')
const next = require('next')

const dev = process.env.NODE_ENV !== 'production'
const app = next({ dev })
const handle = app.getRequestHandler()

app.prepare()
.then(() => {
  const server = express()

  server.get('/p/:id', (req, res) => {
    const actualPage = '/post'
    const queryParams = { title: req.params.id } 
    app.render(req, res, actualPage, queryParams)
  })

  server.get('*', (req, res) => {
    return handle(req, res)
  })

  server.listen(3000, (err) => {
    if (err) throw err
    console.log('> Ready on http://localhost:3000')
  })
})
.catch((ex) => {
  console.error(ex.stack)
  process.exit(1)
})
```

##### 有关URL的信息

我们的/post 页面通过查询字符串参数`title`接受标题。在客户端路由中，我们可以通过URL掩码轻松地给它一个合适的值。 （通过Link中的as 属性）
```
<Link as={`/p/${props.id}`} href={`/post?title=${props.title}`}>
  <a>{props.title}</a>
</Link>
```
但在服务器路由中，我们没有该标题，因为我们只有该URL中的博客帖子的ID。因此，在这种情况下，我们将ID设置为服务器端查询字符串参数.

你可以在下面的路由定义中看到它：
```
server.get('/p/:id', (req, res) => {
  const actualPage = '/post'
  const queryParams = { title: req.params.id } 
  app.render(req, res, actualPage, queryParams)
})
```
这是个问题。但在现实世界中，这不会成为一个问题，因为我们将使用ID从客户端和服务器中的数据服务器获取数据。 

所以，我们只需要一个ID。

在这里，我们简单地使用 Next.js自定义服务器 API 创建了一个路由。有了这个，我们增加了对干净URL的服务器端支持。就像这样，您可以根据需要创建多个路由。

您不限于使用Express。你可以使用任何Node.js服务器框架。您还可以查看[自定义服务器API](https://github.com/zeit/next.js#custom-server-and-routing)的Next.js文档

## 给页面获取数据

现在我们知道如何创建一个相当不错的Next.js应用程序，并充分利用Next.js路由API。

实际上，我们通常需要从远程数据源获取数据。 Next.js带有一个标准的API来获取页面的数据。我们使用称为`getInitialProps`的异步函数来执行此操作

借此，我们可以通过远程数据源为给定页面获取数据，并将其作为 props 传递给我们的页面。我们可以编写我们的`getInitialProps`以在服务器和客户端上工作。因此，Next.js可以在客户端和服务器上使用它。

在本课中，我们将使用getInitialProps构建一个应用程序，该应用程序使用公共TVmaze API显示有关蝙蝠侠电视节目的信息。
![https://cloud.githubusercontent.com/assets/50838/26300776/bbf275ee-3efc-11e7-8304-df96c7c7cad5.png](https://cloud.githubusercontent.com/assets/50838/26300776/bbf275ee-3efc-11e7-8304-df96c7c7cad5.png)

##### 安装设置

```
git clone https://github.com/arunoda/learnnextjs-demo.git
cd learnnextjs-demo
git checkout clean-urls-ssr
```
```
npm install
npm run dev
```
##### Fetching Batman Shows

在我们的演示应用程序中，我们在主页上列出了博客帖子。现在我们要展示蝙蝠侠电视节目的列表。 与其对这些节目进行硬编码，我们将从远程服务器获取它们.

先我们需要安装`isomorphic-unfetch`。这是我们要用来获取数据的库。这是一个简单的浏览器 fetch API的实现，但可以在客户端和服务器环境中使用

```
npm install --save isomorphic-unfetch
```

然后用以下内容替换我们的pages/index.js:

```
import Layout from '../components/MyLayout.js'
import Link from 'next/link'
import fetch from 'isomorphic-unfetch'

const Index = (props) => (
  <Layout>
    <h1>Batman TV Shows</h1>
    <ul>
      {props.shows.map(({show}) => (
        <li key={show.id}>
          <Link as={`/p/${show.id}`} href={`/post?id=${show.id}`}>
            <a>{show.name}</a>
          </Link>
        </li>
      ))}
    </ul>
  </Layout>
)

Index.getInitialProps = async function() {
  const res = await fetch('https://api.tvmaze.com/search/shows?q=batman')
  const data = await res.json()

  console.log(`Show data fetched. Count: ${data.length}`)

  return {
    shows: data
  }
}

export default Index
```
除了Index.getInitialProps之外，上述页面上的所有内容都将为您所熟悉，如下所示：

```
Index.getInitialProps = async function() {
  const res = await fetch('https://api.tvmaze.com/search/shows?q=batman')
  const data = await res.json()

  console.log(`Show data fetched. Count: ${data.length}`)

  return {
    shows: data
  }
}
```
这是一个静态的异步函数，您可以将其添加到应用程序的任何页面中。使用它，我们可以获取数据并将它们作为 props 发送到我们的页面。

正如您所看到的，现在我们正在抓取蝙蝠侠电视节目并将它们作为'shows'属性传到我们的页面。

![https://cloud.githubusercontent.com/assets/50838/26300128/de007dd6-3efa-11e7-9084-6ba7ff10774b.png](https://cloud.githubusercontent.com/assets/50838/26300128/de007dd6-3efa-11e7-9084-6ba7ff10774b.png)

正如你可以在上面的getInitialProps函数中看到的那样，它将打印获取到控制台的数据的数量。

现在，查看浏览器控制台和服务器控制台。 然后重新刷新页面，你会发现页面是服务端渲染的。

在这种情况下，消息仅在服务器上打印。 这是因为我们在服务器上呈现页面。 所以，我们已经有了这些数据，没有理由在客户端再次获取它。

##### 实现文章页面
现在让我们尝试实现“/ post”页面，其中显示有关电视节目的详细信息.

首先，打开server.js并使用以下内容更改/p/id路由：

```
server.get('/p/:id', (req, res) => {
    const actualPage = '/post'
    const queryParams = { id: req.params.id }
    app.render(req, res, actualPage, queryParams)
})
```
然后重新启动您的应用程序以应用上述代码更改。

现在用下面的内容替换 `pages/post.js`
```
import Layout from '../components/MyLayout.js'
import fetch from 'isomorphic-unfetch'

const Post =  (props) => (
    <Layout>
       <h1>{props.show.name}</h1>
       <p>{props.show.summary.replace(/<[/]?p>/g, '')}</p>
       <img src={props.show.image.medium}/>
    </Layout>
)

Post.getInitialProps = async function (context) {
  const { id } = context.query
  const res = await fetch(`https://api.tvmaze.com/shows/${id}`)
  const show = await res.json()

  console.log(`Fetched show: ${show.name}`)

  return { show }
}

export default Post
```

查看该页面的getInitialProps:
```
Post.getInitialProps = async function (context) {
  const { id } = context.query
  const res = await fetch(`https://api.tvmaze.com/shows/${id}`)
  const show = await res.json()

  console.log(`Fetched show: ${show.name}`)

  return { show }
}

```
在那里，函数的第一个参数是上下文对象。它有一个我们可以用来获取信息的查询字段。

在我们的示例中，我们从查询参数中选择了shows ID，并从TVMaze API中获取 show data.

在这个getInitialProps函数中，我们添加了一个console.log来打印节目标题。现在我们将看到它将要打印的位置。
##### 在客户端获取数据
在这里，我们只能在浏览器控制台上看到消息。 这是因为我们通过客户端导航到了帖子页面。然后从客户端获取数据是最好的方法。

现在你已经学会了Next.js最重要的特性之一，它使它成为通用数据读取和服务器端渲染的理想选择。

我们已经学习了getInitialProps的基本知识，这对于大多数用例来说已经足够了。您可以随时参考有关 [fetch data](https://github.com/zeit/next.js#fetching-data-and-component-lifecycle) 的Next.js文档以获取更多信息



## 样式化组件
目前为止，样式化组件在很大程度上是事后才想到的。但你的应用程序值得一些样式。
对于React应用程序，我们可以使用许多不同的技术来为它们添加样式，这些技术可以分为两大类：
 - 传统的基于CSS文件的样式（包括SASS，PostCSS等）
 - CSS in Js styling
 
传统的基于CSS文件的样式（特别是SSR）需要考虑一些实际问题，因此建议在为Next.js设计样式时避免使用此方法。

Next.js在JS框架中预装了一个名为styled-jsx的CSS，专门用于使您的生活更轻松。它允许您为组件编写熟悉的CSS规则;规则将不会影响组件（甚至不包括子组件）。

让我们看看我们如何使用styled-jsx

##### 安装设置
```
git clone https://github.com/arunoda/learnnextjs-demo.git
cd learnnextjs-demo
git checkout clean-urls-ssr
```
```
npm install
npm run dev
```
##### 给首页添加样式
```
import Layout from '../components/MyLayout.js'
import Link from 'next/link'

function getPosts () {
  return [
    { id: 'hello-nextjs', title: 'Hello Next.js'},
    { id: 'learn-nextjs', title: 'Learn Next.js is awesome'},
    { id: 'deploy-nextjs', title: 'Deploy apps with ZEIT'},
  ]
}

export default () => (
  <Layout>
    <h1>My Blog</h1>
    <ul>
      {getPosts().map((post) => (
        <li key={post.id}>
          <Link as={`/p/${post.id}`} href={`/post?title=${post.title}`}>
            <a>{post.title}</a>
          </Link>
        </li>
      ))}
    </ul>
    <style jsx>{`
      h1, a {
        font-family: "Arial";
      }

      ul {
        padding: 0;
      }

      li {
        list-style: none;
        margin: 5px 0;
      }

      a {
        text-decoration: none;
        color: blue;
      }

      a:hover {
        opacity: 0.6;
      }
    `}</style>
  </Layout>
)
```
看看<style jsx>元素。这是我们编写CSS规则的地方.  在您替换此代码后，我们博客的主页将如下所示：
  ![https://cloud.githubusercontent.com/assets/50838/25552915/f18f2f12-2c5a-11e7-97aa-4b9d4b9f95a7.png](https://cloud.githubusercontent.com/assets/50838/25552915/f18f2f12-2c5a-11e7-97aa-4b9d4b9f95a7.png)

在上面的代码中，我们没有直接在style标签中写入样式;相反，它是在模板字符串中写入的.

现在尝试直接写入CSS而不使用模板字符串`（{``}）`。像下面这样：
```
<style jsx>
  h1, a {
    font-family: "Arial";
  }

  ul {
    padding: 0;
  }

  li {
    list-style: none;
    margin: 5px 0;
  }

  a {
    text-decoration: none;
    color: blue;
  }

  a:hover {
    opacity: 0.6;
  }
</style>
```
发现页面报错了。

Styled jsx 作为一个babel插件工作。它将解析所有的CSS并在构建过程中应用它。 （由于我们的样式无需任何开销时间就可以应用）。

它还支持在style-jsx中有约束。将来，您将能够使用styled-jsx中的任何动态变量。这就是为什么CSS需要进入模板字符串。 ```（{``}）```

现在让我们添加一个简单的更改到我们的主页。我们已经隔离了我们的链接组件：
```
import Layout from '../components/MyLayout.js'
import Link from 'next/link'

function getPosts () {
  return [
    { id: 'hello-nextjs', title: 'Hello Next.js'},
    { id: 'learn-nextjs', title: 'Learn Next.js is awesome'},
    { id: 'deploy-nextjs', title: 'Deploy apps with ZEIT'},
  ]
}

const PostLink = ({ post }) => (
  <li>
    <Link as={`/p/${post.id}`} href={`/post?title=${post.title}`}>
      <a>{post.title}</a>
    </Link>
  </li>
)

export default () => (
  <Layout>
    <h1>My Blog</h1>
    <ul>
      {getPosts().map((post) => (
        <PostLink key={post.id} post={post}/>
      ))}
    </ul>
    <style jsx>{`
      h1, a {
        font-family: "Arial";
      }

      ul {
        padding: 0;
      }

      li {
        list-style: none;
        margin: 5px 0;
      }

      a {
        text-decoration: none;
        color: blue;
      }

      a:hover {
        opacity: 0.6;
      }
    `}</style>
  </Layout>
)
```
你看的的页面如下图所示：
![https://cloud.githubusercontent.com/assets/50838/25552972/6becac5c-2c5c-11e7-9fce-61cdc207a10d.png](https://cloud.githubusercontent.com/assets/50838/25552972/6becac5c-2c5c-11e7-9fce-61cdc207a10d.png)

如您所见，CSS规则对子组件内部的元素没有影响

style-jsx的这一特性可帮助您管理更大型应用程序的样式.

在这种情况下，您需要直接对子组件进行样式设置。在我们的特殊情况下，我们需要为Link组件做到这一点：

```
const PostLink = ({ post }) => (
  <li>
    <Link as={`/p/${post.id}`} href={`/post?title=${post.title}`}>
      <a>{post.title}</a>
    </Link>
    <style jsx>{`
      li {
        list-style: none;
        margin: 5px 0;
      }

      a {
        text-decoration: none;
        color: blue;
        font-family: "Arial";
      }

      a:hover {
        opacity: 0.6;
      }
    `}</style>
  </li>
)
```

##### Global Styles

有时，我们确实需要更改子组件内的样式。在使用React markdown 时尤其如此。你可以在我们的文章页面看到.（pages/post.js）。

这是全局样式派上用场的地方。现在尝试使用style-jsx添加一些全局样式规则。将以下内容应用于pages  post.js.

npm install --save react-markdown

```
import Layout from '../components/MyLayout.js'
import Markdown from 'react-markdown'

export default (props) => (
  <Layout>
   <h1>{props.url.query.title}</h1>
   <div className="markdown">
     <Markdown source={`
This is our blog post.
Yes. We can have a [link](/link).
And we can have a title as well.

### This is a title

And here's the content.
     `}/>
   </div>
   <style jsx global>{`
     .markdown {
       font-family: 'Arial';
     }

     .markdown a {
       text-decoration: none;
       color: blue;
     }

     .markdown a:hover {
       opacity: 0.6;
     }

     .markdown h3 {
       margin: 0;
       padding: 0;
       text-transform: uppercase;
     }
  `}</style>
  </Layout>
)
```
你发下markdown 样式生效了。

虽然此功能可以非常方便，但我们始终建议您尝试编写有作用域的样式。

尽管如此，这对于普通样式标签来说是一个很好的解决方案使用styled-jsx所有必要的前缀和CSS验证都是在babel插件中完成的，因此不会产生额外的运行时间开销。

我们刚刚在这里使用了style-jsx，并且还有很多可以做的事情。有关更多信息，请参阅[styled-jsx](https://github.com/zeit/styled-jsx) 


## 部署一个 Next.js App
你有没有遇到过这个问题？
> 我怎样才能部署我的Next.js应用程序？

我们还没有谈过这个，但它非常简单直接。

您可以将Next.js应用程序部署到可以运行Node.js的任何位置。

我们来了解部署Next.js应用程序。

原文讲的是用 now （zeit 的产品服务），我这里不多翻译了，就简单的说一下大概步骤。

服务器的入口文件就使用上文中提到的 server.js，在 server.js 里添加了针对部署环境的选择，代码如下：
```
const dev = process.env.NODE_ENV !== 'production'
```
为了区分部署环境，我们需要在 package.json 中修改 script 属性如下：
```
"scripts": {
  "build": "next build",
  "start": "NODE_ENV=production node server.js -",
  "dev": "NODE_ENV=dev node server.js"
}
```
其中，build 命令是用于打包项目，start 命令是用于生产环境部署，dev 命令是用于本地开发。

执行如下命令即可将 Next项目 部署到服务器：
```
$ npm run build
$ npm run start
```
执行完命令后，可在 http://host:3000 访问。其中，host 是指服务器的IP地址。



 
