## go chaincode 部署的常用命令
### fabric v0.6 pbft

* 启动网络

docker-compose -f 4-peers.yml up

* 查看容器

Docker ps

* 进入容器

docker exec -it pbft_vp0_1 bash

* 部署chaincode

* 官方example02

peer chaincode deploy -p github.com/hyperledger/fabric/examples/chaincode/go/chaincode_example02 -c '{"Function":"init", "Args": ["a","100", "b", "2000"]}'

* 自己的test1.go

peer chaincode deploy -p test1.go -c '{"Function":"init", "Args": []}'

* 查询

peer chaincode query -n "ee5b24a1f17c356dd5f6e37307922e39ddba12e5d2e203ed93401d7d05eb0dd194fb9070549c5dc31eb63f4e654dbd5a1d86cbb30c48e3ab1812590cd0f78539"   -c '{"Function": "query", "Args": ["a"]}' 


* 调用

peer chaincode invoke -n "ee5b24a1f17c356dd5f6e37307922e39ddba12e5d2e203ed93401d7d05eb0dd194fb9070549c5dc31eb63f4e654dbd5a1d86cbb30c48e3ab1812590cd0f78539" -c '{"Function": "invoke", "Args": ["a", "b", "10"]}'

* 打印日志

docker logs -f pbft_vp0_1



