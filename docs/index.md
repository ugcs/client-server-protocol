# UgCS client-server protocol and file formats

## Client-server protocol
Messages between UgCS client and UgCS server are serialized via [Protocol Buffers](https://developers.google.com/protocol-buffers). The .proto files published [here](https://github.com/ugcs/client-server-protocol/tree/master/proto).

## Route files schema
UgCS allows users to export routes to files. [This document](attachments/exported_routes_schema.json) describes the format of such files in the [JSON Schema](https://json-schema.org/) format.
