---
title: curl命令详解与实战
toc: true
date: 2021-07-29 09:52:47
tags: 工具
url: curl-command
---

curl 是常用的命令行工具，用来请求 Web 服务器。它的名字就是客户端（client）的 URL 工具的意思。

它的功能非常强大，命令行参数多达几十种。如果熟练的话，完全可以取代 Postman 这一类的图形界面工具。

今天我总结一下，以备不时之需。

<!-- more-->

# curl命令大全

- 不带任何参数就是get命令

  ```shell
   curl https://www.example.com
  ```

- `-X`指定请求方式

  ```shell
  curl -X POST https://www.example.com
  
  
  curl -X GET https://www.example.com
  ```

- `-d` 参数用于发送POST请求的数据体

  ```shell
  curl -d'login=emma＆password=123' -X POST https://google.com/login
  ```

- `-G`参数用来构造 URL 的查询字符串。

  ```shell
   curl -G -d 'q=kitties' -d 'count=20' https://google.com/search
  ```

  上面命令会发出一个 GET 请求，实际请求的 URL 为`https://google.com/search?q=kitties&count=20`。如果省略`--G`，会发出一个 POST 请求。

  如果数据需要 URL 编码，可以结合`--data--urlencode`参数。

  ```shell
  curl -G --data-urlencode 'comment=hello world' https://www.example.com
  ```

- `-H`参数添加 HTTP 请求的标头。

  ```shell
  curl -d '{"login": "emma", "pass": "123"}' -H 'Content-Type: application/json' https://google.com/login
  ```

- `-i`参数打印出服务器回应的 HTTP 标头并打印返回信息。

  ```shell
  curl -i https://www.example.com
  ```

- -I 只打印服务器回应的HTTP标头

  ```shell
  curl -I www.baidu.com
  ```

- `-F`参数用来向服务器上传二进制文件。

  ```shell
  curl -F 'file=@photo.png' https://google.com/profile
  ```

  上面命令会给 HTTP 请求加上标头`Content-Type: multipart/form-data`，然后将文件`photo.png`作为`file`字段上传。

  ```shell
  curl -F 'file=@photo.png;type=image/png' https://google.com/profile
  ```

  上面的`-F`参数可以指定 MIME 类型

  ```bash
   curl -F 'file=@photo.png;filename=me.png' https://google.com/profile
  ```

  上面命令中，原始文件名为`photo.png`，但是服务器接收到的文件名为`me.png`

- `-o`参数将服务器的回应保存成文件，等同于`wget`命令

  ```shell
  curl -o example.html https://www.example.com
  ```

- `-O`参数将服务器回应保存成文件，并将 URL 的最后部分当作文件名

  ```bash
   curl -O https://www.example.com/foo/bar.html
  ```

