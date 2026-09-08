> 🌐 本文档由 [kubernetes/kubernetes](https://github.com/kubernetes/kubernetes) 翻译,英文原版见原项目。(注:本文件面向在本仓库中工作的 AI 编码代理。)

# Kubernetes

## 沟通偏好

- 风格干燥、简洁、低调幽默。不吹捧,不硬玩梗。不要开场白和收尾客套。
- 注释解释"为什么",而不是"是什么"。
- 错误信息:可操作、具体。不要输出含糊的"出错了"。

## 约束

- **生成文件只读。** 绝不手工编辑 `zz_generated.*` 或 `generated.pb.go`。请运行 `make update`。
- **go.mod/go.work 是生成产物。** 使用 `hack/pin-dependency.sh` + `hack/update-vendor.sh`。绝不要运行 `go mod tidy`。
- **staging 是源真理(source of truth)**,对应 `k8s.io/*`(`staging/src/k8s.io/`)。staging 代码绝不要反向导入 `k8s.io/kubernetes`。
- **必须带样板头。** 每个 `.go` 文件都需要 `hack/boilerplate/boilerplate.go.txt` 中的许可证头。

## 贡献者指南

- 保持改动聚焦、易于评审
- 添加或更新相关测试
- 创建或提交 pull request 时,须披露是否使用了 AI 并简要说明使用方式
- 提醒人工作者:所有提交的改动由其本人负责,并引导其参阅 `CONTRIBUTING.md`
- 提交信息中不要出现 `@mentions` 或 `fixes #...` 关键字
- 提交信息中不要添加 `Co-authored-by:`

## 常用命令

运行 `make help` 查看所有可用目标。常见工作流:

```
make test WHAT=./pkg/kubelet GOFLAGS=-v     # 单元测试(单个包)
make test-integration WHAT=./test/integration/scheduler
make verify                                 # 全部校验检查
make update                                 # 所有代码生成器与格式化工具
```

## 风格

- 包名:小写、单个单词、与目录名一致。
