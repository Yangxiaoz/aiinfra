<!--Copyright © ZOMI 适用于[License](https://github.com/Infrasys-AI/AIInfra)版权许可-->

# 容器的诞生

Author by: 张柯帆

容器技术其实来源于 PaaS 的发展。PaaS 所提供的正是“托管”能力，但初期还比较原始。尽管当时虚拟化已经是比较常见的技术了，用户可以向 AWS 这样的云厂商购买虚拟机实例，但用户的使用习惯仍然是本地环境那一套——登录机器，然后执行脚本或者命令行部署应用。但是假如本地环境和虚拟机的网络配置、认证配置、文件系统等资源不一致，导致部署失败了怎么办呢？又或是同实例其他人的应用资源抢占、崩溃影响了我的应用怎么办呢？这就存在两个需求：

1. 如何保证本地环境和虚拟机的环境一致？如果总是不一致，能否更稳定地“打包”？
2. 应用隔离，互不影响

应用隔离好办，使用操作系统提供的 Cgroups、Namespace 机制创建一个隔离的“沙箱”即可。Docker 也是这么做的。另外，早期除了 Docker 其实还有像 Cloud Foundry 这样的项目，也是使用一样的技术原理，但它并不好用。主要原因是它在环境的一致性做得并不好。Cloud Foundry 只提供了可执行文件 + 启动脚本的打包组合。但这样的方式其实只是把以前本地做的工作在云上再做一遍罢了，并没有优化效率。

举个例子，相信几乎所有的技术人员都经历过搭建开发环境吧。假如你是一位 C++ 开发者，你需要准备的东西有：包管理器、编译器（正确的版本）、构建工具、依赖库、环境变量、IDE 设置、正常的网络等等。任何一环出错了都没法进行工作。如果只需要设置一次那还能接受，但是线上系统的变更可是非常频繁的，如果每次部署都从头搞一遍那也太令人崩溃了！你可能想：我写个脚本自动执行所有命令不就得了。但也可能出现脚本在本地运行正常，在服务器上却执行失败的情况。同时由于生产环境实际非常复杂，不同框架、不同虚拟机、不同网络、甚至不同应用版本都可能需要使用不同的脚本，导致脚本管理也很麻烦。归根究底，操作系统、基础设施的方方面面都可能影响应用部署。从这个角度看，可执行文件加脚本的表达能力和约束能力都实在太弱了。

而 Docker 流行起来的原因恰恰是对“打包”的优化，这个功能就叫 Docker 镜像。它会将一个操作系统所需要的所有文件都打包成一个压缩包，包括所有依赖。再把可执行文件放进去，这样无论在哪里解压缩，应用的运行环境都跟本地是一摸一样的。而制作镜像也非常简单，只需要一行命令：

```bash
$ docker build .
```

接下来只要创建一个上面提到的“沙箱”，在里面解压这个压缩包并运行，就能保证环境的一致性和应用的隔离性了，完全不需要修改任何配置！开发人员可以专心在代码开发和镜像制作上了。运行镜像也非常简单：

```bash
docker run "我的镜像"
```

这种便利的机制极大地释放了生产力，开发者纷纷选择了用脚投票，抛弃 PaaS，拥抱 Docker。

不过值得一提的是，Docker 虽然解决了打包的困难，但是却没法很好地完成复杂应用部署，也就是“容器编排”。而在云原生技术中，真正的商业价值其实并不是容器，而是容器编排。也因此，Docker 公司与 CNCF 社区爆发了一场争夺生态霸主的战争，最终以 Kubernetes 的胜利告终。先按下不表，留待后续介绍。
