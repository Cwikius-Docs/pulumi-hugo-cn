# pulumi-hugo-cn

欢迎来到 Pulumi 中文文档项目（https://pulumi.ossez.com/），本项目是直接从官方的英文文档中 Fork 下来，并且直接翻译后及时更新的。

翻译后的文档使用 Google FireBase 项目提供的服务进行部署，部署访问的地址为：https://pulumi.ossez.com/

我们使用了 Jenkins 来进行 CI，本想使用一些开源或者在线的 CI 工具的，但在调试的时候对我们的编译次数有限制，一些构建需要本地环境，使用虚拟环境的构建时间过长，因此我们就集使用 Jenkins 来进行部署了。如您还有什么好的建议，请联系管理员。

## 中文本地化说明

如果您有兴趣参与我们的小组和项目，请使用下面的联系方式和我们联系：

| 联系方式名称  | 联系方式  |
|---|---|
| 电子邮件  | [service@ossez.com](mailto:service@ossez.com)  |
| QQ 或微信  | 103899765  |
| QQ 交流群 | 15186112 |
| 社区论坛 | [https://www.ossez.com/](https://www.ossez.com/) |

## 公众平台
我们建议您通过社区论坛来和我们进行沟通，请关注我们公众平台上的账号

### 微信公众号
![](https://cdn.ossez.com/img/cwikius/cwikius-qr-wechat-search-w400.png)

### 头条号
我们也在头条号上创建了我们的公众号，请扫描下面的 QR 关注我们的头条号。

![](https://cdn.ossez.com/img/cwikius/cwikus-qr-toutiao.png)


[Hugo](https://gohugo.io) 模块包含有 Pulumi 主题和网站的内容。

这个参考同时也被官方的 https://github.com/pulumi/docs 项目使用，用来创建你可以通过 https://pulumi.com 访问到的内容。如果你有兴趣对文档进行一些修改或者提交你的自己的博客内容到 https://pulumi.com/blog 链接中，那么你就来到了正确的地方了! 🙌

## 官方文档贡献

首先，请确保你已经阅读了我们的 [社区贡献指南](CONTRIBUTING.md) 同时也参考了我们的 [代码规范](CODE_OF_CONDUCT.md)。

## 工具链

We build the Pulumi website statically with Hugo, manage our Node.js dependencies with Yarn, and write most of our documentation in Markdown. Below is a list of the tools you'll need to run the website locally:

* [Go](https://golang.org/)
* [Hugo](https://gohugo.io)
* [Node.js](https://nodejs.org/en/)
* [Yarn](https://classic.yarnpkg.com/en/)

### CSS and JavaScript Tools

We also use a handful of tools for compiling and managing our CSS and JavaScript assets, including:

* [Sass](https://sass-lang.com/)
* [TailwindCSS](https://tailwindcss.com/)
* [Stencil.js](https://stenciljs.com/)
* [TypeScript](https://www.typescriptlang.org/)

You don't need to install these tools individually or globally; the scripts below will handle everything for you. But if you'd like to contribute any CSS or JavaScript, you'll probably want to understand how to work with each of these tools as well.

## Installing prerequisites

Run `make ensure` to check for the appropriate tools and versions and install any dependencies. The script will let you know if you're missing anything important.

```
make ensure
```

## Running Hugo locally

Once you've run `make ensure` successfully, you're ready to run the development server. If you're only planning on writing Markdown or working with Hugo layouts, this command should be all you need:

```
make serve
```

You can browse the development server at http://localhost:1313, and any changes you make to content or layouts should be reloaded automatically.

## Running Hugo with CSS and JavaScript support

If you plan on making changes to CSS or JavaScript files, you'll probably want to use this command instead:

```
make serve-all
```

The `serve-all` target runs Hugo, node-sass, and the Stencil development server concurrently, allowing you to make changes to Sass files, Stencil components, or TypeScript/JavaScript source files, and have those changes compiled and reloaded automatically as well.

## Linting and testing

To check your code and your Markdown files for issues before submitting, run:

```
make lint test
```

## Tidying up

To clear away any module caches or build artifacts, run:

```
make clean
```

## About this repository

This repository is a [Hugo module](https://gohugo.io/hugo-modules/) that doubles as a development server. **It does not contain every page of the Pulumi website**, because most of those pages (e.g., those comprising our CLI and SDK docs) are generated from source code, so they aren't meant to be edited by humans directly.

Because of this, many of the links you follow when browsing around on the development server (to paths underneath `/docs/reference` for example) will fail to resolve because their content files are are checked into a different repository &mdash; most likely https://github.com/pulumi/docs. When we build the Pulumi website, we merge this module along with any others into a single build artifact, but when you're working within an individual module like this one, you may find you're unable to reach certain pages or verify the links you may want to make to them.

If you want to link to a page that exists on https://pulumi.com but not in this repository, just use the page's **relative path** with a [Hugo `relref`](https://gohugo.io/content-management/shortcodes/#ref-and-relref) in the usual way, and we'll make sure all links resolve properly at build-time. For example, to link to the [Digital Ocean Droplet](https://www.pulumi.com/docs/reference/pkg/digitalocean/droplet/) page (a page that doesn't exist in this repository but that would exist in an integration build), you'd use:

```
{{< relref /docs/reference/pkg/digitalocean/droplet >}}
```

This works because we've suppressed Hugo's built-in `relref` validation to keep the module-development workflow as lightweight as possible.

### What's in this repo

* All hand-authored content and documentation, including top-level pages, guides, blog posts, and some tutorials
* Most Hugo module components, including archetypes, layouts, partials, shortcodes, data, etc.

You'll find all of these files in `themes/default`.

### What's not in this repo

* Generated documentation for the Pulumi CLI and SDK. You'll find this at https://github.com/pulumi/docs).
* Generated tutorials. You'll find these at https://github.com/pulumi/examples).
* Templates used for generating resource documentation. You'll find these at https://github.com/pulumi/pulumi.

## Merging and releasing

When a pull request is merged into the default branch of this repository, a follow-up PR is triggered on [pulumi/docs](https://github.com/pulumi/docs) that produces an integration build. Once that build completes and is approved and merged into pulumi/docs, the changes are deployed to pulumi.com.

## Blogging

Interested in writing a blog post? See the [blogging README](BLOGGING.md) for details.

## Shortcodes and web components

We use number of Hugo shortcodes and web components in our pages. You can read more about many of them in the [components README](themes/default/components).
