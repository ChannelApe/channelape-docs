### Create Warehouse Receipt

Implies the Receipt Confirmation is present in and started this process from MFT.

```mermaid
sequenceDiagram
    autonumber
    participant ChannelApe
    participant MFT
    participant Boomi

    MFT->>ChannelApe: GET /shipments?shipment_id={NRIReceiptConfirmation.ClientReferenceId1}
    MFT->>MFT: Create Boomi Payload
    MFT->>MFT: Sign Boomi Payload
    MFT->>Boomi: POST https://intqa.rothys.com/.../createASNReceipt
```

### Create Store Receipt

Implies the Receipt Confirmation is present in and started this process from MFT.

```mermaid
sequenceDiagram
    autonumber
    participant ChannelApe
    participant MFT
    participant Boomi

    MFT->>ChannelApe: GET /shipments?shipment_id={HermesReceipt.additionalFields.warehouse_order_id}
    MFT->>MFT: Create Boomi Payload
    MFT->>MFT: Sign Boomi Payload
    MFT->>Boomi: POST https://intqa.rothys.com/.../createASNReceipt
```