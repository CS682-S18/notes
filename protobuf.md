Data Serialization and Protocol Buffers
=======================================

#### Reading: Kleppmann Chapter 4 

### Overview

A critical design question for any distributed system is how data will be encoded and decoded, either for saving to a file or sending to another node in the network. This can also be referred to as serialization or marshalling.

Consider an application that implements a DateTimeServer and DateTimeClient. The client queries the server for the current date or time and specifies the format of the reply using [ISO 8601](https://www.w3.org/TR/NOTE-datetime) formatting. The server will reply with the requested information along with a count of the number of other queries it has served.

[JSON](https://www.json.org/) and [XML](https://www.w3.org/XML/) are two common ways to encode data. 

In XML, the messages exchanged between client and server might look as follows:

```xml
<DateTimeRequest>
	<Format>yyyy-MM-dd</Format>
</DateTimeRequest>

<DateTimeResponse>
	<Date>2018-01-17</Date>
	<RequestNumber>5</RequestNumber>
</DateTimeResponse>
```

JSON tends to be a bit less verbose:

```json
{
	"format": "yyyy-MM-dd"
}

{
	"date": "2018-01-17",
	"request_number": 5
}
```

Both XML and JSON are widely supported, widely used for web applications, and conveniently human readable. For applications that implement external APIs or services JSON is a common choice. Though JSON and XML both have external mechanisms for schema support, the toolsets are somewhat complicated. For other types of applications, particularly applications that support internal communication, formats that can be more efficiently encoded are preferred.

### Protocol Buffers

Protocol Buffers (protobuf) is one example of a binary encoding library that makes it very easy to define a schema for your data format; create an object representation of your data; and efficiently encode it for transfer across the network. Thrift is another example of this type of library. Essentially, by defining the schema the encoding scheme is able to omit field names (e.g., format, date, and request_number) from the encoded record and replace with more compact field *tags* specified as part of the schema.

We'll work with proto3, the latest version.

#### Schema

The schema defines the structure of the data and is defined in one or more files ending in the `.proto` extension. 

Let's look at an example taken from the [Language Guide](https://developers.google.com/protocol-buffers/docs/proto3). 

```
syntax = "proto3";

message SearchRequest {
  string query = 1;
  int32 page_number = 2;
  int32 result_per_page = 3;
}
```

> - The first line of the file specifies that you're using proto3 syntax: if you don't do this the protocol buffer compiler will assume you are using proto2. This must be the first non-empty, non-comment line of the file.
- The SearchRequest message definition specifies three fields (name/value pairs), one for each piece of data that you want to include in this type of message. Each field has a name and a type.

Additional rules for specifying the schema include the following:

- Several scalar types are supported, including `string`, `int32`, `float`, `bool`, and `sint64` (more efficient for negative values).

- You can also have other message types and enumerations. 

- The unique numbered tag is used in the binary encoding.

- Multiple messages may be specified in a single file.

- Comments use Java/C syntax.

- Fields may also be `repeated` as below.

```
message SearchResponse {
   repeated Result results = 1;
}
message Result {
  string url = 1;
  string title = 2;
  repeated string snippets = 3;
}
```

Several other options may be specified, including the `java_package` used for the generated classes, and the `java_outer_classname` used for the outermost class you would like to generate.

#### Compiling

Once the schema has been implemented, or anytime it is changed, the compiler must be executed. The compiler will generate one or more Java classes to read and write the data. The command looks like the following.

```
protoc -I=$SRC_DIR --java_out=$DST_DIR $SRC_DIR/fileName.proto
```

The Java classes must then be moved to the appropriate place for your IDE to build and use them.

#### Using the API

Each message will have a corresponding `Message` class and a `Builder` class. The `Message` is immutable and has accessors (getters) for all of the fields. The `Builder` may be used to create and populate the object. 

The `writeTo` and `parseFrom` methods are used to write and read the data, to a file or to a socket input/output stream.

### Getting Started

* [Download](https://developers.google.com/protocol-buffers/) and install the compiler. For the mac, I downloaded [https://github.com/google/protobuf/releases/download/v3.5.1/protoc-3.5.1-osx-x86_64.zip](https://github.com/google/protobuf/releases/download/v3.5.1/protoc-3.5.1-osx-x86_64.zip) and put the `bin` directory in my `PATH`.
* Write your first proto as outlined in the [Developer Guide](https://developers.google.com/protocol-buffers/docs/overview). 

* Compile the proto to produce the corresponding Java code and, if necessary, move it to the appropriate `src` directory.
* Follow the [Java Tutorial](https://developers.google.com/protocol-buffers/docs/javatutorial) for using the defined structures in your program. 
