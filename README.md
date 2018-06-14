# Zipkin 日志服务集成

## 内容

* [集成原理](#集成原理)
* [集成步骤](#集成步骤)
* [集成详细步骤](#集成详细步骤)
  * [日志服务](#日志服务)
  * [启动 Jaeger on Aliyun Log Service](#启动-Jaeger-on-Aliyun-Log-Service)
  * [采集](#采集)
  * [Jaeger UI](#jaeger-ui)
* [参考资料](#参考资料)

## 集成原理
Jaeger on Aliyun Log Service 是基于 Jeager 开发的分布式追踪系统，支持将采集到的追踪数据持久化到日志服务中，并通过 Jaeger 的原生接口进行查询和展示。
Jaeger on Aliyun Log Service 包含 Agent、Collector 和 Query 等组件。其中 Collector 和 Zipkin 做了兼容，它暴露的`9411`端口可以接收 Zipkin libraries 发送过来的数据。

## 集成步骤
* 开通日志服务。
* 启动 jaeger-collector、jaeger-query。
* 将采集到的数据配置发往 jaeger-collector。
* 使用 jaeger UI 对追踪数据进行查询分析。

## 集成详细步骤
### 日志服务
登录[日志服务控制台](https://sls.console.aliyun.com/#/)，创建用于存储追踪数据的 project、logstore。

### 启动 Jaeger on Aliyun Log Service
* 下载 docker-compose 模板 [aliyunlog-jaeger-docker-compose.yml](https://github.com/aliyun/aliyun-log-jaeger/blob/master/docker-compose/aliyunlog-jaeger-docker-compose.yml)。
* 运行如下命令启动 jaeger-collector，jaeger-query。
```
docker-compose -f aliyunlog-jaeger-docker-compose.yml up
```
**注意**：运行该命令之前请替换如下参数为真实值 ${PROJECT}、${ENDPOINT}、${ACCESS_KEY_ID}、${ACCESS_KEY_SECRET}、${SPAN_LOGSTORE}

### 采集
以 OpenZipkin Java libraries [brave](https://github.com/openzipkin/brave/tree/master/spring-beans) 为例，配置 endpoint 将数据发往 jaeger-collector。

### 访问 Jaeger UI
http://localhost:16686/search

# 参考资料
* [Jaeger on Aliyun Log Service](https://github.com/aliyun/aliyun-log-jaeger)
* [开放分布式追踪（OpenTracing）入门与 Jaeger 实现](https://yq.aliyun.com/articles/514488?spm=a2c4e.11153959.0.0.5a0447a2hNtH2h)
