# 使用手册

> ver 1.0.0_BD

## 操作指南

要特别说明的是，此部分会在**正式工作人员（副部）**阶段进行**一对四**培训，无需提前学习。本文档只用于辅助教学，起到讲义与快捷复制指令的作用，并不会有详细完整的流程，如有此类需求建议访问 [Nonebot2 官方文档](https://nonebot.dev/)。

<!--

如果你是**技术部工作人员/部长**，如果有需求，可以在任何情况下[联系我](http://localhost:3000/#/./README?id=%e8%81%94%e7%b3%bb%e6%96%b9%e5%bc%8f)获取远程帮助！

 -->

如果你是**操作者**，可以通过如下方式快速部署一个机器人：

### 前期准备

想要部署一个可持续运行的机器人，你至少需要准备如下工具：

- 一台 win/mac 主机提供配置环境
- 一台服务器提供持续运行环境，他可以是你的主机、一台闲置的 linux/win/mac 机器又或是一台云服务器
- ~~你聪明又善于思考的大脑~~
- 当然，还要会在上一部分提到的[技能](README?id=对于操作者)

由于对于初学者，我们需要测试机、服务机两台机器，下文的每个步骤将分别依次从两台机器中进行搭建，代码块的角标代表了操作的环境，我们使用如下的配置进行搭建：

- 测试机：win10/11
- 服务机：Linux(Ubuntu2020)

如果你的两台机器都为一种环境，建议先在测试机上完整实践再重新在服务机实践。

### 环境部署

#### Python

Nonebot2 框架（以下简称“**nb**”）基于 python 编写，所以我们需要在安装 nb 前安装相应的 python 环境。需要注意的是，nb 只支持 **Python 3.8 以上**的版本。

<!-- tabs:start -->

#### **windows(10/11)**

在 windows 平台上，安装 Python 非常简单，只需要访问 [Python 官网地址](https://www.python.org/downloads/)，选择所需的版本下载安装即可。

一般来说，选择最新版本即可。

#### **linux(Ubuntu)**

在 linux 平台上，情况似乎变得不太一样了。使用`python3 --version`命令检查服务器中的 Python 版本，你可能会遇到如下几种情况：

1. 一些云服务器厂商自带 Python 3.8 以上环境（如阿里云），此时我们不需要再额外配置了
1. 一些服务器的服务器版本较低，特别是预装 Ubuntu2018 版本的服务器，他们的 Python 版本一般为 3.6.x，这时我们需要先**卸载**原有 Python3，再**重新安装**高版本的 Python
1. 一些服务器不会附带 Python3，此时我们可以**直接安装**高版本 Python

> 需要注意的是，在 Linux 中，Python 不会默认的指向某个大版本的 Python，我们需要在 Python 后加数字特别说明，如“python3”。
>
> 如果你想直接使用 Python 代指 Python3，可以安装“python-is-python3”。

对于**卸载** Python3，可以参考以下指令：

1、卸载 Python3.6：

```Ubuntu
sudo apt-get remove python3.6
```

2、卸载 Python3.6 及其依赖：

```Ubuntu
sudo apt-get remove --auto-remove python3.6
```

3、清除 python3.6：

```Ubuntu
sudo apt-get purge python3.6
```

对于**安装**高版本 Python3，借助多快好省的 Deadsnakes PPA，可以参考以下指令：

1、更新软件包列表并安装必备组件：

```Ubuntu
sudo apt update
sudo apt install software-properties-common
```

2、将 Deadsnakes PPA 添加到 ubuntu 系统的来源列表中：

```Ubuntu
sudo add-apt-repository ppa:deadsnakes/ppa
```

出现提示时，按“Enter”继续。

3、启用存储库后，可以通过执行以下命令安装 Python 3.11：

```Ubuntu
sudo apt install python3.11
```

<!-- tabs:end -->

至此，Python 环境就部署成功了。

~~这个过程可能会劝退一部分人 qwq~~

#### go-cqhttp

虽然可能没有学过计算机相关内容，但你难免对 Python 有所耳闻，不过 [go-cqhttp](https://github.com/Mrs4s/go-cqhttp) 你可能就完全没有听说过了。

这里对于 go-cqhttp 的过多介绍是没有必要的，简单来说，可以粗略理解为它是链接 bot 服务端与 QQ 的接口。

对于安装 go-cqhttp，其[官方文档](https://docs.go-cqhttp.org/guide/quick_start.html#%E5%9F%BA%E7%A1%80%E6%95%99%E7%A8%8B)已经非常详细，在此不做过多赘述。对于本教程，只需要完成官方文档中“下载”至“解压”的步骤即可，版本对应“64 位 Windows”与“64 位 Linux”。

至此，go-cqhttp 就部署成功了。

#### qsign

如果你还能在 go-cqhttp 中捕获“http”关键词，那 [qsign](https://github.com/fuqiuluo/unidbg-fetch-qsign) 对没有接触过 Qbot 的同学来说将是前所未见的。

qsign 的功能更加特殊化。因为 tx 会持续更新反机器人安全验证机制，所以 Qbot 的部署越来越不顺利。在 2023 年 6 月份左右，tx 更新了一些很安全的机制，导致大多数情况下无法直接用 go-cqhttp 连接 QQ。此时，qsign 就排上用场了，可以简单的理解为 qsign 为服务器登录 QQ 提供了 tx 所检测的安全保障，~~虽然我们明知这是不被允许的（划掉 awa）~~。

对于安装 qsign，其[官方文档](https://github.com/fuqiuluo/unidbg-fetch-qsign/wiki)给出的教程更加详细，直接在对应平台走完完整流程即可。

需要特别说明的是，qsign 需要 Java 环境。官方文档中亦有描述~~（对，环境套环境 awa）~~

至此，qsign 与所有环境就部署成功了。

### 安装 NoneBot2

经历了漫长的环境搭建，我们终于可以开始安装我们的 nb 了。

nb 的官方文档在前文已经给出，但它过于详细，我们用不到那么多的配置项，按照如下步骤安装即可：

#### 安装 nb-cli

nb-cli 是 nb 为操作者开发的快速搭建平台，它可以快速地初始化、修改、管理、删除 Qbot。想要安装它，官方推荐使用 pip 进行安装。

1、首先，使用 pip 安装 nb-cli：

```shell
pip3 install nb-cli
```

2、利用 cd 命令定位到想要存放 Qbot 程序的文件夹：

```shell
cd C:\MyQbot
```

这里的`C:\MyQbot`可以是任意文件夹地址。对于 linux，你可以直接选择在原地部署。

3、使用脚手架创建一个项目：

```shell
nb create
```

在输入词条命令后，会以问答式的询问配置，建议使用如下回答：

```
[?] 选择一个要使用的模板: bootstrap (初学者或用户)
[?] 项目名称: AIxbot
[?] 要使用哪些驱动器? FastAPI (FastAPI 驱动器)
[?] 要使用哪些适配器? Console (基于终端的交互式适配器)
[?] 立即安装依赖? (Y/n) y
[?] 创建虚拟环境? (Y/n) y
[?] 要使用哪些内置插件? echo
```

4、第一次启动 Qbot：

```shell
cd ./AIxbot
nb run
```

至此，你的第一个 Qbot 就创建完成了！

### 安装插件

nb 的插件非常多，我们可以在官方文档的[商店](https://nonebot.dev/store)中寻找所需的插件，这里我们只安装星宝所需的基础插件，依次在 Qbot 文件夹下输入以下指令即可：

```shell
nb plugin install nonebot-plugin-autoreply
nb plugin install nonebot-plugin-learning-chat
```

这两个插件是星宝所需要的基础插件，分别由两位大佬编写，他们的作用如下：

- [AutoReply](https://github.com/lgc-NB2Dev/nonebot-plugin-autoreply/)：可预设的自动回复，支持模糊、正则等多种匹配规则
- [learning-chat](https://github.com/CMHopeSunshine/nonebot-plugin-learning-chat)：简易的机器学习

除了上述插件外，还可以根据需要安装[黑白名单](https://github.com/A-kirami/nonebot-plugin-namelist)、[群管理](https://github.com/yzyyz1387/nonebot_plugin_admin)等插件，可以极大地丰富 Qbot 的功能。

非常值得一提的是，nb 提供了非常全面的[插件接口](https://nonebot.dev/docs/tutorial/create-plugin)，只需要对 Python 语法有简单的了解，我们就可以根据提供的接口自定义机器人的功能。例如在本次纳新中，AI 星宝就搭载了 issue 反馈、自检测维护的功能。

除了为 Qbot 安装插件外，想要让插件可以客制化的达到需求，我们还需要对插件进行一些设置，主要为为自动回复设置预设的自动回复，这里我们可以直接使用技术部~~代代相传（不是）~~的配置文件，他可以应付大部分场景。如果想要额外添加相应项，可以参考 AutoReply 插件的[配置文档](https://autoreply.lgc2333.top/#/configuring/)。

至此，Qbot 正式成为了 AI 星宝！

### 部署 Qbot

emmm，有没有注意到一件事情，我们之前配置好的机器人在哪里使用呢？

实际上，当前就相当于，我们写好了一个程序，但是没有让它上岗。所以，我们要让它行动起来！

1、设置常用参数：

关闭第一次打开的 Qbot，在 `AIxbot` 文件夹（即机器人为名的文件夹）下的`.env.prod`文件中，追加如下配置：

```prod
HOST=IP地址  # 配置 NoneBot 监听的 IP / 主机名
PORT=端口  # 配置 NoneBot 监听的端口
SUPERUSERS=["部署的QQ号", "操作者的QQ号","可扩展，其他管理员的QQ号"]  # 配置 NoneBot 超级用户
NICKNAME=["AIxbot"]  # 配置机器人的昵称
COMMAND_START=[""]  # 配置命令起始字符
COMMAND_SEP=["."]  # 配置命令分割字符
```

如果你正在 windows 环境中练习，你可以将`HOST`设为`1270.0.0.1`，`PORT`需要填写**与 go-cqhttp 及 qsign 相同**的端口号。

2、使用 qsign 部署签名环境：

这一步非常简单，根据 [qsign](https://github.com/fuqiuluo/unidbg-fetch-qsign) 的教程启动即可。

3、启用 go-cqhttp 连接 QQ：

同样的，根据 [go-cqhttp](https://github.com/Mrs4s/go-cqhttp) 的教程启动即可。

4、Nonebot，启动！：

关闭 qsign，在 Qbot 的文件夹下使用`nb run`指令重新启动 nb。

至此，AI 星宝已连接至你的 QQ！

### 测试

连接成功后，如何让 AI 星宝与你交互呢？

可以在 QQ 向 AI 星宝的 QQ 发送如下命令测试：

```QQ
echo Hello, world!
```

如果一切正常，AI 星宝会向你回复“`Hello, world!`”。

如果没有成功，请确保之前的各个步骤均分别成功，如果还是没发现问题所在请向部长或[我](?target=home&id=联系方式)请求帮助。~~（或者直接啃[官方文档](https://github.com/nonebot/nonebot2)）~~

至此，你的 AI 星宝已经可以投入使用了，记得给你的 AI 星宝提供相应的自动回复词库！

### 持久化运行

看到这里，相信你已经学会在 windows 上部署 AI 星宝了。如果你可以接受**一直开着电脑**，并同时运行 go-cqhttp 与 nb，那到这里实际上就可以投入使用了，这**并不影响你的电脑上其他功能的使用**。当然，被占用的端口除外。

如果你手上恰好有两台电脑，那完全可以使用**另一台电**脑充当服务器，你甚至有了一个图形化操作界面。

如果你想试试服务器，阿里云、腾讯云、华为云的学生试用将是不错的选择，三丰云也有永久免费的入门级服务器，这些都是用于练习不错的资源。当然，你需要自学服务器的相关操作，他们一般是 **Linux 系统**。

不论你在使用哪一种方法，我都推荐使用这样一款[简单的 Python 脚本](https://github.com/kqcoxn/Nonebot-CodaRestartHelper-plugin)辅助持久化运行，它并不是必要的，但对于 QQ 相对严格的安全检测与 shell 程序随时可能卡住的情况非常有帮助。

至此，AI 星宝已经可以持久化运行，你已经完全掌握了操作 AI 星宝！

## 开发指南

啊？这玩意真能用一个说明文档教会？有兴趣借助[官方文档](https://github.com/nonebot/nonebot2)慢慢摸索吧 qwq

~~至此，你已经完全掌握了（雾 qwq）~~

---

由[docsify](https://github.com/docsifyjs/docsify/)提供强有力的文档框架支持！<br>由[Nonebot2](https://github.com/nonebot/nonebot2)提供强有力的 QQbot 框架支持！
