<div align="center">

# kubernetes 中文翻译版

**[中文版] kubernetes — 生产级容器编排与调度管理系统,自动化部署、扩缩容与管理容器化应用**

[![原项目](https://img.shields.io/badge/原项目-kubernetes--kubernetes-blue?style=flat-square&logo=github)](https://github.com/kubernetes/kubernetes)
[![中文文档](https://img.shields.io/badge/中文文档-README.zh--CN.md-orange?style=flat-square)](README.zh-CN.md)
[![GitHub Stars](https://img.shields.io/github/stars/kubernetes/kubernetes?style=flat-square&label=原项目Stars)](https://github.com/kubernetes/kubernetes/stargazers)
[![微信联系](https://img.shields.io/badge/微信-uaycar-brightgreen?style=flat-square&logo=wechat)](#)

</div>

---

> 这是 [kubernetes/kubernetes](https://github.com/kubernetes/kubernetes) 的中文翻译版本。
> 完整源代码请访问原项目:https://github.com/kubernetes/kubernetes

**代部署 / 定制服务 / 技术咨询 请添加微信:uaycar**

---

## 📖 项目简介

Kubernetes(简称 K8s)是一个开源的容器编排系统,用于跨多台主机管理容器化应用,提供应用部署、维护与扩缩容所需的基础机制。它建立在 Google 使用内部系统 [Borg](https://research.google.com/pubs/pub43438.html?authuser=1) 运行大规模生产负载十五年的经验之上,并融合了社区的最佳实践与理念。项目由云原生计算基金会([CNCF](https://www.cncf.io/about))托管,是当今云原生领域事实上的标准。

## ✨ 主要特性

- **开源容器编排**:跨多台主机统一管理容器化应用
- **完备的基础机制**:覆盖应用的部署、维护与扩缩容全生命周期
- **久经生产验证**:源自 Google Borg 十五年大规模生产环境经验
- **中立基金会治理**:CNCF 托管,社区开放共治,生态成熟
- **文档与教程完善**:[kubernetes.io](https://kubernetes.io) 提供官方文档
- **免费学习资源**:Udacity《Scalable Microservices with Kubernetes》免费课程
- **组件可复用**:一系列已发布组件可作为库引入其他项目
- **构建方式灵活**:Go 环境直接 `make`,或 Docker 环境一键 `make quick-release`
- **治理透明**:原则、政策与流程清晰,社区会议与路线图公开
- **落地案例丰富**:官网收录各行业组织部署/迁移 Kubernetes 的真实案例

## 📁 文件说明

| 文件 | 说明 |
|:-----|:-----|
| README.md | 本文件(中文简介) |
| README.zh-CN.md | 详细中文文档(完整汉化) |

## 🚀 快速开始

1. **学习入门**:阅读 [kubernetes.io](https://kubernetes.io) 官方文档,或跟随官方交互式教程(Kubernetes Basics)动手体验。
2. **安装 kubectl** 并准备一个本地集群(如 kind、minikube),即可快速上手。
3. **源码构建**,二选一(取自原 README):

   已具备可用的 Go 环境:

   ```
   git clone https://github.com/kubernetes/kubernetes
   cd kubernetes
   make
   ```

   已具备可用的 Docker 环境:

   ```
   git clone https://github.com/kubernetes/kubernetes
   cd kubernetes
   make quick-release
   ```

4. **遇到问题**:先查阅官方 [troubleshooting guide](https://kubernetes.io/docs/tasks/debug/),按流程排查;有疑问可通过社区渠道交流。

完整源代码与最新版本请访问原项目:https://github.com/kubernetes/kubernetes

## 📞 联系方式

**代部署 / 定制服务 / 技术咨询 请添加微信:uaycar**

---

本项目为 [kubernetes/kubernetes](https://github.com/kubernetes/kubernetes) 的中文翻译版本,所有代码版权归原项目作者所有,遵循其原始许可证。

**如果觉得有用,请给原项目点个 Star!** ⭐
