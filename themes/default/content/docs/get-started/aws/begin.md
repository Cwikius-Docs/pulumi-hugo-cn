---
title: 在开始使用之前 | AWS
h1: Before You Begin
linktitle: Before You Begin
meta_desc: This page provides an overview on how to get started with Pulumi when starting an AWS project.
weight: 2
menu:
  getstarted:
    parent: aws
    identifier: aws-begin

aliases: [
  "/docs/quickstart/aws/begin/",
  "/docs/quickstart/aws/install-pulumi/",
  "/docs/quickstart/aws/install-language-runtime/",
  "/docs/quickstart/aws/configure/",
  "/docs/get-started/aws/install-pulumi/",
  "/docs/get-started/aws/install-language-runtime/",
  "/docs/get-started/aws/configure/"
]
---

在我们开始使用 Pulumi 之前，请快速运行下面的一些步骤来确定你的环境已经配置正确了。

### 安装 Pulumi

{{< install-pulumi >}}
{{% notes "info" %}}
在本指南中使用的所有示例都假设你是在 PowerShell 上运行的。
{{% /notes %}}
{{< /install-pulumi >}}

下一步，如果你还没有安装需要的语言运行环境的话，请先进行安装。

### 安装语言运行环境

#### 选择你的语言

{{< chooser language "javascript,typescript,python,go,csharp" / >}}

{{% choosable language "javascript,typescript" %}}
{{< install-node >}}
{{% /choosable %}}

{{% choosable language python %}}
{{< install-python >}}
{{% /choosable %}}

{{% choosable language go %}}
{{< install-go >}}
{{% /choosable %}}

{{% choosable language "csharp,fsharp,visualbasic" %}}
{{< install-dotnet >}}
{{% /choosable %}}

最后，将 Pulumi 配置使用 AWS。

### 配置 Pulumi 来访问你的 AWS 账号

Pulumi 将会要求使用云平台访问的账号来对资源提供管理。你必须使用一个 IAM 用户账号，这个用户账号需要有 **程序访问（Programmatic access）** 权限来通过使用 Pulumi 
在本指南中，你的 IAM 用户必须赋有 `s3:CreateBucket` IAM policy action 权限。

If you have previously <a href="https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html" target="_blank">installed</a> and <a href="https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-configure.html" target="_blank">configured</a> the AWS CLI, Pulumi will respect and use your configuration settings.

If you do not have the AWS CLI installed or plan on using Pulumi from within a CI/CD pipeline, <a href="https://docs.aws.amazon.com/general/latest/gr/aws-sec-cred-types.html#access-keys-and-secret-access-keys" target="_blank">retrieve your access key ID and secret access key</a> and then set the `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY` environment variables on your workstation.

{{< chooser os "linux,macos,windows" >}}
{{% choosable os linux %}}

```bash
$ export AWS_ACCESS_KEY_ID=<YOUR_ACCESS_KEY_ID>
$ export AWS_SECRET_ACCESS_KEY=<YOUR_SECRET_ACCESS_KEY>
```

{{% /choosable %}}

{{% choosable os macos %}}

```bash
$ export AWS_ACCESS_KEY_ID=<YOUR_ACCESS_KEY_ID>
$ export AWS_SECRET_ACCESS_KEY=<YOUR_SECRET_ACCESS_KEY>
```

{{% /choosable %}}

{{% choosable os windows %}}

```powershell
> $env:AWS_ACCESS_KEY_ID = "<YOUR_ACCESS_KEY_ID>"
> $env:AWS_SECRET_ACCESS_KEY = "<YOUR_SECRET_ACCESS_KEY>"
```

{{% /choosable %}}
{{< /chooser >}}

For additional information on setting and using AWS credentials, see [AWS Setup]({{< relref "/docs/intro/cloud-providers/aws/setup" >}}).

Next, you'll create a new project.

{{< get-started-stepper >}}
