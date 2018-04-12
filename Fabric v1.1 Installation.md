# Blockchain_fabric
Study hyperledger fabric
### Author：VayneTian 4/12/2018
## Hyperledger Fabric v1.1 安装与测试
## Prerequisites
### 操作系统
本文使用Ubuntu 16.04
### 安装python2.7
`sudo apt-get install python`
### 安装golang1.9
`sudo apt-get install golang`
### 安装nodejs
`sudo apt-get install nodejs`
### 安装npm
`sudo apt-get install npm`

`npm install npm@5.6.0 -g`
### 安装curl
`sudo apt-get install curl`
### 安装git
`sudo apt-get install git`

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