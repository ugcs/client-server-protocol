# UgCS HCI protocol and file formats

## HCI protocol

HCI protocol allows interacting with UgCS server in order to plan and execute missions, control vehicles, receive vehicle telemetry etc.

UgCS has at least two clients using this protocol: Desktop client and [Telemetry viewer](https://github.com/ugcs/ugcs-telemetry-viewer).

Messages between HCI and UgCS server are serialized via [Protocol Buffers](https://developers.google.com/protocol-buffers). The .proto files published [here](https://github.com/ugcs/client-server-protocol/tree/master/proto).

[See details](hci-protocol.md)

## Route files schema
UgCS allows users to export routes to files. [This document](attachments/exported_routes_schema.json) describes the format of such files in the [JSON Schema](https://json-schema.org/) format.
