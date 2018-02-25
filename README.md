# 根据TW的作业描述，我制作了一个Demo来演示如何完成一个 基于容器化平台+Jenkins的持续集成

> 目标：基于kubernetes搭建一个高可用的CI+CD环境，运用到了Kubernetes，Docker，Jenkins

## 开始体验

> 假设您现在是一个开发人员，您的代码在git上管理，地址为[https://github.com/autherlj/DevOps_HomeWork_Demo]
> 现在这个代码已经上线您可以通过 [http://phpmysql.dcos.nlabs.sg/] 访问，设想一下这个就是我们的WEB产品。

## 下载代码

> 假设您已经掌握git，那么请将代码下载到您的本地
`git clone https://github.com/autherlj/DevOps_HomeWork_Demo`

## 修复问题

> 如果您能访问[http://phpmysql.dcos.nlabs.sg/]  您会发现页面会有"123123123123123123123123123123123123"字符，我们认为这个是产品的瑕疵，我们需要对它进行线上紧急修复。
> 您通过vi index.php 文件发现了错误出现在51行,您删除了"123123123123123123123123123123123123"字符。并且通过下面的命令提交到了github上

```
git status       #查看当前文件index.php被修改
git add .        #添加到本地索引
git commit -m "fix bug"   #提交到本地仓库
git push         #提交到远程仓库这里push会输入我的git的账号和密码我会邮件发送给您
```
# 观察效果

>现在您什么都不用做，等待2~3min如果不出意外，你的改动就已经产生影响。


# 总结

> 如果您能顺利完成这个实验，那么说明我背后的Kubernetes，Docker，Jenkins在为你服务。其实Kubernetes平台远远不止这个特性（如果您了解的话）。我会把平台搭建的ansible脚本和这个Demo的jenkins脚本发送到您的提交地址。

