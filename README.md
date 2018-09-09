# tsdump
[![Build Status](https://api.travis-ci.org/voidint/tsdump.png)](https://travis-ci.org/voidint/tsdump)
[![Coverage Status](https://coveralls.io/repos/github/voidint/tsdump/badge.svg?branch=master)](https://coveralls.io/github/voidint/tsdump?branch=master)

## 目录
- [特性](#特性)
- [安装](#安装)
- [基本使用](#基本使用)
- [Changelog](#changelog)

## 特性
- 支持将数据库(当前仅支持`MySQL`)及其表结构的元数据以`text`、`markdown`、`json`、`csv`形式输出。


## 安装
- 源代码安装
  ```shell
  $ go get -u github.com/voidint/tsdump
  ```
- 二进制安装

  [Download](https://github.com/voidint/tsdump/releases)

## 基本使用

```shell
$ tsdump --help
NAME:
  tsdump - Database table structure dump tool.

USAGE:
  tsdump [OPTIONS] [database [table ...]]

VERSION:
  0.3.0

AUTHOR:
  voidnt <voidint@126.com>

OPTIONS:
  -D, --debug                 enable debug mode
  -h value, --host value      connect to host (default: "127.0.0.1")
  -P value, --port value      port number to use for connection (default: 3306)
  -u value, --user value      user for login if not current user (default: "voidint")
  -p value, --password value  password to use when connecting to server. If password is not given it's solicited on the tty.
  -V value, --viewer value    output viewer. Optional values: txt|csv|json|md (default: "txt")
  -o value, --output value    write to a file, instead of STDOUT
  --help                      show help
  --version, -v               print the version

COPYRIGHT:
  Copyright (c) 2017, 2018, voidint. All rights reserved.
```

- 使用`root`用户创建一个名为`mydb`的数据库实例，以及一张`student`的表。
    ```SQL
    CREATE DATABASE IF NOT EXISTS `mydb` DEFAULT CHARACTER SET utf8 COLLATE utf8_general_ci;

    USE `mydb`;

    CREATE TABLE `student` (
    `sno` char(8) NOT NULL COMMENT '学号',
    `sname` varchar(255) NOT NULL COMMENT '姓名',
    `gender` char(2) DEFAULT NULL COMMENT '性别',
    `native` char(20) DEFAULT NULL COMMENT '籍贯',
    `birthday` datetime DEFAULT NULL COMMENT '出生日期',
    `dno` char(6) DEFAULT NULL COMMENT '所在院系',
    `spno` char(8) DEFAULT NULL COMMENT '专业代码',
    `classno` char(4) DEFAULT NULL COMMENT '班级号',
    `entime` date DEFAULT NULL COMMENT '入校时间',
    `home` varchar(40) DEFAULT NULL COMMENT '家庭住址',
    `tell` varchar(40) DEFAULT NULL COMMENT '联系电话',
    PRIMARY KEY (`sno`)
    ) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COMMENT='学生信息表';
    ```

- 将目标数据库及其所有表的表结构数据以表格形式输出到console
    ```shell
    $ tsdump -h 127.0.0.1 -P 3307 -u root mydb
    Enter Password:
    |----------|---------------|--------------------|
    | DATABASE | CHARACTER SET |     COLLATION      |
    |----------|---------------|--------------------|
    | mydb     | utf8mb4       | utf8mb4_general_ci |
    |----------|---------------|--------------------|

    TABLE:	student	学生信息表
    |----------|--------------|----------|-----|---------|---------------|--------------------|----------|
    |  COLUMN  |  DATA TYPE   | NULLABLE | KEY | DEFAULT | CHARACTER SET |     COLLATION      | COMMENT  |
    |----------|--------------|----------|-----|---------|---------------|--------------------|----------|
    | sno      | char(8)      | NO       | PRI |         | utf8mb4       | utf8mb4_general_ci | 学号     |
    | sname    | varchar(255) | NO       |     |         | utf8mb4       | utf8mb4_general_ci | 姓名     |
    | gender   | char(2)      | YES      |     |         | utf8mb4       | utf8mb4_general_ci | 性别     |
    | native   | char(20)     | YES      |     |         | utf8mb4       | utf8mb4_general_ci | 籍贯     |
    | birthday | datetime     | YES      |     |         |               |                    | 出生日期 |
    | dno      | char(6)      | YES      |     |         | utf8mb4       | utf8mb4_general_ci | 所在院系 |
    | spno     | char(8)      | YES      |     |         | utf8mb4       | utf8mb4_general_ci | 专业代码 |
    | classno  | char(4)      | YES      |     |         | utf8mb4       | utf8mb4_general_ci | 班级号   |
    | entime   | date         | YES      |     |         |               |                    | 入校时间 |
    | home     | varchar(40)  | YES      |     |         | utf8mb4       | utf8mb4_general_ci | 家庭住址 |
    | tell     | varchar(40)  | YES      |     |         | utf8mb4       | utf8mb4_general_ci | 联系电话 |
    |----------|--------------|----------|-----|---------|---------------|--------------------|----------|
    ```

- 将目标数据库下目标表的表结构数据输出到markdown文件
    ```shell
    $ tsdump -h 127.0.0.1 -P 3307 -u root -V md -o ./student.md mydb student
    ```

    output:

    | DATABASE | CHARACTER SET |     COLLATION      |
    |----------|---------------|--------------------|
    | mydb     | utf8mb4       | utf8mb4_general_ci |

    ### `student`
    学生信息表

    |  COLUMN  |  DATA TYPE   | NULLABLE | KEY | DEFAULT | CHARACTER SET |     COLLATION      | COMMENT  |
    |----------|--------------|----------|-----|---------|---------------|--------------------|----------|
    | sno      | char(8)      | NO       | PRI |         | utf8mb4       | utf8mb4_general_ci | 学号     |
    | sname    | varchar(255) | NO       |     |         | utf8mb4       | utf8mb4_general_ci | 姓名     |
    | gender   | char(2)      | YES      |     |         | utf8mb4       | utf8mb4_general_ci | 性别     |
    | native   | char(20)     | YES      |     |         | utf8mb4       | utf8mb4_general_ci | 籍贯     |
    | birthday | datetime     | YES      |     |         |               |                    | 出生日期 |
    | dno      | char(6)      | YES      |     |         | utf8mb4       | utf8mb4_general_ci | 所在院系 |
    | spno     | char(8)      | YES      |     |         | utf8mb4       | utf8mb4_general_ci | 专业代码 |
    | classno  | char(4)      | YES      |     |         | utf8mb4       | utf8mb4_general_ci | 班级号   |
    | entime   | date         | YES      |     |         |               |                    | 入校时间 |
    | home     | varchar(40)  | YES      |     |         | utf8mb4       | utf8mb4_general_ci | 家庭住址 |
    | tell     | varchar(40)  | YES      |     |         | utf8mb4       | utf8mb4_general_ci | 联系电话 |

- 将用户权限范围内数据库及其表结构数据输出到csv文件
    ```shell
    $ tsdump -h 127.0.0.1 -P 3307 -u root -V csv > ./mydb.csv
    ```

- 将目标数据库及其所有表的表结构数据输出到JSON文件
    ```shell
    $ tsdump -h 127.0.0.1 -P 3307 -u root -V json mydb > mydb.json
    ```


## Changelog
### 0.4.0 - 2018/03/25
- `Add feature`: 支持`UNIX Domain Socket`方式连接数据库。[#18](https://github.com/voidint/tsdump/issues/18)
- `Modify feature`: 优化JSON视图输出格式。[#19](https://github.com/voidint/tsdump/issues/19)

### 0.3.0 - 2018/01/05
- `Add feature`: 支持通过`-p`选项指定数据库登录密码。[#16](https://github.com/voidint/tsdump/issues/16)
- `Fixbug`: 标准输出重定向后获得的内容中包含有`Enter Password:`字样。[#17](https://github.com/voidint/tsdump/issues/17)

### 0.2.0 - 2018/01/01
- `Add feature`: 支持从stdin console中读取数据库登录密码。[#5](https://github.com/voidint/tsdump/issues/5)
- `Fixbug`: 修正help信息。[#6](https://github.com/voidint/tsdump/issues/6)
- `Add feature`: 支持命令行参数指定目标数据库和表。[#12](https://github.com/voidint/tsdump/issues/12)
- `Add feature`: 支持通过`-h`选项指定主机名。[#14](https://github.com/voidint/tsdump/issues/14)

### 0.1.0 - 2017/12/31
- `Add feature`: 支持以`csv`视图方式导出表结构数据。[#1](https://github.com/voidint/tsdump/issues/1)
- `Add feature`: 支持以`markdown`视图方式导出表结构数据。[#2](https://github.com/voidint/tsdump/issues/2)
- `Add feature`: 支持以`text`视图方式导出表结构数据。[#3](https://github.com/voidint/tsdump/issues/3)
- `Add feature`: 支持以`json`视图方式导出表结构数据。[#4](https://github.com/voidint/tsdump/issues/4)
