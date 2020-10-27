---
id: operational_faq.md
---

# 部署运维问题



<div class="faq-header" id="1">如果在安装 Milvus 时，从 Docker Hub 拉取镜像总是失败怎么办？</div>

某些地区的用户可能无法快速访问 Docker Hub。如果拉取镜像失败，可以从其它的镜像源拉取镜像。比如中国镜像源的网址为 [registry.docker-cn.com](https://registry.docker-cn.com)。可以在 **/etc/docker/daemon.json** 文件的 `registry-mirrors` 组添加 `"https://registry.docker-cn.com"` 命令，这样就可以默认从中国镜像源拉取镜像了。

```json
{
  "registry-mirrors": ["https://registry.docker-cn.com"]
}
```

<div class="faq-header" id="2">Milvus 只能使用 Docker 部署吗？</div>

Milvus 还支持源码编译，该方法仅支持 Linux 系统。详见 [从源代码编译 Milvus](https://github.com/milvus-io/milvus/blob/master/INSTALL.md)。


<div class="faq-header" id="3">为什么 Milvus 返回 <code>config check error</code> 的错误？</div>

Milvus 和服务端配置文件的版本不对应。很可能是因为你没有下载正确版本的**milvus.yaml** 配置文件，详见 [下载配置文件](milvus_docker-cpu.md#下载配置文件)。


<div class="faq-header" id="4">为什么在导入数据时 Milvus 显示 <code>no space left on device</code> 的错误？</div>

可能是没有为导入数据预留足够的磁盘空间。


<div class="faq-header" id="5">为什么 Milvus 查询召回率一直不理想？</div>

在调用 SDK 进行向量搜索时，可以增大函数中 `nprobe` 参数的值。值越大，结果越精确，但耗时也越久。详见 [如何设置 Milvus 客户端参数](https://www.milvus.io/cn/blogs/2020-2-16-api-setting.md)。

<div class="faq-header" id="6">为什么更新过的设置没有生效？</div>

每次更新配置文件之后必须重启 Milvus Docker 才能让改动生效。详见 [服务端配置 > 配置修改](milvus_config.md#配置修改)。

<div class="faq-header" id="7">如何得知我的 Milvus 已经成功启动？</div>

使用 `sudo docker logs <container ID>` 命令检查 Milvus 的运行状态。

<div class="faq-header" id="8">为什么我的日志文件时间与系统时间不一致？</div>

Docker 镜像内部的日志文件默认使用 UTC 时间。如果宿主机未使用 UTC 时间，就会出现日志文件时间与系统时间不一致的情况。建议在宿主机上挂载日志文件，这样可以保证宿主机上的日志文件和系统时间是一致的。

<div class="faq-header" id="9">如何确认 Milvus 是否支持我的 CPU？</div>

目前，Milvus 支持的指令集有：SSE42、AVX、AVX2、AVX512。你的 CPU 必须支持其中任意一个指令集才能保证 Milvus 正常工作。

<div class="faq-header" id="10">为什么 Milvus 在启动时返回 <code>Illegal instruction</code>？</div>

如果你的 CPU 不支持 SSE42、AVX、AVX2、AVX512 其中任何一个指令集，则 Milvus 无法正常启动。可以通过 `cat /proc/cpuinfo` 查看 CPU 支持的指令集。


<div class="faq-header" id="11">如何确认 Milvus 是否支持我的 GPU？</div>

Milvus 支持 CUDA 6.0 架构以后的显卡。关于 Milvus 支持的架构，详见 [Wikipedia](https://en.wikipedia.org/wiki/CUDA)。

<div class="faq-header" id="12">Milvus 镜像里面启动 server 的脚本在哪？</div>

启动 server 的脚本在 Milvus 容器内的 **/var/lib/milvus/script/** 路径下。

<div class="faq-header" id="13">除了配置文件外，怎样可以判断我确实在使用 GPU 做 search？</div>

有以下三种方式：

- 使用 `nvidia-smi` 命令查看 GPU 使用情况。
- 用 Prometheus 配置，详见 [使用 Grafana 展示监控指标 > 系统运行指标](setup_grafana.md#系统运行指标)。
- 使用 Milvus 服务端的日志。

<div class="faq-header" id="14">可以在 Windows 上安装 Milvus 吗？</div>

理论上只要能够支持 Docker 的操作系统都可以运行 Milvus。

<div class="faq-header" id="15">在 Windows 安装 pymilvus 报错，如何解决？</div>

可以尝试在 Conda 环境下安装。

<div class="faq-header" id="16">内网环境，即离线方式，能否部署 Milvus 服务？</div>

Milvus 是以 Docker 镜像形式发行的，是可以离线部署的：

1. 在有网的环境中拉取最新的 Milvus 镜像；
2. 使用 `docker save` 将镜像保存为 TAR 文件；
3. 拷贝该镜像到无网的环境中；
4. 用 `docker load` 命令导入该镜像。

关于 Docker 的使用详见 [docs.docker.com](https://docs.docker.com)。

<div class="faq-header" id="17">在多个 Milvus 节点接入 Pushgateway 的情况下如何进行区分数据来源？</div>

在 **prometheus.yaml** 里面加一个 Prometheus 的实例就可以。最后在 Prometheus 或者 Grafana 里面显示监控的时候，会指明数据是来自哪个 Milvus 实例。

<div class="faq-header" id="18">我应该使用 SQLite 还是 MySQL 进行元数据管理？</div>

生产环境下，推荐使用 MySQL 。

<div class="faq-header" id="19">如何根据数据量计算需要多大的内存？</div>

不同的索引所需内存不同。可以使用 [Milvus 的 sizing 工具](https://milvus.io/tools/sizing) 去计算查询时所需要的内存。

<div class="faq-header" id="20">Milvus 中如何实现数据迁移？</div>

把原有的 Milvus 服务的整个 **db** 目录拷贝到新的路径下，启动新的 Milvus 服务时，将该 Milvus 服务的 **db** 目录映射为刚拷贝过来的 **db** 目录。
> 注意：不同版本之间，数据可能会不兼容。目前数据格式兼容到 0.7.0。

<div class="faq-header" id="21">Milvus 可以通过扩展某些接口 (如 S3 接口或 GlusterFS 接口) 来扩展存储吗？</div>

目前暂不支持。

<div class="faq-header" id="22">出现 <code>database is locked</code> 的报错怎么解决？</div>

如果元数据管理用的是 SQLite，在有数据频繁写入的情况下会出现该错误。建议将 SQLite 更换为 MySQL。如何更换请参考文档 [使用 MySQL 管理元数据](data_manage.md)。



<div class="faq-header" id="23">仍有问题没有得到解答？</div>

如果仍有其他问题，你可以：

- 在 GitHub 上访问 [Milvus](https://github.com/milvus-io/milvus/issues)，提问、分享、交流，帮助其他用户。
- 加入我们的 [Slack 社区](https://join.slack.com/t/milvusio/shared_invite/enQtNzY1OTQ0NDI3NjMzLWNmYmM1NmNjOTQ5MGI5NDhhYmRhMGU5M2NhNzhhMDMzY2MzNDdlYjM5ODQ5MmE3ODFlYzU3YjJkNmVlNDQ2ZTk)，与其他用户讨论交流。