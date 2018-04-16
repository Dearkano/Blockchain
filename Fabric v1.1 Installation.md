

# Blockchain_fabric
Study hyperledger fabric
### Author：VayneTian 4/12/2018
## Hyperledger Fabric v1.1 安装与测试
## Prerequisites
### 操作系统
本文使用Ubuntu 16.04

* `apt install git`
* `curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash -`
* `apt install -y nodejs`
* `apt install docker`
* `apt install docker-compose`
* `git clone https://github.com/hyperledger/fabric.git`
* `git clone -b master https://github.com/hyperledger/fabric-samples.git`
* `cd fabric/scripts`
* `./bootstrap.sh`
*  https://nexus.hyperledger.org/content/repositories/releases/org/hyperledger/fabric/hyperledger-fabric/
    * 下载对应系统的v1.0.0二进制 并解压到fabric-samples下面
* `cd fabric-samples/first-netword`
* `./byfn.sh -m generate`
* `./byfn.sh -m up`


## 下载测试代码
`git clone -b master https://github.com/hyperledger/fabric-samples.git`

## 下载fabric源码
这段代码会下载fabric的文件并且使用脚本去拉取dockerhub上的镜像并全部标记latest

* 如果你可以翻墙 一步ok

`curl -sSL https://goo.gl/6wtTN5 | bash -s 1.1.0`

* 不能的话

`git clone https://github.com/hyperledger/fabric.git`

打开script文件夹命令行

`sh bootstrap.sh`

## 下载fabric二进制文件

https://nexus.hyperledger.org/content/repositories/releases/org/hyperledger/fabric/hyperledger-fabric/

在源码下新建aberic文件夹 解压进去
这里注意 对应自己的系统去下载

然后将bin文件夹放在fabirc-samples下面

## 一些坑 

* 第一次up遇到这个bug

After 5 attempts, PEER1 has failed to Join the Channel

是环境遗留问题 去./byfn.sh down mychannel一下再开


* 如果提示port被占用，停止容器并删除

docker stop $(docker ps -a -q)

docker rm $(docker ps -a -q)




*  如果是在阿里云机器上部署fabric ，在e2e_cli 启动网络时，遇到以下错误

 FAILED to execute End-2-End Scenario

 用户则需要修改 /etc/resolv.conf 配置，将 options timeout:2 attempts:3 rotate single-request-reopen 这一行内容注释掉

 https://www.cnblogs.com/chenfool/p/8353425.html

 这篇博客的debug内容还不错 其他一概推荐官方教程

 http://hyperledger-fabric.readthedocs.io/en/release-1.1/build_network.html



 ## 环境变量


export CHANNEL_NAME=mychannel


 CORE_PEER_MSPCONFIGPATH=./crypto-config/peerOrganizations/org1.example.com/users/Admin@org1.example.com/msp

CORE_PEER_ADDRESS=peer0.org1.example.com:7051

CORE_PEER_LOCALMSPID="Org1MSP"

CORE_PEER_TLS_ROOTCERT_FILE=./crypto-config/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt

peer channel create -o orderer.example.com:7050 -c $CHANNEL_NAME -f ./channel-artifacts/channel.tx --tls --cafile ./crypto-config/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem

