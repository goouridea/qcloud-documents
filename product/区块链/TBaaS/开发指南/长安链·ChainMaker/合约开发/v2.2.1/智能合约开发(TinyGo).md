## 使用 Go（TinyGo）进行智能合约开发

本章节主要描述使用 Go 进行 ChainMaker 合约编写的方法，主要面向于使用 Go 进行 ChainMaker 的合约开发的开发者。为了最小化 wasm 文件尺寸，应使用 TinyGO 编译器。

<dx-alert infotype="notice" title="">
- 当前在 v2.2.1 版本的长安链网络中，仅支持通过 TBaaS 控制台对存量 TinyGo 合约进行升级，不再支持 TinyGo 合约创建。
- 新用户请使用 Go（DockerGo）进行智能合约开发， Go（TinyGo）仅供老用户进行智能合约升级。
</dx-alert>

### 使用 Docker 镜像进行合约开发

ChainMaker 官方已经将容器发布至 [docker hub](https://hub.docker.com/u/chainmakerofficial)。

1. 拉取镜像
```
docker pull chainmakerofficial/chainmaker-go-contract:2.1.0
```
请指定您本机的工作目录 $WORK_DIR，例如 /data/workspace/contract，挂载到 docker 容器中以方便后续进行必要的一些文件拷贝。
```
docker run -it --name chainmaker-go-contract -v $WORK_DIR:/home chainmakerofficial/chainmaker-go-contract:2.1.0 bash
# 或者先后台启动
docker run -d  --name chainmaker-go-contract -v $WORK_DIR:/home chainmakerofficial/chainmaker-go-contract:2.1.0 bash -c "while true; do echo hello world; sleep 5;done"
# 再进入容器
docker exec -it chainmaker-go-contract bash
```

2. 编译合约
```
cd /home/
# 解压缩合约SDK源码
tar xvf /data/contract_go_template.tar.gz
cd contract_tinygo
# 编译main.go合约
sh build.sh
```
生成合约的字节码文件位置如下：
```
/home/contract_tinygo/main.wasm
```
`main.wasm` 文件可在 [TBaaS 控制台](https://console.cloud.tencent.com/tbaas/overview) 上传并部署。

3. 合约开发框架描述
   解压缩 contract_go_template.tar.gz 后，文件描述如下：
```
/home/contract_tinygo# ls -l
total 64
-rw-rw-r-- 1 1000 1000    56 Jul  2 12:45 build.sh            	# 编译脚本
-rw-rw-r-- 1 1000 1000  4149 Jul  2 12:44 bulletproofs.go		# 合约SDK基于bulletproofs的范围证明接口实现
-rw-rw-r-- 1 1000 1000 18871 Jul  2 12:44 chainmaker.go			# 合约SDK主要接口及实现
-rw-rw-r-- 1 1000 1000  4221 Jul  2 12:44 chainmaker_rs.go		# 合约SDK sql接口实现
-rw-rw-r-- 1 1000 1000 11777 May 24 13:27 easycodec.go			# 序列化工具类
-rw-rw-r-- 1 1000 1000  3585 Jul  2 12:44 main.go			   	# 存证示例代码
-rwxr-xr-x 1 root root 65122 Jul  6 07:22 main.wasm				# 编译成功后的wasm文件
-rw-rw-r-- 1 1000 1000  1992 Jul  2 12:44 paillier.go 			# 合约SDK基于paillier的半同态运算接口实现
```

用户使用 Go（TinyGo） 编写智能合约后，可以把源代码更新到 `main.go` 文件中并重新编译，可得到新的智能合约的字节码，并前往 [TBaaS 控制台](https://console.cloud.tencent.com/tbaas/overview) 上传并部署。更多关于使用 Go（TinyGo） 开发长安链智能合约的详情，可参考长安链官网 [使用 Go（TinyGo）进行智能合约开发](https://docs.chainmaker.org.cn/v2.2.1/html/operation/%E6%99%BA%E8%83%BD%E5%90%88%E7%BA%A6.html#go-tinygo)。
