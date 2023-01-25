# Transfer Orders
## Warehouse to Retail Store initiated by ERP
### No Partial Receipts, No Short Shipment, No Quality Control, No Damages

In this scenario 100 units are transferred from the warehouse to retail store.

```mermaid
sequenceDiagram
    autonumber
    participant ERP
    participant ChannelApe
    participant Warehouse Location
    participant Warehouse
    participant Retail Location
    participant Retail Store

    ERP->>ERP: Transfer Order created
    ERP->>ChannelApe: Fulfillment Order created

    ChannelApe->>Warehouse: Fulfillment Order sent
    note over Warehouse Location: Committed +100 units

    Warehouse->>Warehouse: Shipment created
    
    Warehouse->>ChannelApe: Fulfillment created
    note over Warehouse Location: Committed -100 units
    note over Warehouse Location: Available to Sell -100 units

    ERP->>ChannelApe: Fulfillment retrieved
    ERP->>ERP: Transfer Order updated
    ERP->>ChannelApe: ASN created
    
    ChannelApe->>Retail Store: ASN sent
    note over Retail Location: On Order +100 units

    Retail Store->>Retail Store: Receipt Started

    Retail Store->>ChannelApe: Receipt created
    note over Retail Location: On Order -100 units
    note over Retail Location: Available to Sell +100 units

    Retail Store->>Retail Store: Receipt completed

    Retail Store->>ChannelApe: ASN closed
    note over Retail Location: No adjustment

    ERP->>ChannelApe: Receipt retrieved

    ERP->>ERP: Transfer Order completed
```

### Partial Receipts, No Short Shipment, No Quality Control, No Damages
In this scenario 100 units are transferred from the warehouse to retail store.

Partial receipts are created as the inventory is received.

```mermaid
sequenceDiagram
    autonumber
    participant ERP
    participant ChannelApe
    participant Warehouse Location
    participant Warehouse
    participant Retail Location
    participant Retail Store

    ERP->>ERP: Transfer Order created
    ERP->>ChannelApe: Fulfillment Order created

    ChannelApe->>Warehouse: Fulfillment Order sent
    note over Warehouse Location: Committed +100 units
    
    Warehouse->>Warehouse: Shipment created
    Warehouse->>ChannelApe: Fulfillment created
    note over Warehouse Location: Committed -100 units
    note over Warehouse Location: Available to Sell -100 units

    ERP->>ChannelApe: Fulfillment retrieved
    ERP->>ERP: Transfer Order updated
    ERP->>ChannelApe: ASN created
    
    ChannelApe->>Retail Store: ASN sent
    note over Retail Location: On Order +100 units

    Retail Store->>Retail Store: Receipt Started

    Retail Store->>ChannelApe: Receipt created
    note over Retail Location: On Order -40 units
    note over Retail Location: Available to Sell +40 units

    Retail Store->>ChannelApe: Receipt created
    note over Retail Location: On Order -60 units
    note over Retail Location: Available to Sell +60 units
    
    Retail Store->>Retail Store: Receipt completed

    Retail Store->>ChannelApe: ASN closed
    note over Retail Location: No adjustment

    ERP->>ChannelApe: Receipt retrieved

    ERP->>ERP: Transfer Order completed
```

### Partial Receipts, Short Shipment, No Quality Control, No Damages
In this scenario 100 units are transferred from the warehouse to retail store.

Partial receipts are created as the inventory is received.

The delivery was short 5 units.

```mermaid
sequenceDiagram
    autonumber
    participant ERP
    participant ChannelApe
    participant Warehouse Location
    participant Warehouse
    participant Retail Location
    participant Retail Damages Location
    participant Retail Store

    ERP->>ERP: Transfer Order created
    ERP->>ChannelApe: Fulfillment Order created

    ChannelApe->>Warehouse: Fulfillment Order sent
    note over Warehouse Location: Committed +100 units
    
    Warehouse->>Warehouse: Shipment created
    Warehouse->>ChannelApe: Fulfillment created
    note over Warehouse Location: Committed -100 units
    note over Warehouse Location: Available to Sell -100 units

    ERP->>ChannelApe: Fulfillment retrieved
    ERP->>ERP: Transfer Order updated
    ERP->>ChannelApe: ASN created
    
    ChannelApe->>Retail Store: ASN sent
    note over Retail Location: On Order +100 units

    Retail Store->>Retail Store: Receipt Started

    Retail Store->>ChannelApe: Receipt created
    note over Retail Location: On Order -50 units
    note over Retail Location: Available to Sell +50 units

    Retail Store->>ChannelApe: Receipt created
    note over Retail Location: On Order -45 units
    note over Retail Location: Available to Sell +45 units

    Retail Store->>Retail Store: Receipt completed

    Retail Store->>ChannelApe: ASN closed
    note over Retail Location: On Order -5 units

    ERP->>ChannelApe: Receipt retrieved

    ERP->>ERP: Transfer Order completed
```

### Partial Receipts, Short Shipment, Quality Control, No Damages
In this scenario 100 units are transferred from the warehouse to retail store.

Partial receipts are created as the inventory is received.

The delivery was short 5 units.

A sample size of 10 units are inspected as part of Quality Control.

10 units pass inspection and are returned to Available stock.

```mermaid
sequenceDiagram
    autonumber
    participant ERP
    participant ChannelApe
    participant Warehouse Location
    participant Warehouse
    participant Retail Location
    participant Retail Damages Location
    participant Retail Store

    ERP->>ERP: Transfer Order created
    ERP->>ChannelApe: Fulfillment Order created

    ChannelApe->>Warehouse: Fulfillment Order sent
    note over Warehouse Location: Committed +100 units
    
    Warehouse->>Warehouse: Shipment created
    Warehouse->>ChannelApe: Fulfillment created
    note over Warehouse Location: Committed -100 units
    note over Warehouse Location: Available to Sell -100 units

    ERP->>ChannelApe: Fulfillment retrieved
    ERP->>ERP: Transfer Order updated
    ERP->>ChannelApe: ASN created
    
    ChannelApe->>Retail Store: ASN sent
    note over Retail Location: On Order +100 units

    Retail Store->>Retail Store: Receipt Started

    Retail Store->>ChannelApe: Receipt created
    note over Retail Location: On Order -50 units
    note over Retail Location: Available to Sell +50 units

    Retail Store->>Retail Store: Inspection starts

    Retail Store->>ChannelApe: Inventory Adjustment
    note over Retail Location: Available to Sell -10 units
    note over Retail Location: On Hold +10 units

    Retail Store->>ChannelApe: Receipt created
    note over Retail Location: On Order -45 units
    note over Retail Location: Available to Sell +45 units

    Retail Store->>Retail Store: Receipt completed

    Retail Store->>ChannelApe: ASN closed
    note over Retail Location: On Order -5 units

    ERP->>ChannelApe: Receipt retrieved

    ERP->>ERP: Transfer Order completed

    Retail Store->>Retail Store: Inspection completes

    Retail Store->>ChannelApe: Inventory Adjustment
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
    autonumber
    participant ERP
    participant ChannelApe
    participant Warehouse Location
    participant Warehouse
    participant Retail Location
    participant Retail Damages Location
    participant Retail Store

    ERP->>ERP: Transfer Order created
    ERP->>ChannelApe: Fulfillment Order created

    ChannelApe->>Warehouse: Fulfillment Order sent
    note over Warehouse Location: Committed +100 units
    
    Warehouse->>Warehouse: Shipment created
    Warehouse->>ChannelApe: Fulfillment created
    note over Warehouse Location: Committed -100 units
    note over Warehouse Location: Available to Sell -100 units

    ERP->>ChannelApe: Fulfillment retrieved
    ERP->>ERP: Transfer Order updated
    ERP->>ChannelApe: ASN created
    
    ChannelApe->>Retail Store: ASN sent
    note over Retail Location: On Order +100 units

    Retail Store->>Retail Store: Receipt Started

    Retail Store->>ChannelApe: Receipt created
    note over Retail Location: On Order -50 units
    note over Retail Location: Available to Sell +50 units

    Retail Store->>ChannelApe: Inventory Adjustment - Inspection Started
    note over Retail Location: Available to Sell -10 units
    note over Retail Location: On Hold +10 units

    Retail Store->>ChannelApe: Receipt created
    note over Retail Location: On Order -45 units
    note over Retail Location: Available to Sell +45 units

    Retail Store->>ChannelApe: ASN Closed
    note over Retail Location: On Order -5 units

    ERP->>ChannelApe: Receipt retrieved

    ERP->>ERP: Transfer Order completed
    
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
    autonumber
    participant ERP
    participant ChannelApe
    participant Warehouse Damages Location
    participant Warehouse Location
    participant Warehouse
    participant Retail Location
    participant Retail Damages Location
    participant Retail Store

    ERP->>ERP: Transfer Order created
    ERP->>ChannelApe: Fulfillment Order created

    ChannelApe->>Retail Store: Fulfillment Order sent
    note over Retail Damages Location: Committed +4 units
    
    Retail Store->>Retail Store: Shipment created

    Retail Store->>ChannelApe: Fulfillment created
    note over Retail Damages Location: Committed -4 units
    
    ERP->>ChannelApe: Fulfillment retrieved
    ERP->>ERP: Transfer Order updated
    ERP->>ChannelApe: ASN created

    ChannelApe->>Warehouse: ASN sent
    note over Warehouse Damages Location: On Order +4 units

    Warehouse->>Warehouse: Receipt started

    Warehouse->>ChannelApe: Receipt Created
    note over Warehouse Damages Location: On Order -4 units
    note over Warehouse Damages Location: On Hold +4 units

    Warehouse->>Warehouse: Receipt completed

    Warehouse->>ChannelApe: ASN closed

    ERP->>ChannelApe: Receipt retrieved

    ERP->>ERP: Transfer Order completed

    Warehouse->>Warehouse: Refurbishment starts

    Warehouse->>Warehouse: Refurbishment completes

    Warehouse->>ChannelApe: Inventory Adjustment
    note over Warehouse Damages Location: On Hold -3 units

    Warehouse->>ChannelApe: Inventory Adjustment
    note over Warehouse Location: Available to Sell +3 units
```
