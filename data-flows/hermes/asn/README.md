## Sending Advanced Ship Notice to Hermes

```mermaid
sequenceDiagram
    autonumber
    participant ChannelApe
    participant MFT
    participant Hermes

    ChannelApe->>MFT: ASN Webhook
    MFT->>Hermes: POST /webhooks/channel-ape/inbounds 
```

### Process Hermes ASN Receipts

```mermaid
sequenceDiagram
    autonumber
    participant ChannelApe
    participant MFT
    participant Hermes

    Hermes->>MFT: Create ASN Receipt in /stock-transfer-orders/receipts/
    MFT->>ChannelApe: POST /upsert_shipment_receipts_full*
```