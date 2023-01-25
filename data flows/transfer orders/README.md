# Transfer Orders {#transfer-orders}
## Warehouse to Retail Store initiated by ERP {#transfer-orders-w2r}
### No Partial Receipts, No Short Shipment, No Quality Control, No Damages

In this scenario 100 units are transferred from the warehouse to retail store.

```mermaid
sequenceDiagram
    participant ERP
    participant ChannelApe
    participant Warehouse Location
    participant Warehouse
    participant Retail Location
    participant Retail Store

    ERP->>ChannelApe: Transfer Order Created

    ChannelApe->>Warehouse: Transfer Order Created
    note over Warehouse Location: Committed +100 units
    
    Warehouse->>ChannelApe: Shipment Created
    note over Warehouse Location: Committed -100 units
    note over Warehouse Location: Available to Sell -100 units
    
    ChannelApe->>Retail Store: ASN Created
    note over Retail Location: On Order +100 units

    Retail Store->>ChannelApe: Receipt Created
    note over Retail Location: On Order -100 units
    note over Retail Location: Available to Sell +100 units

    Retail Store->>ChannelApe: ASN Closed
    note over Retail Location: No adjustment

    ChannelApe->>ERP: Transfer Order Completed
```

### Partial Receipts, No Short Shipment, No Quality Control, No Damages
In this scenario 100 units are transferred from the warehouse to retail store.

Partial receipts are created as the inventory is received.

```mermaid
sequenceDiagram
    participant ERP
    participant ChannelApe
    participant Warehouse Location
    participant Warehouse
    participant Retail Location
    participant Retail Store

    ERP->>ChannelApe: Transfer Order Created

    ChannelApe->>Warehouse: Transfer Order Created
    note over Warehouse Location: Committed +100 units
    
    Warehouse->>ChannelApe: Shipment Created
    note over Warehouse Location: Committed -100 units
    note over Warehouse Location: Available to Sell -100 units
    
    ChannelApe->>Retail Store: ASN Created
    note over Retail Location: On Order +100 units

    Retail Store->>ChannelApe: Receipt Created
    note over Retail Location: On Order -40 units
    note over Retail Location: Available to Sell +40 units

    Retail Store->>ChannelApe: Receipt Created
    note over Retail Location: On Order -60 units
    note over Retail Location: Available to Sell +60 units

    Retail Store->>ChannelApe: ASN Closed
    note over Retail Location: No adjustment

    ChannelApe->>ERP: Transfer Order Completed
```

### Partial Receipts, Short Shipment, No Quality Control, No Damages
In this scenario 100 units are transferred from the warehouse to retail store.

Partial receipts are created as the inventory is received.

The delivery was short 5 units.

```mermaid
sequenceDiagram
    participant ERP
    participant ChannelApe
    participant Warehouse Location
    participant Warehouse
    participant Retail Location
    participant Retail Damages Location
    participant Retail Store

    ERP->>ChannelApe: Transfer Order Created

    ChannelApe->>Warehouse: Transfer Order Created
    note over Warehouse Location: Committed +100 units
    
    Warehouse->>ChannelApe: Shipment Created
    note over Warehouse Location: Committed -100 units
    note over Warehouse Location: Available to Sell -100 units
    
    ChannelApe->>Retail Store: ASN Created
    note over Retail Location: On Order +100 units

    Retail Store->>ChannelApe: Receipt Created
    note over Retail Location: On Order -50 units
    note over Retail Location: Available to Sell +50 units

    Retail Store->>ChannelApe: Receipt Created
    note over Retail Location: On Order -45 units
    note over Retail Location: Available to Sell +45 units

    Retail Store->>ChannelApe: ASN Closed
    note over Retail Location: On Order -5 units

    ChannelApe->>ERP: Transfer Order Completed
```

### Partial Receipts, Short Shipment, Quality Control, No Damages
In this scenario 100 units are transferred from the warehouse to retail store.

Partial receipts are created as the inventory is received.

The delivery was short 5 units.

A sample size of 10 units are inspected as part of Quality Control.

10 units pass inspection and are returned to Available stock.

```mermaid
sequenceDiagram
    participant ERP
    participant ChannelApe
    participant Warehouse Location
    participant Warehouse
    participant Retail Location
    participant Retail Damages Location
    participant Retail Store

    ERP->>ChannelApe: Transfer Order Created

    ChannelApe->>Warehouse: Transfer Order Created
    note over Warehouse Location: Committed +100 units
    
    Warehouse->>ChannelApe: Shipment Created
    note over Warehouse Location: Committed -100 units
    note over Warehouse Location: Available to Sell -100 units
    
    ChannelApe->>Retail Store: ASN Created
    note over Retail Location: On Order +100 units

    Retail Store->>ChannelApe: Receipt Created
    note over Retail Location: On Order -50 units
    note over Retail Location: Available to Sell +50 units

    Retail Store->>ChannelApe: Inventory Adjustment - Inspection Started
    note over Retail Location: Available to Sell -10 units
    note over Retail Location: On Hold +10 units

    Retail Store->>ChannelApe: Receipt Created
    note over Retail Location: On Order -45 units
    note over Retail Location: Available to Sell +45 units

    Retail Store->>ChannelApe: ASN Closed
    note over Retail Location: On Order -5 units

    ChannelApe->>ERP: Transfer Order Completed

    Retail Store->>ChannelApe: Inventory Adjustment - Inspection Complete
    note over Retail Location: On Hold -10 units
    note over Retail Location: Available to Sell +10 units
```

### Partial Receipts, Short Shipment, Quality Control, Damages

In this scenario 100 units are transferred from the warehouse to retail store.

Partial receipts are created as the inventory is received.

The delivery was short 5 units.

A sample size of 10 units are inspected as part of Quality Control.

6 units pass inspection and are returned to Available stock.

4 units fail inspection and are transferred to damages.

```mermaid
sequenceDiagram
    participant ERP
    participant ChannelApe
    participant Warehouse Location
    participant Warehouse
    participant Retail Location
    participant Retail Damages Location
    participant Retail Store

    ERP->>ChannelApe: Transfer Order Created

    ChannelApe->>Warehouse: Transfer Order Created
    note over Warehouse Location: Committed +100 units
    
    Warehouse->>ChannelApe: Shipment Created
    note over Warehouse Location: Committed -100 units
    note over Warehouse Location: Available to Sell -100 units
    
    ChannelApe->>Retail Store: ASN Created
    note over Retail Location: On Order +100 units

    Retail Store->>ChannelApe: Receipt Created
    note over Retail Location: On Order -50 units
    note over Retail Location: Available to Sell +50 units

    Retail Store->>ChannelApe: Inventory Adjustment - Inspection Started
    note over Retail Location: Available to Sell -10 units
    note over Retail Location: On Hold +10 units

    Retail Store->>ChannelApe: Receipt Created
    note over Retail Location: On Order -45 units
    note over Retail Location: Available to Sell +45 units

    Retail Store->>ChannelApe: ASN Closed
    note over Retail Location: On Order -5 units

    ChannelApe->>ERP: Transfer Order Completed
    
    Retail Store->>ChannelApe: Inventory Adjustment - Inspection Complete
    note over Retail Location: On Hold -10 units
    note over Retail Location: Available to Sell +6 units

    Retail Store->>ChannelApe: Inventory Adjustment - Damaged
    note over Retail Damages Location: On Hold +4 units
```

## Damages to Warehouse initiated by ERP
### Some of the damages are refurbished

In this scenario 4 units are transferred from the retail store to the warehouse for refurbishment.

3 units are refurbished and returned to Available stock.

1 unit remains in damages.

```mermaid
sequenceDiagram
    participant ERP
    participant ChannelApe
    participant Warehouse Damages Location
    participant Warehouse Location
    participant Warehouse
    participant Retail Location
    participant Retail Damages Location
    participant Retail Store

    ERP->>ChannelApe: Transfer Order Created

    ChannelApe->>Retail Store: Transfer Order Created
    note over Retail Damages Location: Committed +4 units
    
    Retail Store->>ChannelApe: Shipment Created
    note over Retail Damages Location: Committed -4 units
    
    ChannelApe->>Warehouse: ASN Created
    note over Warehouse Damages Location: On Order +4 units

    Warehouse->>ChannelApe: Receipt Created
    note over Warehouse Damages Location: On Order -4 units
    note over Warehouse Damages Location: On Hold +4 units

    Warehouse->>ChannelApe: ASN Closed

    ChannelApe->>ERP: Transfer Order Completed
    
    Warehouse->>ChannelApe: Inventory Adjustment - Refurbishment Complete
    note over Warehouse Damages Location: On Hold -3 units

    Warehouse->>ChannelApe: Inventory Adjustment - Refurbished
    note over Warehouse Location: Available to Sell +3 units
```