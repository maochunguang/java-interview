# 注册中心框架
注册中心

## 各个注册中心对比

|                 | **Nacos**                   | **Eureka**  | **Consul**          | **Zookeeper** | etcd        |
| :-------------- | :-------------------------- | :---------- | :------------------ | :------------ | :---------- |
| 一致性协议      | CP+AP                       | AP          | CP                  | CP            | CP          |
| 健康检查        | TCP/HTTP/MYSQL/ Client Beat | Client Beat | TCP/HTTP/ gRPC/ Cmd | Keep Alive    | HTTP/Client |
| 负载均衡策略    | 权重/ metadata/ Selector    | Ribbon      | Fabio               | —             |             |
| 雪崩保护        | 有                          | 有          | 无                  | 无            | 无          |
| 自动注销实例    | 支持                        | 支持        | 支持                | 支持          |             |
| 访问协议        | HTTP/DNS                    | HTTP        | HTTP/DNS            | TCP           |             |
| 监听支持        | 支持                        | 支持        | 支持                | 支持          |             |
| 多数据中心      | 支持                        | 支持        | 支持                | 不支持        |             |
| 跨注册中心同步  | 支持                        | 不支持      | 支持                | 不支持        | 不支持      |
| SpringCloud集成 | 支持                        | 支持        | 支持                | 支持          | 不支持      |
| Dubbo集成       | 支持                        | 不支持      | 支持                | 支持          | 支持        |
| K8S集成         | 支持                        | 不支持      | 支持                | 不支持        | 支持        |


## nacos

## etcd

## consul