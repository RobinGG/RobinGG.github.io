---
layout: page
title: Cassandra 入门笔记
---


## 关于 Cassandra

C* 是一个开源的 NoSQL 数据库。C* 具有持续可用性， 线性扩展性，易于管理大量服务器和不会有单点故障等特性。

Cassandra的数据模型支持非常方便的列索引，高性能的类日志结构（log-structured）数据更新, 强大的非规格化（denormalization）和物化（materialized）视图和缓存功能。

Cassandra是当同时需要高扩展性、高可用性、高性能、跨多数据中心和在云端管理超大量结构化、半结构化和非架构化数据时的理想解决方案。

## CAP 定理

CAP定理, 又称布鲁斯定理, 它指出一个分布式计算机系统无法同时满足下面三点：

一致性（所有节点在同一时间返回相同的数据）
可用性 （保证对每个客户端请求无论成功与否都有响应）
分区容忍性（系统中任意信息的丢失或失败不会影响系统的继续运行）
根据该定理，任何一个分布式系统只能满足以上三点中的两点而不可能满足全部满足三点。

Cassandra一般被认为是满足AP的系统，也就是说Cassandra认为可用性和分区容忍性比一致性更重要。不过Cassandra可以通过调节replication factor和consistency level来满足C。

## Cassandra 数据结构

Relational Model | Cassandra Model
-----------------|----------------
Database | Keyspace
Table | Column Family(CF)
Private key | Row key
Column name | Column name/key
Column value | Column value

## CQL 

CQL 语句的书写与 SQL 类似。在 CQL 中关键字是不区分大小写的，除非使用双引号。

### CQL 数据类型

CQL 类型 | 对应 Java 类型 | 描述
---------|---------------|-----
ascii	 | String	| ascii字符串
bigint	 | long	| 64位整数
blob	 | ByteBuffer/byte[] | 二进制数组
boolean	 | boolean | 布尔
counter	 | long | 计数器，支持原子性的增减，不支持直接赋值
decimal	 | BigDecimal | 高精度小数
double	 | double | 64位浮点数
float	 | float | 32位浮点数
inet	 | InetAddress | ipv4或ipv6协议的ip地址
int	 | int | 32位整数
list	 | List | 有序的列表
map	 | Map | 键值对
set	 | Set | 集合
text	 | String | utf-8编码的字符串
timestamp	| Date | 日期
uuid	 | UUID | UUID类型
timeuuid	 | UUID | 时间相关的UUID
varchar	 | string | text的别名
varint	| BigInteger | 高精度整型
