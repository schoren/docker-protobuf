FORKED FROM https://github.com/jaegertracing/docker-protobuf
BUILD THAT WORKS WITH M1 MAC: https://hub.docker.com/repository/docker/schoren

```
docker run schoren/protobuf:0.3.1
```

# Protocol Buffers + Docker

[![](https://images.microbadger.com/badges/version/jaegertracing/protobuf.svg)](https://hub.docker.com/r/jaegertracing/protobuf/tags)

A lightweight `protoc` Docker image, published as `jaegertracing/protobuf` to [Docker Hub](https://hub.docker.com/r/jaegertracing/protobuf/tags), with all dependencies built-in, to generate code in multiple languages. Forked from https://github.com/TheThingsIndustries/docker-protobuf.

## Purpose

`gogoproto` annotations in proto files help make internal domain model types more efficient in golang, but using these proto files to generate code in other languages requires to include these dependencies anyway. The `Dockerfile` in this repo compiles all dependencies into the image, for easy code generation in multiple languages.

## Contents

`Dockerfile` configured with dependencies specific to the [Jaeger](github.com/jaegertracing/jaeger) project. 

## What's included in the image
- https://github.com/ckaznocha/protoc-gen-lint
- https://github.com/gogo/protobuf
- https://github.com/golang/protobuf
- https://github.com/google/protobuf
- https://github.com/grpc-ecosystem/grpc-gateway
- https://github.com/grpc/grpc
- https://github.com/grpc/grpc-java

## Supported languages
- C#
- C++
- Go
- Java / JavaNano (Android)
- JavaScript
- Objective-C
- PHP
- Python
- Ruby

## Usage
```
$ docker run --rm -v<some-path>:<some-path> -w<some-path> jaegertracing/protobuf [OPTION] PROTO_FILES
```

For help try:
```
$ docker run --rm jaegertracing/protobuf --help
```

### To generate language specific code

1. Make sure you have the `model.proto` file present in `${PWD}`

2. Use any of the language specific options -
```
  --cpp_out=OUT_DIR           Generate C++ header and source.
  --csharp_out=OUT_DIR        Generate C# source file.
  --java_out=OUT_DIR          Generate Java source file.
  --js_out=OUT_DIR            Generate JavaScript source.
  --objc_out=OUT_DIR          Generate Objective C header and source.
  --php_out=OUT_DIR           Generate PHP source file.
  --python_out=OUT_DIR        Generate Python source file.
  --ruby_out=OUT_DIR          Generate Ruby source file.
```

Example for Java:
```
docker run --rm -u $(id -u) -v${PWD}:${PWD} -w${PWD} jaegertracing/protobuf:latest --proto_path=${PWD} \
    --java_out=${PWD} -I/usr/include/github.com/gogo/protobuf ${PWD}/model.proto
```

CLI options:
- `--proto_path`: The path where protoc should search for proto files
- `--java_out`  : Generate Java code in the provided path

#### Generate dependencies

The generated code might require dependencies on packages like GoGo or Swagger.
The code for these modules can be also generated by using this docker image:

```
docker run --rm -u $(id -u) -v${PWD}:${PWD} -w${PWD} jaegertracing/protobuf:latest --proto_path=${PWD} \
    --java_out=${PWD} /usr/include/github.com/gogo/protobuf/gogoproto/gogo.proto
```

Use this command to find the path to proto files included in the image:
```
docker run --rm -it --entrypoint=/bin/sh jaegertracing/protobuf:latest -c "find /usr/include -name *.proto"
```
