# CURL命令

1. POST命令

   ```
   curl  http://ip:port/message/messageList -X POST -H "Content-Type:application/json" -H "XXX:XXXX" -H "" -d '{"XXXX":"XXX","pageNum":1,"pageSize":20}'
   ```

2. GET命令

   ```
   curl  http://ip:port/message/messageList?a=1&b=2
   
   注意：在linux系统中& 会使进程系统后台运行，必须对&进行下转义为（\&）
   ```



### GET

#### 发送 GET 请求，并将结果打印出来

```
curl https://catonmat.net
```

#### 发送 GET 请求，并将 response 的 body 输出到文件里

```
curl -o output.txt https://catonmat.net
```

------

### POST

#### 发送空的 POST 请求

```
curl -X POST https://catonmat.net
```

#### 发送有参数的 POST 请求

```
curl -d 'login=Queen&password=123' -X POST https://google.com/login
```

当使用 `-d` 参数传入表单数据时，会自动将 `Content-Type` 设置为 `application/x-www-form-urlencoded`，同时当使用 `-d` 参数时，`-X POST` 可以省略。

```
-d` 参数也可以分开写：`curl -d 'login=Queen' -d 'password' https://google.com/login
```

#### 发送可重定向的有参 POST 请求

```
curl -L -d 'tweet=hi' https://api.twitter.com/tweet
```

`curl` 默认不支持重定向，加入 `-L` 参数明确要求可以任意重定向。

#### 发送带 JSON 数据的 POST 请求

```
curl -d '{"login":"Queen","password":"123"}' -H 'Content-Type: application/json' https://google.com/login
```

使用 `-d` 参数传入 JSON 数据，同时必须使用 `-H` 参数显式指明 `Content-Type` 为 `application/json`

#### 发送带 XML 数据的 POST 请求

```
curl -d '<user><login>ann</login><password>123</password></user>' -H 'Content-Type: application/xml' https://google.com/login
```

使用 `-d` 参数传入 xml 数据，同时必须使用 `-H` 参数显式指明 `Content-Type` 为 `application/xml`

#### 发送带纯文本数据的 POST 请求

```
curl -d 'hello world' -H 'Content_Type: text/plain' https://google.com/login
```

使用 `-d` 参数传入纯文本数据，同时使用 `-H` 参数显示指明 `Content-Type` 为 `text/plain`

#### 发送带某个文件中的数据的 POST 请求

```
curl -d '@data.txt' https://google.com/login
```

使用 `-d` 参数传入数据，其中 `@` 符号后面指明数据来源于那个文件。

#### 显式将 POST 数据进行 URL 编码

```
curl --data-urlencode 'comment=hello 中国' https://google.com/login
```

使用 `-d` 时，默认认为 POST 数据已经进行了 [URL 编码]，但是如果数据并没有经过编码处理，那么就需要使用 `--data-urlencode` 将未被编码的数据进行 URL 编码之后，再发送

#### POST 一个二进制文件

```
curl -F 'newfile=@pig.png' https://google.com/profile
```

使用 `-F` 参数强制让 `curl` 发送一个多部分表单数据，会自动将 `Content-Type` 设置为 `multipart/form-data`，该语句让 `curl` 读取 `pig.png` 中的数据并上传至 `https://google.com/profile` ，并将文件命名为 `newfile`

#### 发送一个二进制数据，并设置它的 MIME 类型

```
curl -F 'newfile=@pig.png;type=image/png'
```

使用 `-F` 参数上传了一个二进制文件，并设置该文件的 MIME 类型为 `image/png`，若不设置，则默认为 `application/octet-stream`

[MIME 类型] (Multipurpose Internet Mail Extensions，媒体类型) 是一种标准，用来表示文档、文件和字节流的性质和格式。

#### 发送二进制数据并更改文件名称

```
curl -F 'newfile=@pig.png;filename=cat.png' https://google.com/login
```

和前两个类似，该语句通过 POST 请求发送一个二进制数据，并且更改文件名称，这样服务器看到的就不是原始名称 `pig.png`，而是新名称 `cat.png`，并且保存为 `newfile`

------

### 构建一个查询字符串(使用 `-G` 参数)

该语句可以为 `GET` 请求构建查询字符串，通过 `-G` 和 `-d` 或者 `--data-urlencode` 参数结合使用。`-G` 参数会将所有的 `-d` 参数使用 `&` 连接，再用 `?` 和 URL 连接。

#### 构建两个查询参数

```
curl -G -d 'q=kitties' -d 'count=20' https://google.com/search
```

该语句会生成一个 `GET` 请求：`https://google.com/search?q=kitties&count=20`

**注意**：如果遗漏 `-G` 参数，就会变成一个 POST 请求

#### URL 编码一个查询参数

```
curl -G --data-urlencode 'comment=Spring is coming' https://catonmat.net
--data-urlencode` 与 `-d` 类似，但是会有 URL 编码的功能，上面的 GET 请求会变成 `https://catonmat.net/comment=Spring%20is%20coming
```
