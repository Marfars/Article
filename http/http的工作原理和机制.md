# HTTP工作原理和机制

## HTTP到底是什么

> HyperText Transfer Protocol 超文本传输协议

- 超文本：在电脑中显示的、含有可以指向其他文本的链接的文本

## URL -> HTTP报文

> 示例：http://hencoder.com/users?gender=male

浏览器会将示例链接组合为

```text
GET /users?gender=male HTTP/1.1

Host: hencoder.com
```

- http: 协议类型

- hencoder.com: 服务器地址

- users?gender=male: 路径

### 请求报文格式: Request

```text
请求行 -- GET /users HTTP/1.1

Header -- Host: api.github.com
          Content-Type: text/plain
          Content-length: 243

Body -- ···
```

其中GET是请求方法method，/users是路径path，HTTP/1.1是http版本

### 响应报文格式: Response

```text
状态行 -- HTTP/1.1 200 OK

Headers -- content-type: application/json; charset=utf-8

           cache-control: public, max-age=60, s-maxage=60

           vary: Accept, Accept-Encoding

           etag: W/"02ww332ss13mdhhaa2s13"

           content-encoding: gzip

Body -- [{something}]
```

其中200是status code，OK是status message