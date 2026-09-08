# kubernetes 中文文档

<div align="center">

[![原项目](https://img.shields.io/badge/原项目-kubernetes--kubernetes-blue?style=flat-square&logo=github)](https://github.com/kubernetes/kubernetes)
[![微信联系](https://img.shields.io/badge/微信-uaycar-brightgreen?style=flat-square&logo=wechat)](#)

</div>

> 本文档是 [kubernetes/kubernetes](https://github.com/kubernetes/kubernetes) 官方 README 的中文翻译版本。
> 完整源代码请访问原项目:https://github.com/kubernetes/kubernetes
> **代部署 / 定制服务 / 技术咨询 请添加微信:uaycar**

---

## 项目简介

Kubernetes(也写作 K8s)是一个开源系统,用于管理跨多台主机运行的[容器化应用](https://kubernetes.io/docs/concepts/overview/what-is-kubernetes/),为应用的部署、维护和扩缩容提供基础机制。

Kubernetes 建立在 Google 长达十五年、使用名为 [Borg](https://research.google.com/pubs/pub43438.html?authuser=1) 的系统运行大规模生产负载所积累的经验之上,并结合了社区一流的想法与实践。

Kubernetes 由云原生计算基金会([CNCF](https://www.cncf.io/about))托管。如果你的公司希望参与塑造以容器为封装、动态调度、面向微服务的技术演进,可以考虑加入 CNCF。关于参与方详情以及 Kubernetes 在其中的角色,请阅读 CNCF 的[官方公告](https://cncf.io/news/announcement/2015/07/new-cloud-native-computing-foundation-drive-alignment-among-container)。

## 开始使用 K8s

请阅读 [kubernetes.io](https://kubernetes.io) 上的官方文档。

可以学习 Udacity 上的免费课程《[Scalable Microservices with Kubernetes](https://www.udacity.com/course/scalable-microservices-with-kubernetes--ud615)》。

如果希望在其它应用中把 Kubernetes 代码当作库来使用,请参阅[已发布组件列表](https://git.k8s.io/kubernetes/staging/README.md)。注意:不支持将 `k8s.io/kubernetes` 模块或 `k8s.io/kubernetes/...` 包作为库使用。

## 开始开发 K8s

[社区仓库](https://git.k8s.io/community)收录了从源码构建 Kubernetes 的全部信息,包括如何贡献代码与文档、遇到什么事该联系谁等。

如果想立刻构建 Kubernetes,有两种方式:

**已有可用的 [Go 环境](https://go.dev/doc/install):**

```
git clone https://github.com/kubernetes/kubernetes
cd kubernetes
make
```

**已有可用的 [Docker 环境](https://docs.docker.com/engine):**

```
git clone https://github.com/kubernetes/kubernetes
cd kubernetes
make quick-release
```

完整故事请移步[开发者文档](https://git.k8s.io/community/contributors/devel#readme)。

## 支持

如需支持,请先从[故障排查指南](https://kubernetes.io/docs/tasks/debug/)开始,按官方给出的流程逐步排查。

如有疑问,欢迎通过[任意社区渠道](https://git.k8s.io/community/communication)与我们联系。

## 社区会议

Kubernetes 社区的所有会议都汇总在一份统一的日历中,参见 [Calendar](https://www.kubernetes.dev/resources/calendar/)。

## 采用案例

[User Case Studies](https://kubernetes.io/case-studies/) 网站收录了各行各业组织部署或迁移到 Kubernetes 的真实案例。

## 治理

Kubernetes 项目由一套原则、价值观、政策与流程组成的框架进行治理,帮助社区和各参与方朝着共同目标前进。

了解社区如何自我组织,可以从 [Kubernetes Community](https://github.com/kubernetes/community/blob/master/governance.md) 开始。

[Kubernetes Steering 社区仓库](https://github.com/kubernetes/steering)由 Kubernetes 指导委员会(Steering Committee)使用,该委员会负责监督整个项目的治理。

## 路线图

[Kubernetes Enhancements 仓库](https://github.com/kubernetes/enhancements)提供 Kubernetes 版本发布、特性跟踪与待办事项的相关信息。

---

## 许可与声明

- 本文档为 [kubernetes/kubernetes](https://github.com/kubernetes/kubernetes) 官方 README 的中文翻译版本,仅供学习与交流使用。
- 所有代码与原始内容的版权归原项目作者所有,遵循其原始许可证(Kubernetes 采用 Apache License 2.0)。
- **代部署 / 定制服务 / 技术咨询 请添加微信:uaycar**

**如果觉得有用,请给原项目点个 Star!** ⭐
