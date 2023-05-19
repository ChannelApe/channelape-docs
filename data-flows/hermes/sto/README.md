### Send STO to Hermes

```mermaid
sequenceDiagram
    autonumber
    participant ChannelApe
    participant MFT
    participant Hermes

    ChannelApe->>MFT: Webhook - Send Orders: Fulfillment Order
    MFT->>Hermes: POST /webhooks/channel-ape/outbounds
```

### Process Hermes Shipments

```mermaid
sequenceDiagram
    autonumber
    participant ChannelApe
    participant MFT
    participant Hermes

    Hermes->>MFT: Create Ship Confirmation in /stock-transfer-orders/shipments/
    MFT->>ChannelApe: POST to Webhook - Process Shipments
```