> 🌐 本文档由 [kubernetes/kubernetes](https://github.com/kubernetes/kubernetes) 翻译,英文原版见原项目。

# 安装 client-go

## 使用最新版本

如果你想使用本库的最新版本,请使用 go1.16+ 并运行:

```sh
go get k8s.io/client-go@latest
```

这会在你的 go module 中记录对 `k8s.io/client-go` 的依赖。
之后你就可以在项目中导入并使用 `k8s.io/client-go` 的 API 了。
下次执行 `go build`、`go test` 或 `go run` 时,
`k8s.io/client-go` 及其依赖会被(按需)下载,
详细的依赖版本信息会写入你的 `go.mod` 文件
(你也可以直接运行 `go mod tidy` 来完成这一步)。

## 使用指定版本

如果你想使用 `k8s.io/client-go` 库的某个特定版本,
可以这样声明项目所需的 `client-go` 版本:

- 如果你使用的 Kubernetes 版本 >= `v1.17.0`,请使用对应的 `v0.x.y` 标签。
  例如,`k8s.io/client-go@v0.20.4` 对应 Kubernetes `v1.20.4`:

```sh
go get k8s.io/client-go@v0.20.4
```

- 如果你使用的 Kubernetes 版本 < `v1.17.0`,请使用对应的 `kubernetes-1.x.y` 标签。
  例如,`k8s.io/client-go@kubernetes-1.16.3` 对应 Kubernetes `v1.16.3`:

```sh
go get k8s.io/client-go@kubernetes-1.16.3
```

之后你就可以在项目中导入并使用 `k8s.io/client-go` 的 API 了。
下次执行 `go build`、`go test` 或 `go run` 时,
`k8s.io/client-go` 及其依赖会被(按需)下载,
详细的依赖版本信息会写入你的 `go.mod` 文件
(你也可以直接运行 `go mod tidy` 来完成这一步)。

## 故障排查

### Go 1.16 之前的版本

如果你收到类似
`module k8s.io/client-go@latest found (v1.5.2), but does not contain package k8s.io/client-go/...`
的消息,说明你很可能在使用 1.16 之前的 go 版本,必须显式指定所需的 k8s.io/client-go 版本。
例如:
```sh
go get k8s.io/client-go@v0.20.4
```

### 旧版 client-go 的依赖要求冲突

如果你收到类似
`module k8s.io/api@latest found, but does not contain package k8s.io/api/auditregistration/v1alpha1`
的消息,说明你的构建链路中很可能有东西在要求旧版 `k8s.io/client-go`(例如 `v11.0.0+incompatible`)。

首先,尝试获取更新的版本。例如:
```sh
go get k8s.io/client-go@v0.20.4
```

如果问题仍未解决,查一下是谁在要求 `...+incompatible` 版本的 client-go,
并尽可能把那个库升级到更新的版本:
```sh
go mod graph | grep " k8s.io/client-go@"
```

万不得已时,你可以强制构建使用特定版本的 client-go,
即使部分依赖仍然想要 `...+incompatible` 版本。例如:
```sh
go mod edit -replace=k8s.io/client-go=k8s.io/client-go@v0.20.4
go get k8s.io/client-go@v0.20.4
```

### Go modules 未启用

如果你收到类似 `cannot use path@version syntax in GOPATH mode` 的消息,
说明你的 go modules 很可能没有启用。在所有受支持的 Go 版本中,
它默认应当是开启的。

```sh
export GO111MODULE=on
```

确保你的项目根目录下定义了 `go.mod` 文件。
如果还没有,`go mod init` 会帮你创建一个:

```sh
go mod init
```
