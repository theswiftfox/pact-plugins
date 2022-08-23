# Example Go provider project

This example project has a gRPC provider for the area calculator service.
It tests the following interaction from the proto file:

```protobuf
  rpc calculate (ShapeMessage) returns (AreaResponse) {}
```

You need to [install the Pact Go library](https://github.com/pact-foundation/pact-go/tree/2.x.x#installation). 

## gRPC plugin

To run the test in this project, it requires the gRPC plugin to be installed. See the [documentation on that plugin](https://github.com/pactflow/pact-protobuf-plugin#installation).

## Generated gRPC stub

To generate the Go code for the proto file, you need to install the Protobuf compiler (protoc), and install the Go
protobuf and grpc protoc plugins, and then run `protoc --go_out=. --go-grpc_out=.  --proto_path ../proto  ../proto/area_calculator.proto`.

## Verifying the provider

You can verify the provider by using the Pact Verification CLI tool. For example, to verify the Pact file from the Go consumer example, start the provider and then run the verifier.

```shell
❯ go run provider.go
2022/08/23 17:07:30 Server started 127.0.0.1:39821
```

```shell
❯ pact_verifier_cli -f ../consumer-go/pacts/grpc-consumer-go-area-calculator-provider.json --transport grpc -p 39821
2022-08-23T07:08:11.615313Z  WARN tokio-runtime-worker pact_plugin_driver::metrics: 

Please note:
We are tracking this plugin load anonymously to gather important usage statistics.
To disable tracking, set the 'pact_do_not_track' environment variable to 'true'.


Verifying a pact between grpc-consumer-go and area-calculator-provider

  calculate rectangle area request

  Given a Calculator/calculateMulti request
      with an input .area_calculator.AreaRequest message
      will return an output .area_calculator.AreaResponse message [OK]
    generates a message which
      has a matching body (OK)

```