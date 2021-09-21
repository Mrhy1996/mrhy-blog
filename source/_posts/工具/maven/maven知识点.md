---
title: maven知识点
toc: true
date: 2021-08-14 12:02:24
tags: 工具
url: maven
---

今天整理一下maven的配置，希望自己所学的东西能够及时形成笔记.

<!--more-->

# 基本信息

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.mrhy</groupId>
    <artifactId>mrhy-grow-up</artifactId>
    <packaging>pom</packaging>
    <version>1.0-SNAPSHOT</version>
    <modules>
        <module>mrhy-common</module>
        <module>mrhy-middleware</module>
    </modules>

    <properties>
        <maven.compiler.source>8</maven.compiler.source>
        <maven.compiler.target>8</maven.compiler.target>
        <maven.compiler.source>8</maven.compiler.source>
        <maven.compiler.target>8</maven.compiler.target>
        <project.version>1.0-SNAPSHOT</project.version>
        <spring.version>2.5.1</spring.version>
        <clickhouse.version>0.2.4</clickhouse.version>
        <lombok.version>1.18.16</lombok.version>
        <commons-lang3.version>3.12.0</commons-lang3.version>
        <fastjson.version>1.2.58</fastjson.version>
        <mybatis.version>2.2.0</mybatis.version>
        <disruptor.version>3.4.2</disruptor.version>
        <swagger.version>2.9.2</swagger.version>
        <commons-io.version>2.10.0</commons-io.version>
        <redisson.version>3.16.1</redisson.version>
    </properties>

    <dependencyManagement>
        <dependencies>
            <!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-parent -->
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-parent</artifactId>
                <version>${spring.version}</version>
                <type>pom</type>
            </dependency>
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-dependencies</artifactId>
                <version>${spring.version}</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
            <dependency>
                <groupId>org.mybatis.spring.boot</groupId>
                <artifactId>mybatis-spring-boot-starter</artifactId>
                <version>${mybatis.version}</version>
            </dependency>
            <dependency>
                <groupId>com.alibaba</groupId>
                <artifactId>fastjson</artifactId>
                <version>${fastjson.version}</version>
            </dependency>
            <!--lombok-->
            <dependency>
                <groupId>org.projectlombok</groupId>
                <artifactId>lombok</artifactId>
                <version>${lombok.version}</version>
            </dependency>

            <dependency>
                <groupId>ru.yandex.clickhouse</groupId>
                <artifactId>clickhouse-jdbc</artifactId>
                <version>${clickhouse.version}</version>
            </dependency>
            <!--            工具类-->
            <dependency>
                <groupId>commons-io</groupId>
                <artifactId>commons-io</artifactId>
                <version>${commons-io.version}</version>
            </dependency>
            <dependency>
                <groupId>org.apache.commons</groupId>
                <artifactId>commons-lang3</artifactId>
                <version>${commons-lang3.version}</version>
            </dependency>
            <dependency>
                <groupId>com.lmax</groupId>
                <artifactId>disruptor</artifactId>
                <version>${disruptor.version}</version>
            </dependency>
            <!--swagger-->
            <dependency>
                <groupId>io.springfox</groupId>
                <artifactId>springfox-swagger2</artifactId>
                <version>${swagger.version}</version>
            </dependency>
            <dependency>
                <groupId>io.springfox</groupId>
                <artifactId>springfox-swagger-ui</artifactId>
                <version>${swagger.version}</version>
            </dependency>
            <dependency>
                <groupId>org.redisson</groupId>
                <artifactId>redisson</artifactId>
                <version>${redisson.version}</version>
            </dependency>
            <dependency>
                <groupId>com.mrhy</groupId>
                <artifactId>mrhy-common</artifactId>
                <version>${project.version}</version>
            </dependency>
        </dependencies>
    </dependencyManagement>

</project>
```

