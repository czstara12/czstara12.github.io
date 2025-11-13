---
title: 安装使用cassandra
date: 2024-04-01 16:09:44
categories:
- cassandra
tags:
- linux
---

安装使用cassandra

<!-- more -->

# 安装步骤

假设你已经安装好了wsl和ubuntu子系统

打开windows终端 运行`wsl`

```sh
PS C:\Users\czsta> wsl
```

这样就进入了ubuntu环境

## 开始安装

```bash
cd ~
sudo apt update
sudo apt install -y default-jdk
wget https://dlcdn.apache.org/cassandra/4.1.4/apache-cassandra-4.1.4-bin.tar.gz
tar -xf apache-cassandra-4.1.4-bin.tar.gz
```
安装完毕

## 启动

如果重新启动终端需要运行

```bash
wsl
cd ~
```

安装好后可以直接接着运行此处

```bash
cd apache-cassandra-4.1.4/bin
./cassandra
./cqlsh
```

现在你已经进入了cqlsh环境

```
cqlsh>
```

## 操作

以天文数据库为例

```cql
-- 创建键空间
CREATE KEYSPACE IF NOT EXISTS astronomy WITH replication = {'class':'SimpleStrategy', 'replication_factor' : 3};

-- 使用键空间
USE astronomy;

-- 创建行星列族
CREATE TABLE IF NOT EXISTS planets (
    planet_id uuid PRIMARY KEY,
    name text,
    distance_from_sun bigint, -- 以千米为单位
    diameter bigint -- 以千米为单位
);

-- 创建恒星列族
CREATE TABLE IF NOT EXISTS stars (
    star_id uuid PRIMARY KEY,
    name text,
    distance_from_earth bigint, -- 以光年为单位
    type text
);

-- 创建恒星列族
CREATE TABLE stats (
    star_id uuid PRIMARY KEY,
    name text,
    distance_from_earth bigint, -- 以光年为单位
    type text
);



-- 向 `planets` 表格插入数据
INSERT INTO planets (planet_id, name, distance_from_earth, diameter) VALUES (uuid(), 'Earth', 149600000, 12742);
INSERT INTO planets (planet_id, name, distance_from_earth, diameter) VALUES (uuid(), 'Mars', 227940000, 6779);

-- 向 `stars` 表格插入数据
INSERT INTO stars (star_id, name, distance_from_earth, type) VALUES (uuid(), 'Sun', 10, 'G2V');
INSERT INTO stars (star_id, name, distance_from_earth, type) VALUES (uuid(), 'Proxima Centauri', 424, 'M5.5Ve');



-- 添加行星数据
INSERT INTO planets (planet_id, name, distance_from_sun, diameter) VALUES (uuid(), 'Earth', 149600000, 12742);

-- 修改行星数据
UPDATE planets SET distance_from_sun = 149597870 WHERE name = 'Earth';

-- 删除行星数据
DELETE FROM planets WHERE name = 'Mars'; -- 假设我们先前添加了火星

-- 同样地，对恒星数据进行操作
INSERT INTO stars (star_id, name, distance_from_earth, type) VALUES (uuid(), 'Sun', 0, 'G2V');




-- 展示所有行星
SELECT * FROM planets;

-- 展示所有恒星
SELECT * FROM stars;


-- 为恒星类型创建索引
CREATE INDEX ON stars (type);


-- 查询特定类型的所有恒星
SELECT * FROM stars WHERE type = 'G2V' ALLOW FILTERING;


-- 查询特定类型的所有恒星
SELECT * FROM stars WHERE type = 'G2V' ALLOW FILTERING;


-- 选择特定列
SELECT name, distance_from_sun FROM planets WHERE name = 'Earth';

-- 投影操作实际上是选择特定的列，所以上面的查询同时展示了选择和投影


-- 使用标量函数 now
SELECT name, distance_from_sun * 0.621 FROM planets;

-- 聚合函数（假设有多个行星数据）min max avg sum count
SELECT COUNT(*) FROM planets;


-- 添加一条带TTL的记录
INSERT INTO planets (planet_id, name, distance_from_sun, diameter) VALUES (uuid(), 'Temporary Planet', 100000000, 12345) USING TTL 30;

-- 查看记录（需在30秒内）
SELECT * FROM planets WHERE name = 'Temporary Planet';

-- 清除表格
TRUNCATE planets;

-- 删除表格 索引
DROP TABLE planets;
DROP INDEX stars_type_idx;

-- 批量执行
BEGIN BATCH
   INSERT INTO planets (planet_id, name, distance_from_sun, diameter) VALUES (uuid(), 'Mars', 227940000, 6779);
   INSERT INTO stars (star_id, name, distance_from_earth, type) VALUES (uuid(), 'Alpha Centauri', 437, 'G2V');
APPLY BATCH;


---------------------------------------------------------------------------------------------------------------------------------------------

-- 物化视图
CREATE MATERIALIZED VIEW stars_by_type AS
SELECT *
FROM stars
WHERE type IS NOT NULL AND star_id IS NOT NULL
PRIMARY KEY (type, star_id);

-- 更新数据（如果存在）
UPDATE planets SET diameter = 12345 WHERE name = 'Earth' IF EXISTS;

-- 删除数据（如果满足条件）
DELETE FROM planets WHERE name = 'Mars' IF diameter = 6779;

```
