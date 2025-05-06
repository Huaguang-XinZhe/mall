# Mall 项目 Railway 部署指南

本文档介绍如何在 [Railway](https://railway.app/) 平台上部署 mall 电商项目。

## 前提条件

1. 注册 Railway 账号
2. 安装 Railway CLI (可选)：`npm i -g @railway/cli`
3. 登录 Railway：`railway login`

## 步骤 1：创建 Railway 项目

```
railway init
```

## 步骤 2：创建必要的服务

在 Railway 平台上创建以下服务：

### 2.1 MySQL 服务

1. 登录 Railway Dashboard
2. 点击 "New Project" -> "Provision MySQL"
3. 记录生成的数据库连接信息

初始化数据库：

1. 进入 MySQL 服务的管理界面
2. 选择 "Data" 标签页
3. 上传并执行 `document/sql/mall.sql` 脚本

### 2.2 Redis 服务

1. 在项目中，点击 "New Service" -> "Provision Redis"
2. 记录连接信息

### 2.3 MongoDB 服务 (如果使用 mall-portal 模块)

1. 在项目中，点击 "New Service" -> "Provision MongoDB"
2. 记录连接信息

### 2.4 (可选) RabbitMQ 服务

如果需要 RabbitMQ 功能，可以：

1. 在项目中，点击 "New Service" -> "Provision RabbitMQ"
2. 记录连接信息

## 步骤 3：部署 mall 应用

有两种方式可以部署应用：

### 方法 1：直接在 Railway 上部署 GitHub 仓库

1. 在项目中，点击 "New Service" -> "GitHub Repo"
2. 搜索并选择你的 mall 项目分支
3. 配置环境变量 (参见下方环境变量说明)

### 方法 2：使用 Railway CLI 部署

1. 将项目克隆到本地
2. 在项目根目录执行：`railway up`

## 环境变量配置

在 Railway 部署面板中，需要配置以下环境变量：

### 基本配置

```
SPRING_PROFILES_ACTIVE=railway
PORT=8080
```

### MySQL 配置

```
MYSQL_URL=jdbc:mysql://<RAILWAY_MYSQL_HOST>:<RAILWAY_MYSQL_PORT>/<RAILWAY_MYSQL_DATABASE>?useUnicode=true&characterEncoding=utf-8&serverTimezone=Asia/Shanghai&useSSL=false
MYSQL_USERNAME=<RAILWAY_MYSQL_USERNAME>
MYSQL_PASSWORD=<RAILWAY_MYSQL_PASSWORD>
```

### Redis 配置

```
REDIS_HOST=<RAILWAY_REDIS_HOST>
REDIS_PORT=<RAILWAY_REDIS_PORT>
REDIS_PASSWORD=<RAILWAY_REDIS_PASSWORD>
```

### MongoDB 配置 (如果使用 mall-portal)

```
MONGODB_URI=mongodb://<RAILWAY_MONGODB_USERNAME>:<RAILWAY_MONGODB_PASSWORD>@<RAILWAY_MONGODB_HOST>:<RAILWAY_MONGODB_PORT>/<RAILWAY_MONGODB_DATABASE>
```

### RabbitMQ 配置 (如果使用)

```
RABBITMQ_HOST=<RAILWAY_RABBITMQ_HOST>
RABBITMQ_PORT=<RAILWAY_RABBITMQ_PORT>
RABBITMQ_VHOST=/mall
RABBITMQ_USERNAME=<RAILWAY_RABBITMQ_USERNAME>
RABBITMQ_PASSWORD=<RAILWAY_RABBITMQ_PASSWORD>
```

### MinIO 配置 (可选)

```
MINIO_ENDPOINT=<YOUR_MINIO_ENDPOINT>
MINIO_BUCKET=mall
MINIO_ACCESS_KEY=<YOUR_MINIO_ACCESS_KEY>
MINIO_SECRET_KEY=<YOUR_MINIO_SECRET_KEY>
```

## 部署后验证

1. 访问 Railway 为你的服务分配的域名
2. 尝试访问 Swagger 接口文档地址：`https://<YOUR_RAILWAY_DOMAIN>/swagger-ui/`

## 部署注意事项

1. Railway 免费计划每月有使用限制，注意监控使用情况
2. 首次部署可能需要等待较长时间，因为需要编译整个项目
3. 可以根据需求选择只部署某些模块，而不是整个项目
4. 如需部署 mall-admin-web 前端项目，可以使用 Railway 的静态网站托管服务
