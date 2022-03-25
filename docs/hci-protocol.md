# Client protocol

### Overview
HCI protocol is an application level protocol. It is a stateful protocol used for mission planning.


### General Concepts
HCI protocol is a request/response protocol. Any message sent by a client to a server is a request message (or simply a request), and a server reply message is a response.

Communication is initiated by a client and is followed by one or several request messages. In a data stream, all messages are separated into blocks, transmitting one after another.

Note: There is no any guaranteed order of server responses. That means a client cannot perform an order-based synchronization of requests and responses. E.g., if sequence <M1, M2, M3> is sent by a client and <R1, R2, R3> are corresponding server responses, client can receive them in order of any of the possible permutations:  <R1, R2, R3>, <R1, R3, R2>, <R2, R1, R3>, <R2, R3, R1>, <R3, R1, R2>, <R3, R2, R1>.

### Packet Structure
Block (or packet) is a sequence of fields, described by the table.

|Field|Size|Description|
|-------------------|--------------|-----------|
|Signature          |2             |Marker: 0x4850 ("HP", stands for "HCI Protobuf")|
|Version            |2             |Protocol version. Current value is 1.|
|Instance Identifier|4             |Message correlation identifier. Can be used to match request-response pairs within the communication session.|
|Message Type       |4             |Every supported message (protobuf structure) has its own unique type number. It indicates how packet data should be deserialized (decoded) into object. [Message types.](https://github.com/ugcs/ugcs-java-sdk/blob/master/ucs-api/src/main/java/com/ugcs/ucs/proto/mapping/HciMessageMapping.java)|
|Message Length     |4             |Length of the message data field. Allowed to be 0.|
|Message Data       |Message Length|Decoded packet message. Encoding protobuf binary.|

Server guarantees response value is always equal to the associated request’s one. However, it does not track its uniqueness within the session.
Every party (client and server) should ensure correctness of the output stream and avoid packet overlapping. If a message does not satisfy HCI protocol structure, the receiver should discard it.

Packet is a general container of HCI protocol messages. If packet contains a non-empty data field, it represents a serialized message object, encoded by the protocol buffers binary serializer.

Note: https://developers.google.com/protocol-buffers/

Messages are described by two .proto files. Messages.proto file defines all possible message types and their structure, Domain.proto files defines structure of the domain types used in requests and responses.


###Obtaining Client Identifier
To create a new client session HCI should send an AuthorizeHciRequest message and set the clientId field value to -1. In this case UCS will create a new session object and return its identity as clientId field of the AuthorizeHciResponse message.

Any HCI session should be associated with an authenticated user. To authenticate user client should provide its credentials via LoginRequest message.

If client application wants to attach to an existing session, it should specify client id in a corresponding field.

###Examples
Examples on [UgCS GitHub](https://github.com/ugcs/ugcs-java-sdk/tree/master/ucs-client/src/main/java/com/ugcs/ucs/client/samples)