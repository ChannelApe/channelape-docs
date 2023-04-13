# NRI

An overview of the interfaces in scope for Project Olympus and Project Hermes with NRI.

## Sending Advanced Ship Notice to NRI

ChannelApe will send the ASN Webhook to our MFT instance where the following will occur:
1. Choose the NRI API
1. Mapping to NRI's spec

```mermaid
sequenceDiagram
    autonumber
    participant ChannelApe
    participant MFT
    participant NRI

    ChannelApe->>MFT: ASN Webhook
    MFT-->>MFT: Does ASN have Carton Details?

    alt Yes
    MFT->>NRI: POST /api/v1/receiptpreview/receipts
    note left of NRI: Receipt Preview
    else No
    MFT->>NRI: POST /api/v1/purchaseorderpreview/purchaseorders
    note left of NRI: Purchase Order Preview
    end
```

### ASN Carton Details Test

An ASN has Carton Details if a value is found in the `SSCC` field in the payload.

```mermaid
flowchart LR
    start[/ASN Webhook Payload/] --> inspection[[Inspect Payload]]
    inspection --> decision{Is there a value for SSCC?}
    decision --> |No| nripo([NRI Purchase Order Preview])
    decision --> |Yes| nrir([NRI Receipt Preview])
```

## Receipt Confirmation

NRI publishes Receipt Confirmations to their API where they will remain available to API consumers until acknowledged.

ChannelApe will poll this API every 60 minutes to ingest and acknowledge new Receipt Confirmations.

```mermaid
sequenceDiagram
    autonumber
    participant ChannelApe
    participant MFT
    participant NRI

    loop 60 minutes
    MFT->>NRI: GET /api/v1/receiptconfirmation/confirmations
    NRI-->>MFT:Receipt Confirmations

    MFT-->>MFT: Received Carton has SSCC?
    alt Yes
        MFT->>ChannelApe: Update inventory
    end

    MFT->>ChannelApe: Create Receipt

    MFT->>NRI: POST /api/v1/receiptconfirmation/confirmations
    note right of MFT: Acknowledge Receipt Confirmation 
    end
```

### SSCC in Received Cartons Test

The Received Carton has an SSCC if the value in `ReceiptCartons[n].Sscc` of the ReceiptConfirmation is not blank.

### Inventory
Inventory will become immediately available upon ingestion if the Cartons Received have an SSCC.
This can't be done for Cartons without an SSCC because only Shipments with Carton Level Details communicated using the Receipt Preview API are instantly received in NRI's system, and the SSCC is the indicator.

ChannelApe cannot rely on dates either because NRI only records the Receipt Completion Date, not the start date and the Arrival Date does not indicate start of the receiving process.
This is done to avoid overinflating inventory.