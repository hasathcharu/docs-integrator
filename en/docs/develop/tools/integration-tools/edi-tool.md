---
sidebar_position: 9
title: EDI Tool
description: Generate Ballerina types and parsers from EDI schema definitions.
---

# EDI tool

The `bal edi` tool generates Ballerina code from EDI (Electronic Data Interchange) schema definitions, enabling B2B integration with trading partners using standards such as X12 and EDIFACT. The generated code includes record types for EDI segments and transaction sets, along with parser and serializer functions that convert between raw EDI text and type-safe Ballerina records.

The EDI tool uses JSON schema files to describe EDI message structures — segments, fields, delimiters, and data types — and generates code from them.

**Sample EDI schema:**

```json
{
  "name": "SimpleOrder",
  "delimiters": {
    "segment": "~",
    "field": "*",
    "component": ":",
    "repetition": "^"
  },
  "segments": [
    {
      "code": "HDR",
      "tag": "header",
      "minOccurances": 1,
      "fields": [
        { "tag": "code" },
        { "tag": "orderId" },
        { "tag": "organization" },
        { "tag": "date" }
      ]
    },
    {
      "code": "ITM",
      "tag": "items",
      "maxOccurances": -1,
      "fields": [
        { "tag": "code" },
        { "tag": "item" },
        { "tag": "quantity", "dataType": "int" }
      ]
    }
  ]
}
```

**Sample EDI:**

```bash
HDR*HDR123*ACME_CORP*20240519~
ITM*Pen*10~
ITM*Notebook*5~
ITM*Eraser*3~
ITM*Ruler*7~
ITM*Stapler*2~
```

## Prerequisites

Execute the command below to pull the EDI tool from [Ballerina Central](https://central.ballerina.io/).

```bash
bal tool pull edi
```

Verify the tool using the following command.

```bash
bal edi --help
```

## Generating code from an EDI schema

Use `codegen` to generate typed Ballerina records and parser functions from a single EDI schema file.

```bash
bal edi codegen -i path/to/schema.json -o modules/orders/main.bal
```

This generates the following functions in the output file:

- `fromEdiString` — Convert an EDI string to a Ballerina record
- `toEdiString` — Convert a Ballerina record to an EDI string
- `getSchema` — Get the EDI schema as an `EdiSchema` object
- `fromEdiStringWithSchema` — Convert an EDI string to a Ballerina record using a pre-loaded schema
- `toEdiStringWithSchema` — Convert a Ballerina record to an EDI string using a pre-loaded schema

It also generates the corresponding Ballerina record types:

```ballerina
public type Header_Type record {|
   string code = "HDR";
   string orderId?;
   string organization?;
   string date?;
|};

public type Items_Type record {|
   string code = "ITM";
   string item?;
   int? quantity?;
|};

public type SimpleOrder record {|
   Header_Type header;
   Items_Type[] items = [];
|};
```

### Using the generated code

**Parsing incoming EDI messages:**

```ballerina
import ballerina/log;
import <add-the-project-name>.orders;

function processIncomingOrder(string rawEdi) returns error? {
    // Parse EDI text into a typed SimpleOrder record
    orders:SimpleOrder newOrder = check orders:fromEdiString(rawEdi);

    log:printInfo("Order received",
        orderId = newOrder.header.orderId,
        organization = newOrder.header.organization
    );

    foreach orders:Items_Type item in newOrder.items {
        log:printInfo("Item", item = item.item, quantity = item?.quantity);
    }
}
```

**Generating outbound EDI messages:**

```ballerina
import <add-the-project-name>.orders;

function generateOrderEdi() returns string|error {
    orders:SimpleOrder newOrder = {
        header: {
            orderId: "ORD-001",
            organization: "HealthCare Inc.",
            date: "20240101"
        },
        items: [
            {item: "Aspirin", quantity: 100},
            {item: "Insulin", quantity: 50}
        ]
    };

    return orders:toEdiString(newOrder);
}
```

## Generating a library package

Use `libgen` to generate a complete Ballerina library package from a directory of EDI schemas. The library organizes each schema into a separate module and includes REST connectors for EDI-to-JSON and JSON-to-EDI conversions.

```bash
bal edi libgen -p <org/package> -i path/to/schemas/ -o path/to/output/
```

Generated packages can be published to Ballerina Central and reused across projects.

## Converting EDI schemas

The EDI tool can convert standard EDI schema formats into the Ballerina JSON schema format used by `codegen` and `libgen`.

### X12 schema conversion

```bash
bal edi convertX12Schema -i path/to/x12-schema -o path/to/output/
```

### EDIFACT schema conversion

```bash
bal edi convertEdifactSchema -v <version> -t <transaction-type> -o path/to/output/
```

### ESL schema conversion

```bash
bal edi convertESL -b path/to/definitions -i path/to/esl-schema -o path/to/output/
```

## Command reference

### bal edi codegen

Generates Ballerina record types and utility functions from a single EDI schema definition file.

```bash
bal edi codegen -i <schema-path> -o <output-path>
```

| Flag | Required | Description |
| --- | --- | --- |
| `-i`, `--input` | Yes | Path to the EDI schema file (JSON format) |
| `-o`, `--output` | Yes | Output path for the generated Ballerina source file |

### bal edi libgen

Generates a complete Ballerina library package from a collection of EDI schemas.

```bash
bal edi libgen -p <org/package> -i <schema-directory> -o <output-path>
```

| Flag | Required | Description |
| --- | --- | --- |
| `-p`, `--package` | Yes | Package identifier in `org/package` format |
| `-i`, `--input` | Yes | Path to the directory containing EDI schema files |
| `-o`, `--output` | Yes | Output directory for the generated library package |

### bal edi convertX12Schema

Converts an X12 schema to the Ballerina EDI schema format.

```bash
bal edi convertX12Schema -i <input> -o <output> [options]
```

| Flag | Required | Description |
| --- | --- | --- |
| `-i`, `--input` | Yes | Path to the X12 schema file or directory |
| `-o`, `--output` | Yes | Output directory for the converted schema |
| `-H`, `--headers` | No | Enable headers mode |
| `-c`, `--collection` | No | Enable collection mode |
| `-d`, `--segdet` | No | Path to the segment details file |

### bal edi convertEdifactSchema

Converts an EDIFACT schema to the Ballerina EDI schema format.

```bash
bal edi convertEdifactSchema -v <version> -t <type> -o <output>
```

| Flag | Required | Description |
| --- | --- | --- |
| `-v`, `--version` | Yes | EDIFACT version (e.g., `d96a`) |
| `-t`, `--type` | Yes | Transaction type (e.g., `ORDERS`, `INVOIC`) |
| `-o`, `--output` | Yes | Output directory for the converted schema |

### bal edi convertESL

Converts an ESL (EDI Schema Language) file to the Ballerina EDI schema format.

```bash
bal edi convertESL -b <definitions> -i <input> -o <output>
```

| Flag | Required | Description |
| --- | --- | --- |
| `-b`, `--basedef` | Yes | Path to the ESL base definitions |
| `-i`, `--input` | Yes | Path to the ESL schema file |
| `-o`, `--output` | Yes | Output directory for the converted schema |

## What's next

- [Health Tool](health-tool.md) — Generate healthcare integration code
- [XSD Tool](xsd-tool.md) — Generate types from XML schemas
- [Data transformation](/docs/develop/transform/edi) — Transform EDI data in Ballerina
