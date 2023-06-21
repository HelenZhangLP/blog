---
title: Database
tags:
- 数据库
---

## 数据库
数据库（database）是用来 <span class='custom-box custom-box-933'>组织、存储和管理</span>数据的仓库。
数据的来源：出行记录、消费记录、浏览网页，消息等。
数据的种类：文本数据、图像、音乐、声音等
用户可以对数据库进行增、删、改、查操作

## 常见数据库
* MySQL 数据库（使用最广，流行度最高，开源免费；Community free + Entrerprise charge）
* Oracle 数据库 (收费)
* SQL Server 数据库（charge）
* Mongodb 数据库（Community free + Entrerprise charge）

> MySQL/Oracle/SQL Server 为 <span class='custom-box custom-box-933'>传统型数据库</span>（又叫 <span class='custom-box custom-box-393'>关系型数据库或 sql 数据库</span>）设计理念相同，用法类似。
> Mongodb <span class='custom-box custom-box-933'>新型数据库，非关系型数据库，NoSQL 数据库</span>。在一定程序上弥补了传统数据库缺陷。

## 传统型数据库的数据组织结构
数据以什么样的结构进行存储，传统型数据库，数据的组织结构分为数据库，数据表，数据行，字段这4个部分组成
项目开发库中，表、行、字段的关系：
* 一般每个项目都对应独立的数据库
* 不同的数据存储在数据库不同的表中，如users,books
* 每个表中具体存储哪些信息，由字段决定。如 users:id,name,age 等
* 每一行一条数据。

## 数据库软件
* MySQL Server 用于数据存储与服务的软件；对应的安装包为 mysql-*.*.8-mocos****.dmg
* MySQL Workbench MySQL 可视化管理工具,相应的安装包为 mysql-Workbench-Community-***-macos-***.dmg