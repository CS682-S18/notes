Protocol Buffers
================


### Getting Started

* [Download](https://developers.google.com/protocol-buffers/) and install the compiler. For the mac, I downloaded [https://github.com/google/protobuf/releases/download/v3.5.1/protoc-3.5.1-osx-x86_64.zip](https://github.com/google/protobuf/releases/download/v3.5.1/protoc-3.5.1-osx-x86_64.zip) and put the `bin` directory in my `PATH`.
* Write your first proto as outlined in the [Developer Guide](https://developers.google.com/protocol-buffers/docs/overview). An example is outlined below:

```
syntax = "proto3";

//from protobuf directory compile this as follows:
//protoc -I=protos/ --java_out=src/main/java/ protos/dateserver.proto

option java_package = "dateserver";
option java_outer_classname = "DateServerMessages";

message DateServerRequest {
	string format = 1;
}

message DateServerResponse {
	string date = 1;
	int32 request_number = 2;
}
```

* Compile the proto to produce the corresponding Java code and, if necessary, move it to the appropriate `src` directory.
* Follow the [Java Tutorial](https://developers.google.com/protocol-buffers/docs/javatutorial) for using the defined structures in your program. For example:

```
String format = "yyyy-MM-dd";
DateServerRequest request = DateServerRequest.newBuilder().setFormat(format).build();
request.writeDelimitedTo(outstream);

DateServerRequest request = DateServerRequest.parseDelimitedFrom(instream);
String format = request.getFormat();
```
