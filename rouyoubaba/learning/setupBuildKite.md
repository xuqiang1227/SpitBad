# BuildKite配置流程
今天接触了一下buildKite的工作，第一次接触这些CI的部署环节。
具体的buildKite配置代码和docker相关的内容是其他同事已经处理好的，我只需要用他的模板在buildkite和git上进行一些配置，让两个网站通过hook，让我的项目达到CI的目的。

## 一共操作包含以下几个步骤
1. 在buildkite.com 创建一个 New Pipeline
2. 在git上面创建对应的repo
3. 通过代码以及页面配置将二者绑定

### 首先进行步骤1
单机页面右上角的 `+` ，进入到创建页面，分别填入 
* `Name` pipeline 名称 
* `Git Repository` 第二步中repo创建的git地址，采用SSH 
* 在Steps中设置 `Label` 步骤的名称，可以叫做upload)
* `Commands to run`  git地址对应的buildkite执行文件的具体地址，比如可以是.buildkite/upload-pipeline，upload-pipeline就是具体的shell脚本
* 右边的配置参数，我所部署的的是填写 `Agent Targeting Rules` 通过一个AWS的主机去跑这个build
* 点击 Create Pipeline之后，在 `GitHub Enterprise Settings` 中要勾选 `Build pull requests from third-party forked repositories` ，fork的repo也会跑这个build 

> 详见图  
![](setupBuildKite/96A3074A-750D-4062-BA2B-00286948331A.png)

### 步骤2
* 通过buildkite中的 `GitHub Enterprise Setup Instructions` 链接后，获得webhook地址
* 在git的setting中设置点击 Add webhook 并给予下列权限

![](setupBuildKite/DAE90A97-271F-4832-A9E5-0F7B7CBEDF48.png)
* [Generating a new SSH key and adding it to the ssh-agent](https://help.github.com/enterprise/2.7/user/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/)  通过  `ssh-keygen -t rsa -b 4096 -C “随意输入” -f “abc”` ，将abc.pub中的内容拷贝 `pbcopy < abc.pub` 
* 并在Deploy keys中， `Add deploy key` ，将上一步拷贝的内容粘入

### 步骤3
* 参考 [SHUSH操作](https://github.com/realestate-com-au/shush) ，进行加密处理(可能先要进行 `saml-aws authenticate` 进行权限验证，如果需要选的话，选择muppets，用命令 `aws kms list-aliases` ，选择最上面的 `TargetKeyId` )
* 在code-base中的 .buildkite 执行的shell脚本中，修改 ` KMS_ENCRYPTED_SSH_PRIVATE_KEY` 为通过shush后获取的key即可