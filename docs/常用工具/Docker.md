# Docker

## Docker 下载镜像

如果我们想要在本地运行容器，就必须保证本地存在对应的镜像。所以，第一步，我们需要下载镜像。当我们尝试下载镜像时，Docker 会尝试先从默认的镜像仓库（默认使用 Docker Hub 公共仓库）去下载，当然了，用户也可以自定义配置想要下载的镜像仓库。

#### 1.1 下载镜像

镜像是运行容器的前提，我们可以使用 `docker pull[IMAGE_NAME]:[TAG]`命令来下载镜像，其中 `IMAGE_NAME` 表示的是镜像的名称，而 `TAG` 是镜像的标签，也就是说我们需要通过 “**镜像 + 标签**” 的方式来下载镜像。

**注意：** 您也可以不显式地指定 TAG, 它会默认下载 latest 标签，也就是下载仓库中最新版本的镜像。这里并不推荐您下载 latest 标签，因为该镜像的内容会跟踪镜像的最新版本，并随之变化，所以它是不稳定的。在生产环境中，可能会出现莫名其妙的 bug, 推荐您最好还是显示的指定具体的 TAG。

举个例子，如我们想要下载一个 Mysql 5.7 镜像，可以通过命令来下载：

```shell
docker pull mysql:5.7
```

**注意：** 由于官方 DockerHub 仓库服务器在国外，下载速度较慢

#### 1.2 验证

让我们来验证一下，本地是否存在 Mysql5.7 的镜像，运行命令：

```shell
docker images
```

可以看到本地的确存在该镜像，则确实是下载成功了！

#### 1.3 下载镜像相关细节

通过下载过程，可以看到，一个镜像一般是由多个层（ `layer`） 组成，类似 `f7e2b70d04ae`这样的串表示层的唯一 ID（实际上完整的 ID 包括了 256 个 bit, 64 个十六进制字符组成）。

**您可能会想，如果多个不同的镜像中，同时包含了同一个层（ layer）,这样重复下载，岂不是导致了存储空间的浪费么?**

实际上，Docker 并不会这么傻会去下载重复的层（ `layer`）,Docker 在下载之前，会去检测本地是否会有同样 ID 的层，如果本地已经存在了，就直接使用本地的就好了。

**另一个问题，不同仓库中，可能也会存在镜像重名的情况发生, 这种情况咋办？**

严格意义上，我们在使用 `docker pull` 命令时，还需要在镜像前面指定仓库地址( `Registry`), 如果不指定，则 Docker 会使用您默认配置的仓库地址。例如配置的是国内 `docker.io` 的仓库地址，我在 `pull` 的时候，docker 会默认为我加上 `docker.io/library` 的前缀。

如：当我执行 `docker pull mysql:5.7` 命令时，实际上相当于 `docker pull docker.io/mysql:5.7`，如果您未自定义配置仓库，则默认在下载的时候，会在镜像前面加上 DockerHub 的地址。

Docker 通过前缀地址的不同，来保证不同仓库中，重名镜像的唯一性。

#### 1.4 PULL 子命令

命令行中输入：

```shell
docker pull --help
```

会得到如下信息：

```shell
slimshady@MacBook-Pro ~ % docker pull --help

Usage:	docker pull [OPTIONS] NAME[:TAG|@DIGEST]

Pull an image or a repository from a registry

Options:
  -a, --all-tags                Download all tagged images in the repository
      --disable-content-trust   Skip image verification (default true)
  -q, --quiet                   Suppress verbose output
```

我们可以看到主要支持的子命令有：

1.  `-a,--all-tags=true|false`: 是否获取仓库中所有镜像，默认为否；
2.  `--disable-content-trust`: 跳过镜像内容的校验，默认为 true;

## 查看镜像信息

#### 2.1 images 命令列出镜像

通过使用如下两个命令，列出本机已有的镜像：

```shell
docker images
```

或：

```shell
docker image ls
```

如下图所示：

![image-20200525164236760](https://tva1.sinaimg.cn/large/007S8ZIlgy1gf4sejxos4j31ma054td9.jpg)

解释：

*   **REPOSITORY**: 来自于哪个仓库；
*   **TAG**: 镜像的标签信息，比如 5.7、latest 表示不同的版本信息；
*   **IMAGE ID**: 镜像的 ID, 如果您看到两个 ID 完全相同，那么实际上，它们指向的是同一个镜像，只是标签名称不同罢了；
*   **CREATED**: 镜像最后的更新时间；
*   **SIZE**: 镜像的大小，优秀的镜像一般体积都比较小，这也是我更倾向于使用轻量级的 alpine 版本的原因；

>   注意：图中的镜像大小信息只是逻辑上的大小信息，因为一个镜像是由多个镜像层（ `layer`）组成的，而相同的镜像层本地只会存储一份，所以，真实情况下，占用的物理存储空间大小，可能会小于逻辑大小。

#### 2.2 使用 tag 命令为镜像添加标签

通常情况下，为了方便在后续工作中，快速地找到某个镜像，我们可以使用 `docker tag` 命令，为本地镜像添加一个新的标签。`docker tag` 命令功能更像是, 为指定镜像添加快捷方式一样。

标签和原镜像的镜像 ID 是一模一样的，说明它们是同一个镜像，只是别名不同而已。

#### 2.3 使用 inspect 命令查看镜像详细信息

通过 `docker inspect` 命令，我们可以获取镜像的详细信息，其中，包括创建者，各层的数字摘要等。

```shell
docker inspect IMAGE ID
```

`docker inspect` 返回的是 `JSON` 格式的信息，如果您想获取其中指定的一项内容，可以通过 `-f` 来指定，如获取镜像大小：

```shell
docker inspect -f {{".Size"}} IMAGE ID
```

#### 2.4 使用 history 命令查看镜像历史

前面的小节中，我们知道了，一个镜像是由多个层（layer）组成的，那么，我们要如何知道各个层的具体内容呢？

通过 `docker history` 命令，可以列出各个层（layer）的创建信息：

```shell
docker history IMAGE ID
```

![image-20200525170130201](https://tva1.sinaimg.cn/large/007S8ZIlgy1gf4t03poegj31mm0kw4kn.jpg)

可以看到，上面过长的信息，为了方便展示，后面都省略了，如果您想要看具体信息，可以通过添加 `--no-trunc` 选项，如下面命令：

```shell
docker history --no-trunc IMAGE ID
```

## 搜索镜像

#### 3.1 search 命令

您可以通过下面命令进行搜索：

```shell
docker search [option] keyword
```

比如，您想搜索仓库中 `mysql` 相关的镜像，可以输入如下命令：

```shell
docker search mysql
```

#### 3.2 search 子命令

命令行输入 `docker search --help`, 输出如下：

```shell
slimshady@MacBook-Pro ~ % docker search --help

Usage:	docker search [OPTIONS] TERM

Search the Docker Hub for images

Options:
  -f, --filter filter   Filter output based on conditions provided
      --format string   Pretty-print search using a Go template
      --limit int       Max number of search results (default 25)
      --no-trunc        Don't truncate output
```

可以看到 `search` 支持的子命令有：

*   `-f,--filter filter`: 过滤输出的内容；
*   `--limit int`：指定搜索内容展示个数;
*   `--no-trunc`：不截断输出内容；

举个列子，比如我们想搜索官方提供的 mysql 镜像，命令如下：

```
docker search --filter=is-official=true mysql
```

![image-20200525170645573](https://tva1.sinaimg.cn/large/007S8ZIlgy1gf4t3o28nsj31nk042n0v.jpg)

再比如，我们想搜索 Stars 数超过 100 的 mysql 镜像：

```
docker search --filter=stars=100 mysql
```



## 删除镜像

#### 4.1 通过标签删除镜像

通过如下两个都可以删除镜像：

```shell
docker rmi [image]
```

或者：

```shell
docker image rm [image]
```

支持的子命令如下：

*   `-f,-force`: 强制删除镜像，即便有容器引用该镜像；
*   `-no-prune`: 不要删除未带标签的父镜像；

对于带有标签的镜像：

**实际上，当同一个镜像拥有多个标签时，执行 `docker rmi` 命令，只是会删除了该镜像众多标签中，您指定的标签而已，并不会影响原始的那个镜像文件。**如果某个镜像不存在多个标签，当且仅当只有一个标签时，执行删除命令时，您就要小心了，这会彻底删除镜像。

#### 4.2 通过 ID 删除镜像

除了通过标签名称来删除镜像，我们还可以通过制定镜像 ID, 来删除镜像，如：

```shell
docker rmi IMAGE ID
```

**一旦制定了通过 ID 来删除镜像，它会先尝试删除所有指向该镜像的标签，然后在删除镜像本身。**

#### 4.3 删除镜像的限制

删除镜像很简单，但也不是我们何时何地都能删除的，它存在一些限制条件。

当通过该镜像创建的容器未被销毁时，镜像是无法被删除的。因为有容器正在引用他。同时，可以通过添加 `-f` 子命令，也就是强制删除，才能移除掉该镜像。

```shell
docker rmi -f IMAGE ID
```

但是，我们一般不推荐这样暴力的做法，正确的做法应该是：

1.  先删除引用这个镜像的容器；
2.  再删除这个镜像；

#### 4.4 清理镜像

我们在使用 Docker 一段时间后，系统一般都会残存一些临时的、没有被使用的镜像文件，可以通过以下命令进行清理：

```shell
docker image prune
```

它支持的子命令有：

*   `-a,--all`: 删除所有没有用的镜像，而不仅仅是临时文件；
*   `-f,--force`：强制删除镜像文件，无需弹出提示确认；

## 创建镜像

Docker 创建镜像主要有三种：

1.  基于已有的镜像创建；
2.  基于 Dockerfile 来创建；
3.  基于本地模板来导入；

我们将主要介绍常用的 1，2 两种。

#### 5.1 基于已有的镜像创建

通过如下命令来创建：

```
docker container commit
```

支持的子命令如下：

*   `-a,--author`="": 作者信息；
*   `-c,--change`=[]: 可以在提交的时候执行 Dockerfile 指令，如 CMD、ENTRYPOINT、ENV、EXPOSE、LABEL、ONBUILD、USER、VOLUME、WORIR 等；
*   `-m,--message`="": 提交信息；
*   `-p,--pause`=true: 提交时，暂停容器运行。

#### 5.2 基于 Dockerfile 创建

通过 Dockerfile 的方式来创建镜像，是最常见的一种方式了，也是比较推荐的方式。Dockerfile 是一个文本指令文件，它描述了是如何基于一个父镜像，来创建一个新镜像的过程。

## 导出&加载镜像

通常我们会有下面这种需求，需要将镜像分享给别人，这个时候，我们可以将镜像导出成 tar 包，别人直接通过加载这个 tar 包，快速地将镜像引入到本地镜像库。

要想使用这两个功能，主要是通过如下两个命令：

1.  `docker save`
2.  `docker load`

#### 6.1 导出镜像

```
docker save -o IMAGE ID
```

#### 6.2 加载镜像

别人拿到了这个 `tar` 包后，要如何导入到本地的镜像库呢？

通过执行如下命令：

```
docker load -i xxx.tar
```

或者：

```
docker load < xxx.tar
```

导入成功后，查看本地镜像信息，你就可以获得别人分享的镜像了。

