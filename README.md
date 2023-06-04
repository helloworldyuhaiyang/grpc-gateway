# grpc-gateway
## description
Implementing a grpc gateway based on traefik. There is no need for stub of proto files, just turn on reflection of GRPC server.

## Run it
```shell
docker-compose up -d
```

## Test it
format: http://127.0.0.1:80/{pack}.{service-name}/{method-name}

GRPC-Content-Type: http header, Default is application/json. Currently, only application/json and application/protobuf formats are supported

```shell
curl http://127.0.0.1:80/go/p1.Hello/SayHello -H "GRPC-Content-Type: application/json" -d '{
    "username": "hellogrpc"
}'


curl http://127.0.0.1:80/go/p2.User/Register -H "GRPC-Content-Type: application/json" -d '{
    "name": "hellogrpc",
    "headerUrl": "www.google.com/test/url",
    "signature": "it is a sign",
    "countryCode": 10
}'
```

## Proto files:

user.proto
```proto
syntax = "proto3";
package p2;
option go_package = "./api/";



service User {
    rpc Register(RegisterIn) returns (RegisterOut) {}
}

message RegisterIn {
    string name = 1;
    string headerUrl = 2;
    string signature = 3;
    int32 countryCode = 4;
}

message RegisterOut {
    int32 code = 1;
    string msg = 2;
}

```

hello.proto
```proto
syntax = "proto3";
package p1;
option go_package = "./api/";

message HelloRequest {
    string username = 1;
}

message HelloResponse {
    string message = 1;
}

message NoArgsRequest {
}

message NoArgsResponse {
}

service Hello {
    rpc SayHello(HelloRequest) returns (HelloResponse){}
    rpc NoArgs(NoArgsRequest) returns (NoArgsResponse){}
}
```


