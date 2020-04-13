# github action使用教程

## 背景

为了改进jenkins需要搭建独立服务器，还需要专门的CI服务器进行打包的痛点。使用github action可以不用搭建专门的jenkins服务器，以及相应的CI服务器。github通过yml格式的文档就可以完成Jenkins server+CI server的所有功能。来达到减少维护量和服务器成本的目的。

## 功能说明

.github/workflows/ 目录下按照模块进行了区分，方便各个模块修改自己的CI，不会影响其他模块。

* client.yml
* management.yml
* product-service.yml
* statistics-service.yml

每个模块的yml文件中基本分为如下几步：

* 查看当前环境变量，用于调试和排错。
* 通过各自模块的dockerfile打包镜像
* 登陆自己项目的github docker仓库
* 推送打包好的docker image到自己项目的仓库中
* 将自己仓库中的docker image部署到自己的k8s集群的test环境中


## 使用方法

1. 在自己的项目中找到Settings=>Sectets=>Add a new secret=>Name:packages_token=>Value:填入你带有packages授权的token
2. 同理将k8s config加入KUBE_CONFIG变量(步骤同packages_token)

具体token权限详见参考文档

在每次提交的时候，github就会启动相应的yml对应的流程进行构建。可以点击项目的action进行过程查看。

## TAG

注意由于项目中使用了Jenkins做编译打包，为了避免Github Action与Jenkins有冲突，所以我们在Github Action流水线打标签时添加了CI关键词，Github Action当前实现了部署到测试环境中。不依赖jenkins。

## 参考文档
http://www.ruanyifeng.com/blog/2019/09/getting-started-with-github-actions.html
https://help.github.com/cn/packages/publishing-and-managing-packages/about-github-packages#about-tokens

