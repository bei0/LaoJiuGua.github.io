---
author: XiaoBei
categories:
- - Mysql
date: '2022-12-08 11:05:07'
excerpt: 场景：产品列表显示产品的多个标签 mysql 查询。 以往的解决办法都是查出先查出产品列表，在循环查询标签，多个标签用","隔开。 这样查询列表查一次数据库，循环列表又要查询数据库多次。性能太差
  表结构：  产品表： mysql   CREATE TABLE `kx_product` (     `id` int(11) NOT NULL AUTO_INCREMENT,     `title`
  v...
tags:
- Mysql
title: Mysql多条数据合并成一个字段显示
updated: '2022-12-08 11:29:36'
---
场景：产品列表显示产品的多个标签 mysql 查询。

以往的解决办法都是查出先查出产品列表，在循环查询标签，多个标签用","隔开。

这样查询列表查一次数据库，循环列表又要查询数据库多次。性能太差

### 表结构：

- 产品表：

  ```mysql
  CREATE TABLE `kx_product` (
    `id` int(11) NOT NULL AUTO_INCREMENT,
    `title` varchar(200) NOT NULL DEFAULT '标题',
    `preview` varchar(200) NOT NULL DEFAULT '' COMMENT '预览图片',
    `content` varchar(2000) NOT NULL DEFAULT '' COMMENT '内容',
    `number` varchar(20) NOT NULL DEFAULT '' COMMENT '货号',
    `stock` int(10) NOT NULL DEFAULT '0' COMMENT '库存',
    `add_time` int(11) NOT NULL DEFAULT '0' COMMENT '添加时间',
    `update_time` int(11) NOT NULL DEFAULT '0' COMMENT '修改时间',
    `memo` varchar(255) NOT NULL DEFAULT '' COMMENT '备注',
    PRIMARY KEY (`id`)
  ) ENGINE=InnoDB AUTO_INCREMENT=6 DEFAULT CHARSET=utf8 COMMENT='系统：产品表';
  ```
- 标签表:

  ```mysql
  CREATE TABLE `kx_product_label` (
    `id` int(11) NOT NULL AUTO_INCREMENT,
    `title` varchar(100) NOT NULL DEFAULT '' COMMENT '标题',
    `deleted` tinyint(1) NOT NULL DEFAULT '0' COMMENT '1:删除;0:正常',
    `status` tinyint(1) NOT NULL DEFAULT '1' COMMENT '-1:删除;-1:隐藏;0:禁用; 1:启用；2：锁定',
    `add_time` int(11) NOT NULL DEFAULT '0' COMMENT '添加时间',
    `update_time` int(11) NOT NULL DEFAULT '0' COMMENT '修改时间',
    `memo` varchar(255) NOT NULL DEFAULT '' COMMENT '备注',
    PRIMARY KEY (`id`)
  ) ENGINE=InnoDB AUTO_INCREMENT=3 DEFAULT CHARSET=utf8 COMMENT='系统：产品标签';
  ```
- 产品标签关系表：

  ```mysql
  CREATE TABLE `kx_product_item_label` (
    `id` int(11) NOT NULL AUTO_INCREMENT,
    `id_product` int(11) NOT NULL DEFAULT '0' COMMENT 'from:kx_product->id',
    `id_label` int(11) NOT NULL DEFAULT '0' COMMENT 'from:kx_product_label->id',
    `deleted` tinyint(1) NOT NULL DEFAULT '0' COMMENT '1:删除;0:正常',
    `status` tinyint(1) NOT NULL DEFAULT '1' COMMENT '-1:删除;-1:隐藏;0:禁用; 1:启用；2：锁定',
    `add_time` int(11) NOT NULL DEFAULT '0' COMMENT '添加时间',
    `update_time` int(11) NOT NULL DEFAULT '0' COMMENT '修改时间',
    `memo` varchar(255) NOT NULL DEFAULT '' COMMENT '备注',
    PRIMARY KEY (`id`)
  ) ENGINE=InnoDB AUTO_INCREMENT=25 DEFAULT CHARSET=utf8 COMMENT='系统：产品标签集合 一个产品有很多个标签';
  ```

### 解决办法

使用 **关键词 GROUP_CONCAT()：（返回一个字符串结果，该结果由分组中的值连接组合而成。）**

#### 链接查询

```mysql
SELECT
    a.id,
    a.content,
    a.title,
    a.add_time,
    a. CODE,
    a.number,
  l.title
FROM
    `kx_product` `a`
LEFT JOIN `kx_product_item_label` `ls` ON `ls`.`id_product` = `a`.`id`
LEFT JOIN `kx_product_label` `l` ON `l`.`id` = `ls`.`id_label`
```

#### 结果

![https://image.52xiaobei.cn/image/20200630113806554.png](https://image.52xiaobei.cn/image/20200630113806554.png)

#### 加上**GROUP_CONCAT() 关键字**

![https://image.52xiaobei.cn/image/20200630114115893.png](https://image.52xiaobei.cn/image/20200630114115893.png)

备注：**GROUP_CONCAT关键字 可以去重，排序**

**去重：GROUP_CONCAT(DISTINCT l.title SEPARATOR ',' ) AS labels**

**排序：GROUP_CONCAT( l.title order by l.id desc SEPARATOR ',') AS labels**
