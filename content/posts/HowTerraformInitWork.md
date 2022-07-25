---
title: terraform源码解析（init命令）
date: 2022-07-25T00:00:00+08:00
comments: true
toc: true
tags:
 - Terraform
---

开个新坑，慢慢填～ 主要目的还是想把源码吃透以后把Terraform的内核吃掉，这样我们可以仅依赖于它的模板解析和grpc调用provider去生成资源

在Goland里，如果想调试Terraform代码，通过一些断点来挖掘深层次的逻辑，通过如下代码的提示，我们可以通过设置环境变量`TF_FORK=0`来达到这一目的

```go
func main() {
	os.Exit(realMain())
}

func realMain() int {
	var wrapConfig panicwrap.WrapConfig

	// don't re-exec terraform as a child process for easier debugging
	if os.Getenv("TF_FORK") == "0" {
		return wrappedMain()
	}

    ...
}
```

在进入 initCommand 之前，Terraform会在本地的目录上搜索已经存在的provider并记录到如下的结构中

![terraform-init-1](https://syasusu-blog-store.oss-cn-hangzhou.aliyuncs.com/terraform-code-review/terraform-init-1.png)

initCommand里面都在做一些模板的terraform block的解析，state的解析，backend的构建，关键在`getProviders`中的 `EnsureProviderVersions`