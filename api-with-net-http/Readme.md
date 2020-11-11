# REST API with Standard Library net/http

## Run the example
```
git checkout origin/standard-library-net-http-03
```

If you are not already in the folder
```
cd api-with-net-http
```

```
go run main.go
```

```
curl localhost:7999
```

You should see output `hello world` printed.

## Why a Struct
In our previous example we implemented handler interface on a `anything` type. That worked. So why we are using a `server` struct?
The benefit of using a struct becomes apparent when we build more complex servers. And want to have other methods / types in the struct for instance logger.

## Handlerfunc

The [HandlerFunc](https://golang.org/pkg/net/http/#HandlerFunc) type is an adapter to allow the use of ordinary functions as HTTP handlers. If f is a function with the appropriate signature, HandlerFunc(f) is a Handler that calls f.

We can pass in any function / method that has the same signature as `ServeHTTP` as a handler.

## JSON
For net/http response we can technically write any content we want. But in most cases our data is not read by humans but consumed by other services / applications. So having structured data is a big part of REST. In the olden days `XML` was the go to format for REST but now a days most REST API use `JSON` as the format for the data.

Go has great support for `JSON` out of the box.

Once you run this version of the application you can test the output

```bash
curl -v localhost:7999/user
```

Your output would looks somehting like this

```shell
*   Trying ::1...
* TCP_NODELAY set
* Connected to localhost (::1) port 7999 (#0)
> GET /user HTTP/1.1
> Host: localhost:7999
> User-Agent: curl/7.64.1
> Accept: */*
> 
< HTTP/1.1 200 OK
< Content-Type: application/json
< Date: Wed, 05 Aug 2020 05:46:31 GMT
< Content-Length: 64
< 
{"username":"moficodes","email":"moficodes@gmail.com","age":27}
* Connection #0 to host localhost left intact
* Closing connection 0
```

## HTTP Verbs
In the previous example we had a method called `getUser`. It is pretty clear it is a GET operation. But if you were to make a request like
```
curl -x POST localhost:7999/user
```
You would still see the same output. 

This might seem weird since we had named our function `getUser` we probably was expecting to only do get in that route. But naming our function has no effect on the HTTP verb we allow. 

```go
http.HandleFunc("/user", s.getUser)
```
This is the line of code that registers that route. And all it says is anytime a request comes we will be serving that using the `s.getUser` HandlerFunc.

net/http does not have direct support for http verbs like GET, POST, PUT, DELETE etc. But it is easily implementable. This is also a selling point for other library / framework that is more developer friendly in letting us control the allowed http methods.