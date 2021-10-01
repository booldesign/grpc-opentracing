# OpenTracing support for gRPC in Go

源码来自 github.com/grpc-ecosystem/grpc-opentracing，感谢作者

* server端增加了过滤 grpc 健康检查

## Documentation

See the basic usage examples below and the [package documentation on
godoc.org](https://godoc.org/github.com/grpc-ecosystem/grpc-opentracing/go/otgrpc).

## Client-side usage example

Wherever you call `grpc.Dial`:

```go
// You must have some sort of OpenTracing Tracer instance on hand.
var tracer opentracing.Tracer = ...
...

// Set up a connection to the server peer.
conn, err := grpc.Dial(
address,
... // other options
grpc.WithUnaryInterceptor(
otgrpc.OpenTracingClientInterceptor(tracer)),
grpc.WithStreamInterceptor(
otgrpc.OpenTracingStreamClientInterceptor(tracer)))

// All future RPC activity involving `conn` will be automatically traced.
```

## Server-side usage example

Wherever you call `grpc.NewServer`:

```go
// You must have some sort of OpenTracing Tracer instance on hand.
var tracer opentracing.Tracer = ...
...

// Initialize the gRPC server.
s := grpc.NewServer(
... // other options
grpc.UnaryInterceptor(
otgrpc.OpenTracingServerInterceptor(tracer)),
grpc.StreamInterceptor(
otgrpc.OpenTracingStreamServerInterceptor(tracer)))

// All future RPC activity involving `s` will be automatically traced.
```

