---
sidebar_position: 5
title: EDI Processing
description: Parse, transform, and generate EDI documents.
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

# EDI processing

Work with Electronic Data Interchange (EDI) formats used in supply chain, healthcare, and financial integrations. Ballerina provides the `ballerina/edi` module for parsing and serializing EDI data, and the `bal edi` CLI tool for generating type-safe code from EDI schemas.

## EDI standards overview

Ballerina supports the major EDI standards through schema-based processing:

- **X12** -- The ANSI ASC X12 standard used widely in North America for purchase orders (850), invoices (810), advance ship notices (856), and more.
- **EDIFACT** -- The UN/EDIFACT international standard used in global trade, logistics, and customs.
- **ESL** -- Electronic Shelf Labeling and custom delimited formats.

All standards follow the same workflow: define a schema, generate Ballerina code, then parse or serialize EDI documents using typed records.

## Setting up the EDI tool

Pull the `bal edi` tool to generate Ballerina code from EDI schemas.

```bash
bal tool pull edi
```

Once installed, the tool provides commands for code generation and schema conversion.

## Generating code from EDI schemas

The `bal edi` tool reads an EDI schema (in JSON format) and generates Ballerina record types and parser/serializer functions. Let's add the generated code into a separate library.

<Tabs>
<TabItem value="ui" label="Visual Designer" default>

1. After creating a new integration, click the **+ Add Artifact** icon.
2. Select **Library** as the type, enter `documents` as the library name, and click **Add Library**.

   ![Add New Library](/img/develop/transform/edi/add-new-library.png)

</TabItem>
</Tabs>

Here we will use the following json as the edi schema

```json
{
  "name": "Document",
  "delimiters": {
    "segment": "~",
    "field": "*",
    "component": ":",
    "repetition": "^"
  },
  "segments": [
    {
      "code": "BEG",
      "tag": "BEG",
      "minOccurs": 1,
      "maxOccurs": 1,
      "fields": [
        {"tag": "purposeCode"},
        {"tag": "typeCode"},
        {"tag": "poNumber"},
        {"tag": "releaseNumber"},
        {"tag": "date"}
      ]
    },
    {
      "code": "SE",
      "tag": "SE",
      "minOccurs": 1,
      "maxOccurs": -1,
      "fields": [
        {"tag": "code"},
        {"tag": "segmentCount"},
        {"tag": "controlNumber"}
      ]
    }
  ]
}
```

And the EDI content as below.

```bash
BEG*00*SA*PO-001*20260511~
SE*4*0001~
```

Then, from inside the new library directory, run the code generator against your schema.

```bash
bal edi codegen -i path/to/schema.json -o document.bal
```

This generates the followings in the output file:

- **Record types** -- matching each segment and composite in the schema.
- **`fromEdiString`** -- Convert an EDI string to a Ballerina record.
- **`toEdiString`** -- Convert a Ballerina record to an EDI string.
- **`getSchema`** -- Get the EDI schema as an `EdiSchema` object.
- **`fromEdiStringWithSchema`** -- Convert an EDI string to a Ballerina record using a pre-loaded schema.
- **`toEdiStringWithSchema`** -- Convert a Ballerina record to an EDI string using a pre-loaded schema.

## Parsing EDI documents

Once you have generated code from a schema, parse EDI text into typed Ballerina records.

<Tabs>
<TabItem value="ui" label="Visual Designer" default>

1. Open your main integration and click **+ Add Artifact** on the canvas.
2. Select **Automation** from the artifacts panel.

   ![Artifacts panel with Automation selected](/img/develop/transform/edi/automation.png)

3. In the flow designer, click **+** between the **Start** and **Error Handler** nodes. In the **Statement** panel on the right, click **Call Function**.

   ![Call Function in the Statement panel](/img/develop/transform/edi/call-function.png)

4. In the **Functions** panel, scroll to the **io** section and select **fileReadString**.

   ![Select fileReadString from io functions](/img/develop/transform/edi/file-read-string.png)

5. Set **Path** to the path of your EDI file (e.g., `document1.edi`) and **Result** to `ediContent`, then click **Save**.

   ![Configure fileReadString inputs](/img/develop/transform/edi/add-inputs-file-read-string.png)

6. Click **+** again and select **Call Function**. In the **Functions** panel under **Within Project > documents**, select **fromEdiString**.

   ![Select fromEdiString from the documents library](/img/develop/transform/edi/from-edi-string.png)

7. Set **Edi Text** to `ediContent` and **Result** to `documents`, then click **Save**.

   ![Configure fromEdiString inputs](/img/develop/transform/edi/populate-from-edi-string.png)

8. Click **+** again. In the right panel, expand the **Logging** section and select **Log Info**.

   ![Select Log Info from the Logging panel](/img/develop/transform/edi/log-info.png)

9. In the **Msg** field, select **Expression**, enter `documents.toString()`, and click **Save**.

</TabItem>
<TabItem value="code" label="Ballerina Code">

```ballerina
import ballerina/io;
import ballerina/log;

import <add-org-name>/documents;

public function main() returns error? {
    do {
        string ediContent = check io:fileReadString("path/to/document.edi");
        documents:Document documents = check documents:fromEdiString(string `${ediContent}`);
        log:printInfo(documents.toString());
    } on fail error e {
        log:printError("Error occurred", 'error = e);
        return e;
    }
}
```

</TabItem>
</Tabs>

## Generating EDI output

Build EDI documents from Ballerina records and serialize them to the standard text format.

<Tabs>
<TabItem value="ui" label="Visual Designer" default>

1. Click **+ Add Artifact** and select **Automation**.
2. In the flow designer, click **+** and select **Declare Variable**. Set **Name** to `document`, **Type** to `documents:Document`, and enter the following as the **Expression**, then click **Save**.

   ```json
    {
        "BEG":{
            "purposeCode":"BEG",
            "typeCode":"00",
            "poNumber":"SA",
            "releaseNumber":"PO-001",
            "date":"20260511"
        },
        "SE":[
            {
                "code":"SE",
                "segmentCount":"4",
                "controlNumber":"0001"
            }
        ]
    }
   ```

   ![Declare document variable](/img/develop/transform/edi/declare-document-variable.png)

3. Click **+** and select **Call Function**. Under the **documents** section, select **toEdiString**. Set **Data** to `document` and **Result** to `ediResult`, then click **Save**.

   ![Configure toEdiString inputs](/img/develop/transform/edi/populate-to-edi-string.png)

4. Click **+** and select **Log Info**. Set **Msg** to `ediResult`, then click **Save**.

   ![Configure Log Info with ediResult](/img/develop/transform/edi/log-print-info.png)

</TabItem>
<TabItem value="code" label="Ballerina Code">

```ballerina
import ballerina/log;

import <add-org-name>/documents;

public function main() returns error? {
    do {
        documents:Document document = {
            "BEG": {
                "purposeCode": "BEG",
                "typeCode": "00", 
                "poNumber": "SA", 
                "releaseNumber": "PO-001", 
                "date": "20260511"
            },
            "SE": [
                {
                    "code": "SE", 
                    "segmentCount": "4", 
                    "controlNumber": "0001"
                }
            ]
        };
        string ediResult = check documents:toEdiString(document);
        log:printInfo(string `${ediResult}`);
    } on fail error e {
        log:printError("Error occurred", 'error = e);
        return e;
    }
}
```

</TabItem>
</Tabs>

## EDI to JSON/XML conversion

A common integration pattern is converting EDI documents to JSON or XML for downstream systems.

<Tabs>
<TabItem value="ui" label="Visual Designer" default>

1. Click **+** and select **Declare Variable**. Set **Name** to `document`, **Type** to `documents:Document`, and enter the following as the **Expression**, then click **Save**.

   ```json
   {
       "BEG": {
           "purposeCode": "BEG",
           "typeCode": "00",
           "poNumber": "SA",
           "releaseNumber": "PO-001",
           "date": "20260511"
       },
       "SE": [
           {
               "code": "SE",
               "segmentCount": "4",
               "controlNumber": "0001"
           }
       ]
   }
   ```

   ![Declare document variable](/img/develop/transform/edi/declare-document-variable.png)

2. Click **+** and select **Declare Variable**. Set **Name** to `jsonDocument`, **Type** to `json`, and **Expression** to `document.toJson()`, then click **Save**.

   ![Declare JSON variable](/img/develop/transform/edi/declare-json-variable.png)

3. Click **+** and select **Call Function**. Under the `io` section, select **fileWriteJson**. Set **Path** to `document.json` and **Content** to `jsonDocument`, then click **Save**.

   ![Configure fileWriteJson inputs](/img/develop/transform/edi/populate-file-write-json.png)

</TabItem>
<TabItem value="code" label="Ballerina Code">

```ballerina
import ballerina/data.xmldata;
import ballerina/io;
import ballerina/log;

import <add-org-name>/documents;

public function main() returns error? {
    do {
        documents:Document document = {
            "BEG": {
                "purposeCode": "BEG",
                "typeCode": "00",
                "poNumber": "SA",
                "releaseNumber": "PO-001",
                "date": "20260511"
            },
            "SE": [
                {
                    "code": "SE",
                    "segmentCount": "4",
                    "controlNumber": "0001"
                }
            ]
        };
        json jsonDocument = document.toJson();
        check io:fileWriteJson("document.json", jsonDocument);
    } on fail error e {
        log:printError("Error occurred", 'error = e);
        return e;
    }
}
```

</TabItem>
</Tabs>

The `json` value produced by `toJson()` can also be converted to XML using the `ballerina/data.xmldata` module. Call `xmldata:toXml()` on the record directly and write the result with `io:fileWriteString`.

```ballerina
xml documentXml = check xmldata:toXml(document);
check io:fileWriteString("document.xml", documentXml.toString());
```

## Low-Level EDI processing

For dynamic or schema-less scenarios, use the `ballerina/edi` module directly to parse EDI into JSON.

<Tabs>
<TabItem value="ui" label="Visual Designer" default>

1. Click **+** and select **Call Function**. Under the `io` section, select **fileReadString**. Set **Path** to the path of your EDI file and **Result** to `ediText`, then click **Save**.
2. Click **+** and select **Call Function**. Under the `io` section, select **fileReadJson**. Set **Path** to the path of your schema file and **Result** to `schema`, then click **Save**.
3. Click **+** and select **Declare Variable**. Set **Name** to `ediSchema`, **Type** to `edi:EdiSchema`, and **Expression** to `check schema.fromJsonWithType()`, then click **Save**.
4. Click **+** and select **Call Function**. Under the `edi` section, select **fromEdiString**. Set **Edi Text** to `ediText`, **Schema** to `ediSchema`, and **Result** to `invoiceData`, then click **Save**.

</TabItem>
<TabItem value="code" label="Ballerina Code">

```ballerina
import ballerina/edi;
import ballerina/io;

public function main() returns error? {
    string ediText = check io:fileReadString("invoice.edi");

    // Parse EDI using a schema definition at runtime
    json schema = check io:fileReadJson("invoice_schema.json");
    edi:EdiSchema ediSchema = check schema.fromJsonWithType();

    json invoiceData = check edi:fromEdiString(ediText, ediSchema);
    io:println(invoiceData.toJsonString());
}
```

</TabItem>
</Tabs>

## Best practices

- **Generate code from schemas** rather than parsing EDI manually -- the generated records and functions handle segment delimiters, escape characters, and validation
- **Use packages for reuse** -- bundle frequently used EDI schemas into shared Ballerina packages
- **Validate early** -- parse EDI at the integration boundary to catch format errors before business logic executes
- **Convert to records immediately** -- work with typed records throughout your integration and serialize back to EDI only at the output boundary

## What's next

- [Type System & Records](type-system-records.md) -- Define EDI record types
