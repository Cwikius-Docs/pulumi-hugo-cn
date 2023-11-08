---
title: Documentation
linktitle: Docs
meta_desc: Learn how to create, deploy, and manage infrastructure on any cloud using Pulumi's open source infrastructure as code SDK.
meta_image: /images/docs/meta-images/docs-meta.png
aliases:
- /docs/reference/
notitle: true
docs_home: true
h1: Pulumi Docs
description: <p>Pulumi is an <a href="https://github.com/pulumi/pulumi" target="_blank">open source</a> infrastructure as code tool for creating, deploying, and managing cloud infrastructure.</p>
link_buttons:
  primary:
    label: Quickstart
    link: /docs/get-started/
  secondary:
    label: Install
    link: /docs/install/
sections:
- type: cards-logo-label-link
  heading: Clouds
  description: <p>Pulumi supports AWS, Azure, Google Cloud, Kubernetes, and <a href="/registry/">120+ packages</a>.</p>
  cards:
  - label: AWS & Pulumi
    icon: aws-40
    link: /docs/clouds/aws/
  - label: Azure & Pulumi
    icon: azure-40
    link: /docs/clouds/azure/
  - label: Google Cloud & Pulumi
    icon: google-cloud-40
    link: /docs/clouds/gcp/
  - label: Kubernetes & Pulumi
    icon: kubernetes-40
    link: /docs/clouds/kubernetes/
- type: full-width-cards
  heading: Featured docs
  cards:
  - icon: lightbulb-blue-21-21
    heading: Concepts
    description: Dive deeper into what Pulumi is and how it works.
    link: /docs/concepts/
  - icon: cloud-blue-21-21
    heading: Pulumi Cloud
    description: Learn about Pulumi’s secure cloud service for individuals and teams.
    link: /docs/pulumi-cloud/
  - icon: swap-blue-21-21
    heading: Adopting Pulumi
    description: Migrate any infrastructure to Pulumi with comprehensive guides.
    link: /docs/using-pulumi/adopting-pulumi/
- type: blue-sparkle
  heading: Why Pulumi?
  description: Pulumi enables developers to deploy infrastructure in any cloud environment with one common approach. Leverage familiar languages to make the most of abstractions and reuse while enjoying access in your favorite IDEs, and testing tools!
- type: cards-logo-label-link
  heading: Languages
  description: Build infrastructure intuitively on any cloud using familiar languages.
  cards:
  - label: Node.js
    icon: icon-32-32 node-color-32-32
    link: /docs/languages-sdks/javascript/
  - label: Python
    icon: icon-32-32 python-color-32-32
    link: /docs/languages-sdks/python/
  - label: Go
    icon: icon-32-32 go-color-32-32
    link: /docs/languages-sdks/go/
  - label: .NET
    icon: icon-32-32 dotnet-color-32-32
    link: /docs/languages-sdks/dotnet/
  - label: Java
    icon: icon-32-32 java-color-32-32
    link: /docs/languages-sdks/java/
  - label: YAML
    icon: icon-32-32 yaml-color-32-32
    link: /docs/languages-sdks/yaml/
- type: full-width-cards
  heading: Reference
  cards:
    - icon: code-tree-blue-21-21
      heading: Pulumi Cloud REST API docs
      description: Leverage automation with the Pulumi Cloud REST API.
      link: /docs/pulumi-cloud/cloud-rest-api/
    - icon: terminal-blue-21-21
      heading: Pulumi CLI docs
      description: Browse the complete documentation of available CLI commands.
      link: /docs/cli/
- type: flat
  heading: Have questions?
  description: <p>For questions or feedback, reach out on <a href="https://slack.pulumi.com" target="_blank">community Slack</a>, <a href="https://github.com/pulumi" target="_blank">GitHub</a>, or <a href="/support/">contact support</a>.</p>
---

欢迎来到 Pulumi 中文文档！ 本页面对 Pulumi 是什么进行了说明，并且指导你如何开始使用 Pulumi。同时也对一些参考的文档和支持的云平台进行了说明。

### 什么是 Pulumi？

Pulumi 是一个 <a href="https://github.com/pulumi/pulumi" target="_blank">开源（open source）</a> 系统基础架构代码化工具。
这个工具被用来创建，部署和管理云计算平台基础架构。
Pulumi 能在基础计算平台上工作，例如 VMs，网络和数据库，同时还能够在更加现代化的架构上进行工作，包括有容器，Kubernetes 集群和无服务方法（serverless functions）。
Pulumi 能够广泛支持公有，私有和混合云服务提供商。


<div class="flex justify-center py-6">
    <a class="btn btn-lg mx-1 my-1" href="{{< relref "/docs/get-started" >}}">开始使用</a>
</div>

<div class="bg-gray-100 rounded max-w-6xl my-4 px-4 py-2">
    <div class="md:flex justify-between items-center">
        <a class="block rounded hover:bg-gray-200 transition-all my-2 py-4 text-center px-6" href="{{< relref "/docs/get-started/aws" >}}">
            <img class="inline-block h-8 w-auto -mb-2" src="/logos/tech/aws.svg">
        </a>
        <a class="block rounded hover:bg-gray-200 transition-all my-2 text-center md:mx-2 py-4 px-6" href="{{< relref "/docs/get-started/azure" >}}">
            <img class="inline-block h-8 w-auto" src="/logos/tech/azure.svg">
        </a>
        <a class="block rounded hover:bg-gray-200 transition-all my-2 text-center md:mx-2 py-4 px-6" href="{{< relref "/docs/get-started/gcp" >}}">
            <img class="inline-block h-8 w-auto" src="/logos/tech/gcp.svg">
        </a>
        <a class="block rounded hover:bg-gray-200 transition-all my-2 py-4 text-center px-6" href="{{< relref "/docs/get-started/kubernetes" >}}">
            <img class="inline-block h-8 w-auto" src="/logos/tech/k8s.svg">
        </a>
    </div>
</div>

<div class="my-4 md:flex py-8">
    <div class="md:w-1/3">
        <h3 class="no-anchor">为什么是 Pulumi？</h3>
        <p class="text-sm text-gray-700">
            通过使用 <a href="{{< relref "/docs/intro/languages" >}}">你熟悉的语言</a> 为基础机构进行代码化，可以让你有很多便利：
            IDEs，抽象嵌入功能，类，和包，已有的调试和测试工具以及更多。结果就是针对复制和粘贴来说能够更有生产效率，同时针对不同的云平台都能产生相同的结果。
        </p>
    </div>
    <div class="md:mx-8 md:w-1/3">
        <h3 class="no-anchor">可选的</h3>
        <p class="text-sm text-gray-700">
            相对<a href="{{< relref "/docs/intro/vs" >}}">其他管理方式</a>，例如使用 YAML,
            JSON, 或者领域的一些特定语言 domain-specific languages (DSLs) 等方式，你需要你的项目小组进行学习使用。这些工具通常在分享和重用上面有不少的问题。
            有可能不能应用在当前的系统中，同时针对不同的云平台你需要独立进行配置。
        </p>
    </div>
    <div class="md:w-1/3">
        <h3 class="no-anchor">社区支持</h3>
        <p class="text-sm text-gray-700">
            Pulumi 是 <a href="https://github.com/pulumi/pulumi" target="_blank">开源（open source）</a> 软件。因此具有很高的扩展性，
            同时还有非常强大的社区来提供支持，这些社区通常都是云平台使用的先锋。
            从重用类库到相同的基础架构，到分享你自己的设计，Pulumi 能够为你带来更多的便利。不同语言的也云平台的支持使用插件模式来进行扩展。
            针对公用，私用甚至混合云支持都可以通过插件来进行扩展。
        </p>
    </div>
</div>

有关任何问题或者反馈，请访问官方的 [community Slack channel](https://slack.pulumi.com),
[GitHub](https://github.com/pulumi), 或者 [support@pulumi.com](mailto:support@pulumi.com)。

有关本中文版本文档的翻译，支持，使用，请访问 [GitHub 中文文档源代码](https://github.com/cwiki-us-docs/pulumi-hugo-cn),
中文社区的 Pulumi 版块 [https://www.ossez.com](https://www.ossez.com)，以及我们中文项目中提供的微信公众号。
