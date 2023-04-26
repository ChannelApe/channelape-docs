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