Invalid requests
================

### Spaces in Content-Length

<!-- meta={"type": "request"} -->
```http
POST / HTTP/1.1
Content-Length: 4 2


```

```log
off=0 message begin
off=5 len=1 span[url]="/"
off=17 len=14 span[header_field]="Content-Length"
off=33 len=2 span[header_value]="4 "
off=35 error code=11 reason="Invalid character in Content-Length"
```

### Spaces in Content-Length #2

<!-- meta={"type": "request"} -->
```http
POST / HTTP/1.1
Content-Length: 13 37


```

```log
off=0 message begin
off=5 len=1 span[url]="/"
off=17 len=14 span[header_field]="Content-Length"
off=33 len=3 span[header_value]="13 "
off=36 error code=11 reason="Invalid character in Content-Length"
```

### ICE protocol and GET method

<!-- meta={"type": "request"} -->
```http
GET /music/sweet/music ICE/1.0
Host: example.com


```

```log
off=0 message begin
off=4 len=18 span[url]="/music/sweet/music"
off=27 error code=8 reason="Expected SOURCE method for ICE/x.x request"
```

### Headers separated by CR

<!-- meta={"type": "request"} -->
```http
GET / HTTP/1.1
Foo: 1\rBar: 2


```

```log
off=0 message begin
off=4 len=1 span[url]="/"
off=16 len=3 span[header_field]="Foo"
off=21 len=1 span[header_value]="1"
off=23 error code=3 reason="Missing expected LF after header value"
```

### Invalid header token #1

<!-- meta={"type": "request", "noScan": true} -->
```http
GET / HTTP/1.1
Fo@: Failure


```

```log
off=0 message begin
off=4 len=1 span[url]="/"
off=18 error code=10 reason="Invalid header token"
```

### Invalid header token #2

<!-- meta={"type": "request", "noScan": true} -->
```http
GET / HTTP/1.1
Foo\01\test: Bar


```

```log
off=0 message begin
off=4 len=1 span[url]="/"
off=19 error code=10 reason="Invalid header token"
```