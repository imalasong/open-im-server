## 源码运行指南


#### 部署中间件
  1、修改 docker-compose.yml 文件中的内容,把172.16.1.57替换成你的内网ip
  <br/>2、修改./scripts/mongo-init.sh文件
```
use admin
db.auth('root', 'openIM123')

db = db.getSiblingDB('openim_v3')
db.createUser({
user: "root",
pwd: "openIM123",
roles: [
// Assign appropriate roles here
{ role: 'readWrite', db: 'openim_v3' }
]
});
 <br/> 3、修改 config/config.yaml 文件,把172.16.1.57替换成你的内网ip
```
  2、在项目根路径执行以下命令：

```
docker compose up -d

db.createUser({ user:'root',pwd:'openIM123',roles:[ { role:'userAdminAnyDatabase', db: 'openim_v3'},"readWriteAnyDatabase"]});
```

#### 部署应用程序
  1、修改 config/config.yaml 文件,把172.16.1.57替换成你的内网ip
  2、运行cmd目录下的所有程序，除了openim-cmdutils/main.go

```
go run ./cmd/openim-api/main.go --config_folder_path=./config

go run ./cmd/openim-crontask/main.go --config_folder_path=./config

go run ./cmd/openim-msggateway/main.go --config_folder_path=./config

go run ./cmd/openim-msgtransfer/main.go --config_folder_path=./config

go run ./cmd/openim-push/main.go --config_folder_path=./config

go run ./cmd/openim-rpc/openim-rpc-auth/main.go --config_folder_path=./config

go run ./cmd/openim-rpc/openim-rpc-conversation/main.go --config_folder_path=./config

go run ./cmd/openim-rpc/openim-rpc-friend/main.go --config_folder_path=./config

go run ./cmd/openim-rpc/openim-rpc-group/main.go --config_folder_path=./config

go run ./cmd/openim-rpc/openim-rpc-msg/main.go --config_folder_path=./config

go run ./cmd/openim-rpc/openim-rpc-third/main.go --config_folder_path=./config

go run ./cmd/openim-rpc/openim-rpc-user/main.go --config_folder_path=./config
```



#### 可选配置

 1、运行Kafka可视化监控工具

```
  docker run --detach -p 8081:8080 --name kafka-ui --restart always --env KAFKA_CLUSTERS_0_NAME=local --env DYNAMIC_CONFIG_ENABLED=true --env AUTH_TYPE=LOGIN_FORM --env SPRING_SECURITY_USER_NAME=admin --env SPRING_SECURITY_USER_PASSWORD=admin provectuslabs/kafka-ui:master
 
 账户密码：admin/admin
```

