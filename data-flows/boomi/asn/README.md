### Create ASN

```mermaid
sequenceDiagram
    autonumber
    participant ChannelApe
    participant MFT
    participant Boomi

    Boomi->>MFT: Create ASN in /stock-transfer-orders/advanced-ship-notice/
    MFT->>MFT: Map to ChannelApe Shipment
    MFT->>ChannelApe: POST /upsert_shipments_full
```