# hipnames
High PrEcision Gnss Analysis And ManagEment Software
高精度单机解算服务

::: danger 注意
单机版服务已经集成了北斗数据流服务，里面自带集成了Ntrip Caster的功能。你可以通过添加设备、配置解算和结果推送等一系列功能操作来实现完整的GNSS解算。
:::

## 镜像安装

Hipnames为单机版服务，使用 [docker-compose](https://docs.docker.com/compose/install) 脚本进行镜像安装

x86_64 架构使用镜像registry.cn-hangzhou.aliyuncs.com/navmg/hipnames:xxxx

aarch_64 架构使用镜像registry.cn-hangzhou.aliyuncs.com/navmg/hipnames-aarch64:xxxx

当前案例中版本的服务镜像为：`1.0.0`

docker-compose-hipnames.yml脚本配置

```yaml
version: "3"

services:
  hipnames:
    image: registry.cn-hangzhou.aliyuncs.com/navmg/hipnames:1.0.0
    container_name: hipnames
    hostname: hipnames
    restart: always
    volumes:
      - ${HIPNAMES_BACKEND_LOGBACK}:/app/logback
      - ${HIPNAMES_BACKEND_DATABASE_PATH}:/app/hipnames/hipnames-database.db
      - ${HIPNAMES_BACKEND_PATH}:/app/hipnames
    ports:
      - ${HIPNAMES_BACKEND_PORT}:18007
      - ${HIPNAMES_BACKEND_CASTER_SERVER_PORT}:9090
      - ${HIPNAMES_BACKEND_CASTER_CLIENT_PORT}:9095
      - ${HIPNAMES_FRONTEND_PORT}:80
    networks:
      - iot-net

networks:
  iot-net:
    driver: bridge
```



### 环境变量解释说明

::: tip 说明
`单机版服务`默认的服务端口为18007，默认的`HIPNAMES_BACKEND_CASTER_SERVER_PORT` ntrip caster server默认端口为9090，默认的`HIPNAMES_BACKEND_CASTER_CLIENT_PORT` ntrip caster client默认端口为9095
:::

| **变量**  | **说明**            |
|---------|-------------------|
| HIPNAMES_BACKEND_LOGBACK    | 服务日志文件根路径         |
| HIPNAMES_BACKEND_DATABASE_PATH | 后台数据库存储绝对路径         |
| HIPNAMES_BACKEND_PATH | 文件存储跟路径           |
| HIPNAMES_BACKEND_PORT | 后台服务映射端口         |
| HIPNAMES_BACKEND_CASTER_SERVER_PORT | ntrip caster server映射端口         |
| HIPNAMES_BACKEND_CASTER_CLIENT_PORT | ntrip caster client映射端口         |
| HIPNAMES_FRONTEND_PORT | 前端网站访问端口         |


::: warning 特别说明
单机版服务中的文件存储格式按照小时段进行存储文件的，文件时间按照GPS时分类文件夹的。
:::

