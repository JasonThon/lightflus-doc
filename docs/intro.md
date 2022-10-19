# 简介

Hi Everyone，我们很高兴为大家介绍我们打造的一款新的通用流计算系统—— `Lightflus`。相比目前主流的流处理框架（`Flink`，`Spark Streaming`，`Storm`），`Lightflus` 的
**设计目标**是更低的上手门槛和更简单的运维。即使团队内没有人接触过流计算，但熟悉 `Stream API`
的用户，也可以在了解基本的概念后在自己的电脑上快速编写并部署一个流计算任务。不论是大型公司还是小团队，都可以通过`Lightflus` 来降低引入实时计算技术的成本。

目前 `Lightflus` 还处于 demo 阶段，欢迎大家来试用并提出宝贵意见！~ 。第一版正式的商业版本的发布我们会在 Slack 群里同步。

# Features

`Lightflus` demo 版本将提供以下的特性：

* 提供 `Dataflow Typescript API`；
* `map`, `flatMap`, `reduce` 等常用算子；
* 提供 `window` 和 `trigger` 的能力；
* 支持 `kafka`, `mysql` 和 `s3(minio)` 的 `source`/`sink`；
* CLI 支持；

`Lightflus` demo version 不支持，但将在 released version 1.0 中支持的：

* `Checkpoint` Fault Tolerance 支持；
* 支持 `join` 算子；
* `elasticsearch` 的 `source`/`sink`；
* 将支持 SaaS 和 On-Premise 两种使用模式；

# Document

如果想了解关于 `Lightflus` 更多的详细信息，比如：

* 收费方式
* Architecture Design
* Technical Details

的用户可以看文档。

# Community

上 V 站的朋友应该都能翻墙（重点），我们倾向于选择比 WeChat 更合适的沟通工具来减少信息噪音，更重要的是能够拉进我们团队与用户的沟通距离，因此**强烈推荐**各位使用 Slack。 非常欢迎各位加入 Lightflus 的
Slack
社区，我们会在公共频道广播关于产品的版本更新和 bug 修复追踪。同时，我们在 Slack 上提供了与我们团队及其他使用者的交流频道，以及 issue tracking 的频道。

# Quick Start For Demo

## CLI 安装

```bash
brew install lightflus-cli
```

## 通过 Docker 启动 demo

```bash
# 拉取 demo 的 repo
git clone https://github.com/lightflus/demo.git

# 进入 demo 的目录
cd demo

# 启动 demo
docker-compose up
```

demo 将会启动以下服务：

* `lightflus-coordinator`
* `lightflus-worker`
* `kafka`
* `minio`
* `lightflus-apiserver`
* `demo-app`

## 编写 word count 的任务

* 先通过 npm 安装 lightflus 的 api

```bash
npm install lightflus
```

* 生成任务文件

```bash
lightflus create wordCount
```

* 编写任务代码

## 部署流任务

```bash
lightflus apply -f wordCount.ts
```

在控制台会输出一个 jobId `1`

## 输入数据

```
POST /demo/job/{job_id}
{
    "value": "hello world"
}

```

反复请求多次可以累加 word count 的状态

## 确认结果

登录 `http://localhost:9001` 来查看在指定路径下生成的 wordCount 文件。 



