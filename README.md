# Blockchain_fabric
Study hyperledger fabric
### Author：Dearkano 3/19/2018
## Hyperledger Fabric v0.6 安装与测试
## Prerequisites
### 操作系统
本文使用Ubuntu 16.04
### 安装python
`sudo apt-get install python`
### 安装golang
`sudo apt-get install golang`
### 安装nodejs
`sudo apt-get install nodejs`
### 安装curl
`sudo apt-get install curl`
### 安装git
`sudo apt-get install git`

最后记得update
`sudo apt-get update`
### 安装docker与docker-compose
由于国内网络原因使用镜像</br>
https://www.cnblogs.com/tianhei/p/7802064.html</br>
或</br>
https://www.cnblogs.com/studyzy/p/7492637.html</br>
## Fabric
### 拉取Fabric v0.6镜像

```sh
$ docker pull yeasy/hyperledger-fabric-base:0.6-dp \
  && docker pull yeasy/hyperledger-fabric-peer:0.6-dp \
  && docker pull yeasy/hyperledger-fabric-membersrvc:0.6-dp \
  && docker pull yeasy/blockchain-explorer:latest \
  && docker tag yeasy/hyperledger-fabric-peer:0.6-dp hyperledger/fabric-peer \
  && docker tag yeasy/hyperledger-fabric-base:0.6-dp hyperledger/fabric-baseimage \
  && docker tag yeasy/hyperledger-fabric-membersrvc:0.6-dp hyperledger/fabric-membersrvc
```
注意多拉几次，全部拉成功，可能网络原因会连接失败

使用默认镜像（官方dockhub）进行拉取时，往往会遇到速度极慢或handshake timeout的情况<br/>
解决方案:换用国内仓库daocloud.如果你未遇到拉取过慢的情况，请跳过下面的内容<br/>
``echo "DOCKER_OPTS=\"\$DOCKER_OPTS --registry-mirror=http://f2d6cb40.m.daocloud.io\"" | sudo tee -a /etc/default/docker
  ``<br/>
``service docker stop``<br/>
修改 /etc/defaut/docker 文件，增加参数实行强制普通模式避免新版docker使用第三方仓库报错：<br/> 
``DOCKER_OPTS="--insecure-registry http://dl.dockerpool.com:5000"``<br/>
再重启服务：<br/> 
``service docker restart``<br/>

### clone demo
`git clone https://github.com/yeasy/docker-compose-files`

### 启动peer
`cd docker-compose-files/hyperledger_fabric/v0.6.0/pbft/`</br>
`docker-compose -f 4-peers.yml up`</br>
可以看到终端中已经成功运行了peer网络

### 部署chaincode
在当前地址打开另一个终端</br>
`docker exec -it pbft_vp0_1 bash`</br>
可以看到命令行发生了变化，变成了</br>
`root@vp0:/go/src/github.com/hyperledger/fabric#`</br>
部署一个example</br>
`peer chaincode deploy -p github.com/hyperledger/fabric/examples/chaincode/go/chaincode_example02 -c '{"Function":"init", "Args": ["a","100", "b", "200"]}'`</br>
这个示例是初始化两个账户a和b，a有余额100元，b有余额200元</br>
我们可以看到部署成功并返回了ChainCode的ID</br>
我这里是</br>
ee5b24a1f17c356dd5f6e37307922e39ddba12e5d2e203ed93401d7d05eb0dd194fb9070549c5dc31eb63f4e654dbd5a1d86cbb30c48e3ab1812590cd0f78539
### 查询chaincode
下面我们把这个ID放入一个变量中：</br>
`CC_ID="ee5b24a1f17c356dd5f6e37307922e39ddba12e5d2e203ed93401d7d05eb0dd194fb9070549c5dc31eb63f4e654dbd5a1d86cbb30c48e3ab1812590cd0f78539"`</br>
下面我们来查询一下a账户的余额：</br>
`peer chaincode query -n ${CC_ID} -c '{"Function": "query", "Args": ["a"]}'`</br>
Query Result: 100 </br>
可以看到余额是100元</br>
### 调用chaincode
接下来，我们让a给b转账10元，运行命令：</br>
`peer chaincode invoke -n ${CC_ID} -c '{"Function": "invoke", "Args": ["a", "b", "10"]}'`
现在已经转账完毕，我们再来查询一下a账户的余额：</br>
`peer chaincode query -n ${CC_ID} -c '{"Function": "query", "Args": ["a"]}'`
Query Result: 90 </br>
可以看到少了十元</br>

### 使用REST在windows调用
首先安装chrome插件DHC</br>
提供一个链接</br>
https://pan.baidu.com/s/1mSdtfURbKauk2WYYPn_YmA</br>
解压以后把_metadata重命名metadata然后用chrome添加即可</br>
（下面是使用虚拟机的内容）</br>
在ifconfig里找到虚拟机的IP,我这里地址是192.168.198.128:7050/chaincode，然后在DHC里发送http请求即可完成调用</br>
一个示例</br>
body部分：</br>
```sh
{ 
  "jsonrpc": "2.0", 
  "method": "deploy", 
  "params": { 
    "type": 1, 
    "chaincodeID":{ 
        "path":"github.com/hyperledger/fabric/examples/chaincode/go/chaincode_example02" 
    }, 
    "ctorMsg": { 
        "function":"init", 
        "args":["a", "1000", "b", "2000"] 
    } 
  }, 
  "id": 1 
}
```
header部分:</br>
`Content-Type:text/javascript`</br>
成功会返回code200,建议使用vmware，vbox可能出现主机ping不到客户机的情况。<br/>
<br/>
（下面是使用两台主机的内容）<br/>
在ifconfig里找到对应IP，注意以太网和无线局域网的IP是不同的，注意根据自己当前所使用网络类型区别（一般很容易分辨）</br>
有必要强调的是，如果您使用浙大校园网(有线or无线)，请务必使用通过上述命令查得的私有IP（10开头），而非其他第三方应用检测的公网IP<br/>
由于暂未知（可能是交换机限制）的原因，公网IP不仅ping不通，以其为目标的post请求也会失败<br/>
强烈建议您在进行post尝试前先运行ping命令进行检测，以避免不必要的麻烦<br/>
之后的操作（包括端口号和下级目录名）同虚拟机配置<br/>
<br/>

