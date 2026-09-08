> 🌐 本文档由 [kubernetes/kubernetes](https://github.com/kubernetes/kubernetes) 翻译,英文原版见原项目。

# client-go 内幕

[client-go](https://github.com/kubernetes/client-go/) 库包含多种机制,可在开发自定义控制器时使用。这些机制定义在该库的 [tools/cache 目录](https://github.com/kubernetes/client-go/tree/master/tools/cache)中。

下图展示了 client-go 库中各组件如何协同工作,以及它们与你要编写的自定义控制器代码之间的交互点。

<p align="center">
  <img src="images/client-go-controller-interaction.jpeg" height="600" width="700"/>
</p>

## client-go 组件

* Reflector:反射器,定义于[*cache* 包的 *Reflector* 类型](https://github.com/kubernetes/client-go/blob/master/tools/cache/reflector.go),通过 watch 监听 Kubernetes API 中指定资源类型(kind)的变化。完成这一工作的函数是 *ListAndWatch*。watch 的对象可以是内建资源,也可以是自定义资源。当 Reflector 通过 watch API 收到"存在新资源实例"的通知时,会在 *watchHandler* 函数中通过相应的 list API 获取新建的对象,并将其放入 Delta FIFO 队列。

* Informer:informer 定义于[*cache* 包的基础控制器](https://github.com/kubernetes/client-go/blob/master/tools/cache/controller.go),负责从 Delta FIFO 队列弹出对象,对应函数是 *processLoop*。这个基础控制器的职责是保存对象供后续检索,并调用我们的控制器、把对象传递给它。

* Indexer:索引器为对象提供索引功能,定义于[*cache* 包的 *Indexer* 类型](https://github.com/kubernetes/client-go/blob/master/tools/cache/index.go)。典型的索引用例是基于对象标签创建索引。Indexer 可以基于多个索引函数维护多套索引。Indexer 使用线程安全的数据存储来保存对象及其 key。[*cache* 包的 *Store* 类型](https://github.com/kubernetes/client-go/blob/master/tools/cache/store.go)中定义了一个名为 *MetaNamespaceKeyFunc* 的默认函数,它为对象生成 `<namespace>/<name>` 形式的 key。

## 自定义控制器组件

* Informer 引用:指向 Informer 实例的引用,该实例知道如何处理你的自定义资源对象。你的自定义控制器代码需要创建合适的 Informer。

* Indexer 引用:指向 Indexer 实例的引用,该实例知道如何处理你的自定义资源对象。它同样需要由你的控制器代码创建。你将使用这个引用来检索对象、进行后续处理。

client-go 的基础控制器提供了 *NewIndexerInformer* 函数来创建 Informer 和 Indexer。在你的代码中,既可以[直接调用该函数](https://github.com/kubernetes/client-go/blob/master/examples/workqueue/main.go#L174),也可以[使用工厂方法创建 informer](https://github.com/kubernetes/sample-controller/blob/master/main.go#L61)。

* 资源事件处理器:这是一组回调函数,Informer 想把对象投递给你的控制器时会调用它们。编写这些函数的典型模式是:取出被分发对象的 key,把该 key 放入工作队列等待进一步处理。

* 工作队列:这是你在控制器代码中创建的队列,用于把对象的投递与处理解耦。资源事件处理器的职责是提取被投递对象的 key 并加入工作队列。

* 处理条目(Process Item):这是你在代码中创建的、从工作队列取条目进行处理的函数。实际处理逻辑可以由一个或多个其他函数完成,它们通常借助 [Indexer 引用](https://github.com/kubernetes/client-go/blob/master/examples/workqueue/main.go#L73)或 list 包装器,根据 key 检索对应的对象。
