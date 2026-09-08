> 🌐 本文档由 [kubernetes/kubernetes](https://github.com/kubernetes/kubernetes) 翻译,英文原版见原项目。

# 使用 kube-up 在 GCE 上启动 Windows Kubernetes 集群

## 重要提示

每当 `windows` 目录下的文件结构发生变化时,必须手工同步更新 `windows/BUILD` 和 `k8s.io/release/lib/releaselib.sh`。我们强烈建议不要改动文件结构,因为 Kubernetes 发布产物的使用方依赖发布结构保持稳定。

## 启动集群

前置条件:一个 Google Cloud Platform 项目。

### 0. 准备环境

在 Linux 机器上把本仓库克隆到 `$GOPATH/src` 目录下。然后,可选地用以下命令清理/准备环境:

```bash
# Remove files that interfere with get-kube / kube-up:
rm -rf ./kubernetes/; rm -f kubernetes.tar.gz; rm -f ~/.kube/config

# To run e2e test locally, make sure "Application Default Credentials" is set in any of the places:
# References: https://cloud.google.com/sdk/docs/authorizing#authorizing_with_a_service_account
#             https://cloud.google.com/sdk/gcloud/reference/auth/application-default/
#    1. $HOME/.config/gcloud/application_default_credentials.json, if doesn't exist, run this command:
gcloud auth application-default login
# Or 2. Create a json format credential file as per http://cloud/docs/authentication/production,
#       then export to environment variable
export GOOGLE_APPLICATION_CREDENTIAL=[path_to_the_json_file]
```

### 1. 构建 Kubernetes

注意:只有当你想测试自己对代码库的本地改动时才需要这一步。

构建这些二进制最直接的方式是运行 `make release`。但它会为所有受支持平台构建二进制,可能很慢。你可以按照下面的说明只构建必要的二进制来加速:

```bash
# Build binaries for both Linux and Windows:
KUBE_BUILD_PLATFORMS="linux/amd64 windows/amd64" make quick-release
```

### 2. 创建 Kubernetes 集群

你可以创建常规 Kubernetes 集群或端到端测试集群。

只有端到端测试集群支持运行 Kubernetes e2e 测试(因为 [e2e 集群创建](https://github.com/kubernetes/kubernetes/blob/b632eaddbaad9dc1430d214d506b72750bbb9f69/hack/e2e-internal/e2e-up.sh#L24)和 [e2e 测试脚本](https://github.com/kubernetes/kubernetes/blob/b632eaddbaad9dc1430d214d506b72750bbb9f69/hack/ginkgo-e2e.sh#L42)都是基于 `cluster/gce/config-test.sh` 配置的),它还启用了诸如 Windows 节点 SSH 访问等调试特性。

请确保你已按上一节的说明正确设置了环境变量。

首先,设置以下环境变量,它们用于控制集群中 Linux 与 Windows 节点的数量,并启用 IP 别名(Windows Pod 路由所需)。至少需要 1 个 Linux 工作节点,推荐 2 个,因为许多默认集群插件(如 `kube-dns`)需要在 Linux 节点上运行。主控平面只在 Linux 上运行。

```bash
export NUM_NODES=2  # number of Linux nodes
export NUM_WINDOWS_NODES=2
export KUBE_GCE_ENABLE_IP_ALIASES=true
export KUBERNETES_NODE_PLATFORM=windows
export LOGGING_STACKDRIVER_RESOURCE_TYPES=new
```

然后用以下两种方法之一启动集群:

#### 2a. 创建常规 Kubernetes 集群

确保你的 GCP 认证仍然有效:

```bash
gcloud auth application-default login
gcloud auth login
```

携带这些环境变量调用 kube-up.sh:

```bash
# WINDOWS_NODE_OS_DISTRIBUTION: the Windows version you want your nodes to
#   run, e.g. win2019 or win1909.
# KUBE_UP_AUTOMATIC_CLEANUP (optional): cleans up existing cluster without
#   prompting.
WINDOWS_NODE_OS_DISTRIBUTION=win2019 KUBE_UP_AUTOMATIC_CLEANUP=true ./cluster/kube-up.sh
```

如果你的 GCP 项目配置了双因素认证,可能需要在运行 `kube-up` 后不久点击一下安全密钥。

销毁集群请运行:

```bash
./cluster/kube-down.sh
```

如果你想同时运行多个集群,可以使用两个不同的 GCP 项目,并:

1.  为每个项目/集群使用独立的 shell。
2.  在每个 shell 中把 `CLOUDSDK_CORE_PROJECT` 环境变量设为要使用的 GCP 项目。该变量会覆盖当前 gcloud 配置。
3.  在 `kube-up.sh` 和 `kube-down.sh` 命令前加上 `PROJECT=${CLOUDSDK_CORE_PROJECT}`。

#### 2b. 创建 Kubernetes 端到端(E2E)测试集群

如果你已按步骤 1 构建了自己的发布二进制,运行以下命令启动用于运行 K8s e2e 测试的集群。最新环境变量请参考 [windows-gce](https://github.com/kubernetes/test-infra/blob/master/config/jobs/kubernetes/sig-windows/windows-gce.yaml) 的 e2e 测试配置。

```bash
KUBE_GCE_ENABLE_IP_ALIASES=true KUBERNETES_NODE_PLATFORM=windows \
  LOGGING_STACKDRIVER_RESOURCE_TYPES=new NUM_NODES=2 \
  NUM_WINDOWS_NODES=3 WINDOWS_NODE_OS_DISTRIBUTION=win2019 \
  ./hack/e2e-internal/e2e-up.sh
```

如果已存在 e2e 集群,该命令会提示你先销毁再新建。只销毁现有 e2e 集群请运行:

```bash
./hack/e2e-internal/e2e-down.sh
```

无论选择创建哪种集群,结果都是一个包含 1 个 Linux 主节点、`NUM_NODES` 个 Linux 工作节点和 `NUM_WINDOWS_NODES` 个 Windows 工作节点的 Kubernetes 集群。

## 验证集群

调用该脚本运行冒烟测试,验证集群是否正确启动:

```bash
cluster/gce/windows/smoke-test.sh
```

有时冒烟测试第一次会失败,因为拉取 Windows 测试容器耗时过长。通常重试一次即可通过。

## 针对集群运行 e2e 测试

如果你用上述步骤启动了端到端测试集群,就可以按下面的步骤运行 K8s e2e 测试。这些步骤基于 [kubernetes-sigs/windows-testing](https://github.com/kubernetes-sigs/windows-testing)。

*   构建必要的测试二进制。每次修改测试代码后都必须重新构建。

    ```bash
    make WHAT=test/e2e/e2e.test
    ```

*   设置必要的环境变量并获取 `run-e2e.sh` 脚本:

    ```bash
    export KUBECONFIG=~/.kube/config
    export WORKSPACE=$(pwd)
    export ARTIFACTS=${WORKSPACE}/e2e-artifacts

    curl \
      https://raw.githubusercontent.com/kubernetes-sigs/windows-testing/master/gce/run-e2e.sh \
      -o ${WORKSPACE}/run-e2e.sh
    chmod u+x run-e2e.sh

    # Fetch a prepull manifest for the k8s version you're using.
    curl \
      https://raw.githubusercontent.com/kubernetes-sigs/windows-testing/master/gce/prepull-1.21.yaml \
      -o ${WORKSPACE}/prepull-head.yaml
    ```

    e2e 测试脚本对 k8s 仓库路径有一些讨厌的假设。如果你的 `~/go/src/k8s.io/kubernetes` 目录实际上是指向 `~/go/src/github.com/<username>/kubernetes` 的符号链接,请再创建这个额外的符号链接:

    ```bash
    cd ~/go/src/github.com; ln -s . github.com
    ```

    没有这个额外符号链接,调用 `run-e2e.sh` 脚本时可能收到该错误:

    ```bash
    chdir ../../github.com/<username>/kubernetes/_output/bin: no such file or directory
    ```

*   针对 GCE 上的集群运行全部 Windows e2e 测试的标准参数,可以在 `ci-kubernetes-e2e-windows-gce` 持续集成测试任务的[测试配置](https://github.com/kubernetes/test-infra/blob/master/config/jobs/kubernetes/sig-windows/windows-gce.yaml#L78)中搜索 `--test-cmd-args` 看到。这些参数应传给 `run-e2e` 脚本;ginkgo 参数要用引号包起来转义。例如:

    ```bash
    ./run-e2e.sh --node-os-distro=windows --minStartupPods=8 \
      --ginkgo.focus="\[Conformance\]|\[NodeConformance\]|\[sig-windows\]" \
      --ginkgo.skip="\[LinuxOnly\]|\[Serial\]|\[Feature:.+\]" \
      --ginkgo.parallel.total=8    # TODO: does this flag actually help?
    ```

    如果遇到认证错误,可能需要重新认证:

    ```bash
    gcloud auth application-default login
    gcloud auth login
    ```

*   运行单个测试:把 ginkgo focus 设为匹配你的测试名;例如,"DNS should provide DNS for the cluster" 测试可以这样运行:

    ```bash
    ./run-e2e.sh --node-os-distro=windows \
      --ginkgo.focus="provide\sDNS\sfor\sthe\scluster"
    ```

    针对 Windows 节点测试时务必始终带上 `--node-os-distro=windows`。

测试运行完成后,日志文件位于 `${ARTIFACTS}` 目录下。

## E2E 测试

创建 pull request 后,你可以评论 `/test pull-kubernetes-e2e-windows-gce` 来运行覆盖本目录改动的集成测试。
