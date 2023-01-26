# Transfer Orders

A Transfer Order is a transaction that's created to move inventory from one of your locations to another.

Transfer Orders should be used when inventory has to be shipped and received.

## Warehouse to Retail Store

The scenarios below document the inventory adjustments ChannelApe expects at each stage of the transfer order's journey.

In our examples, we follow a Transfer Order that has been created to physically move 100 units from the Warehouse to the Retail Store.

### 1. No Partial Receipts, No Short Shipment, No Quality Control Adjustments, No Damages

The transfer order is shipped complete and a receipt is sent once completely received.

There are no adjustments to indicate progress of quality control procedures.

Inventory is received in good condition and deemed available to sell.

<details open>
<summary>Show Diagram</summary>

```mermaid
sequenceDiagram
    autonumber
    participant ERP
    actor ERP Integration
    participant ChannelApe
    participant Warehouse Location
    participant Warehouse
    actor Retail Integration
    participant Retail Location
    participant Retail Store

    ERP->>ERP: Transfer Order created

    ERP Integration->>ERP: Transfer Order retrieved
    ERP Integration->>ChannelApe: Fulfillment Order created

    ChannelApe->>Warehouse: Fulfillment Order sent
    note over Warehouse Location: Committed +100 units

    Warehouse->>Warehouse: Shipment created
    
    Warehouse->>ChannelApe: Fulfillment created
    note over Warehouse Location: Committed -100 units
    note over Warehouse Location: Available to Sell -100 units

    ERP Integration->>ChannelApe: Fulfillment retrieved
    ERP Integration->>ERP: Transfer Order updated
    ERP Integration->>ChannelApe: ASN created
    
    Retail Integration->>ChannelApe: ASN retrieved
    Retail Integration->>Retail Store: ASN created
    note over Retail Location: On Order +100 units

    Retail Store->>Retail Store: Receipt Started

    Retail Store->>Retail Store: Receipt created
    Retail Integration->>Retail Store: Receipt retrieved
    Retail Integration->>ChannelApe: Receipt created
    note over Retail Location: On Order -100 units
    note over Retail Location: Available to Sell +100 units

    Retail Store->>Retail Store: Receipt completed

    Retail Integration->>Retail Store: Retrieve ASN status change
    Retail Integration->>ChannelApe: ASN closed
    note over Retail Location: No adjustment

    ERP Integration->>ChannelApe: Receipt retrieved
    ERP Integration->>ERP: Transfer Order completed
```

</details>

### 2. Partial Receipts, No Short Shipment, No Quality Control Adjustments, No Damages

The transfer order is shipped complete and multiple receipts are created throughout the receiving process.

There are no adjustments to indicate progress of quality control procedures.

Inventory is received in good condition and deemed available to sell.

<details open>
<summary>Show Diagram</summary>

```mermaid
sequenceDiagram
    autonumber
    participant ERP
    actor ERP Integration
    participant ChannelApe
    participant Warehouse Location
    participant Warehouse
    actor Retail Integration
    participant Retail Location
    participant Retail Store

    ERP->>ERP: Transfer Order created

    ERP Integration->>ERP: Transfer Order retrieved
    ERP Integration->>ChannelApe: Fulfillment Order created

    ChannelApe->>Warehouse: Fulfillment Order sent
    note over Warehouse Location: Committed +100 units
    
    Warehouse->>Warehouse: Shipment created
    Warehouse->>ChannelApe: Fulfillment created
    note over Warehouse Location: Committed -100 units
    note over Warehouse Location: Available to Sell -100 units

    ERP Integration->>ChannelApe: Fulfillment retrieved
    ERP Integration->>ERP: Transfer Order updated
    ERP Integration->>ChannelApe: ASN created
    
    Retail Integration->>ChannelApe: ASN retrieved
    Retail Integration->>Retail Store: ASN created
    note over Retail Location: On Order +100 units

    Retail Store->>Retail Store: Receipt Started

    Retail Store->>Retail Store: Receipt created
    Retail Integration->>Retail Store: Receipt retrieved
    Retail Integration->>ChannelApe: Receipt created
    note over Retail Location: On Order -40 units
    note over Retail Location: Available to Sell +40 units

    Retail Store->>Retail Store: Receipt created
    Retail Integration->>Retail Store: Receipt retrieved
    Retail Integration->>ChannelApe: Receipt created
    note over Retail Location: On Order -60 units
    note over Retail Location: Available to Sell +60 units
    
    Retail Store->>Retail Store: Receipt completed

    Retail Integration->>Retail Store: Retrieve ASN status change
    Retail Integration->>ChannelApe: ASN closed
    note over Retail Location: No adjustment

    ERP Integration->>ChannelApe: Receipt retrieved

    ERP Integration->>ERP: Transfer Order completed
```

</details>

### 3. Partial Receipts, Short Shipment, No Quality Control Adjustments, No Damages

The transfer order is shipped 5 units short and multiple receipts are created throughout the receiving process.

There are no adjustments to indicate progress of quality control procedures.

Inventory is received in good condition and deemed available to sell.

<details open>
<summary>Show Diagram</summary>

```mermaid
sequenceDiagram
    autonumber
    participant ERP
    actor ERP Integration
    participant ChannelApe
    participant Warehouse Location
    participant Warehouse
    actor Retail Integration
    participant Retail Location
    participant Retail Damages Location
    participant Retail Store

    ERP->>ERP: Transfer Order created

    ERP Integration->>ERP: Transfer Order retrieved
    ERP Integration->>ChannelApe: Fulfillment Order created

    ChannelApe->>Warehouse: Fulfillment Order sent
    note over Warehouse Location: Committed +100 units
    
    Warehouse->>Warehouse: Shipment created
    Warehouse->>ChannelApe: Fulfillment created
    note over Warehouse Location: Committed -100 units
    note over Warehouse Location: Available to Sell -100 units

    ERP Integration->>ChannelApe: Fulfillment retrieved
    ERP Integration->>ERP: Transfer Order updated
    ERP Integration->>ChannelApe: ASN created
    
    Retail Integration->>ChannelApe: ASN retrieved
    Retail Integration->>Retail Store: ASN created
    note over Retail Location: On Order +100 units

    Retail Store->>Retail Store: Receipt Started

    Retail Store->>Retail Store: Receipt created
    Retail Integration->>Retail Store: Receipt retrieved
    Retail Integration->>ChannelApe: Receipt created
    note over Retail Location: On Order -50 units
    note over Retail Location: Available to Sell +50 units

    Retail Store->>Retail Store: Receipt created
    Retail Integration->>Retail Store: Receipt retrieved
    Retail Integration->>ChannelApe: Receipt created
    note over Retail Location: On Order -45 units
    note over Retail Location: Available to Sell +45 units

    Retail Store->>Retail Store: Receipt completed

    Retail Integration->>Retail Store: Retrieve ASN status change
    Retail Integration->>ChannelApe: ASN closed
    note over Retail Location: On Order -5 units

    ERP Integration->>ChannelApe: Receipt retrieved

    ERP Integration->>ERP: Transfer Order completed
```

</details>

### 4. Partial Receipts, Short Shipment, Quality Control Adjustments, No Damages

The transfer order is shipped 5 units short and multiple receipts are created throughout the receiving process.

10 units are selected for population sampling and inspected as part of the Quality Control process.

All 10 units pass inspection and are returned to Available stock.

<details open>
<summary>Show Diagram</summary>

```mermaid
sequenceDiagram
    autonumber
    participant ERP
    actor ERP Integration
    participant ChannelApe
    participant Warehouse Location
    participant Warehouse
    actor Retail Integration
    participant Retail Location
    participant Retail Damages Location
    participant Retail Store

    ERP->>ERP: Transfer Order created

    ERP Integration->>ERP: Transfer Order retrieved
    ERP Integration->>ChannelApe: Fulfillment Order created

    ChannelApe->>Warehouse: Fulfillment Order sent
    note over Warehouse Location: Committed +100 units
    
    Warehouse->>Warehouse: Shipment created
    Warehouse->>ChannelApe: Fulfillment created
    note over Warehouse Location: Committed -100 units
    note over Warehouse Location: Available to Sell -100 units

    ERP Integration->>ChannelApe: Fulfillment retrieved
    ERP Integration->>ERP: Transfer Order updated
    ERP Integration->>ChannelApe: ASN created
    
    Retail Integration->>ChannelApe: ASN retrieved
    Retail Integration->>Retail Store: ASN created
    note over Retail Location: On Order +100 units

    Retail Store->>Retail Store: Receipt Started

    Retail Store->>Retail Store: Receipt created
    Retail Integration->>Retail Store: Receipt retrieved
    Retail Integration->>ChannelApe: Receipt created
    note over Retail Location: On Order -50 units
    note over Retail Location: Available to Sell +50 units

    Retail Store->>Retail Store: Inspection starts

    Retail Store->>ChannelApe: Inventory Adjustment
    note over Retail Location: Available to Sell -10 units
    note over Retail Location: On Hold +10 units

    Retail Store->>Retail Store: Receipt created
    Retail Integration->>Retail Store: Receipt retrieved
    Retail Integration->>ChannelApe: Receipt created
    note over Retail Location: On Order -45 units
    note over Retail Location: Available to Sell +45 units

    Retail Store->>Retail Store: Receipt completed

    Retail Integration->>Retail Store: Retrieve ASN status change
    Retail Integration->>ChannelApe: ASN closed
    note over Retail Location: On Order -5 units

    ERP Integration->>ChannelApe: Receipt retrieved

    ERP Integration->>ERP: Transfer Order completed

    Retail Store->>Retail Store: Inspection completes

    Retail Store->>ChannelApe: Inventory Adjustment
    note over Retail Location: On Hold -10 units
    note over Retail Location: Available to Sell +10 units
```

</details>

### 5. Partial Receipts, Short Shipment, Quality Control Adjustments, Damages

The transfer order is shipped 5 units short and multiple receipts are created throughout the receiving process.

10 units are selected for population sampling and inspected as part of the Quality Control process.

6 units pass inspection and are returned to Available stock.

4 units fail inspection and are transferred to the Damages location.

<details open>
<summary>Show Diagram</summary>

```mermaid
sequenceDiagram
    autonumber
    participant ERP
    actor ERP Integration
    participant ChannelApe
    participant Warehouse Location
    participant Warehouse
    actor Retail Integration
    participant Retail Location
    participant Retail Damages Location
    participant Retail Store

    ERP->>ERP: Transfer Order created

    ERP Integration->>ERP: Transfer Order retrieved
    ERP Integration->>ChannelApe: Fulfillment Order created

    ChannelApe->>Warehouse: Fulfillment Order sent
    note over Warehouse Location: Committed +100 units
    
    Warehouse->>Warehouse: Shipment created
    Warehouse->>ChannelApe: Fulfillment created
    note over Warehouse Location: Committed -100 units
    note over Warehouse Location: Available to Sell -100 units

    ERP Integration->>ChannelApe: Fulfillment retrieved
    ERP Integration->>ERP: Transfer Order updated
    ERP Integration->>ChannelApe: ASN created
    
    Retail Integration->>ChannelApe: ASN retrieved
    Retail Integration->>Retail Store: ASN created
    note over Retail Location: On Order +100 units

    Retail Store->>Retail Store: Receipt Started

    Retail Store->>Retail Store: Receipt created
    Retail Integration->>Retail Store: Receipt retrieved
    Retail Integration->>ChannelApe: Receipt created
    note over Retail Location: On Order -50 units
    note over Retail Location: Available to Sell +50 units

    Retail Store->>Retail Store: Inspection starts

    Retail Store->>ChannelApe: Inventory Adjustment
    note over Retail Location: Available to Sell -10 units
    note over Retail Location: On Hold +10 units

    Retail Store->>Retail Store: Receipt created
    Retail Integration->>Retail Store: Receipt retrieved
    Retail Integration->>ChannelApe: Receipt created
    note over Retail Location: On Order -45 units
    note over Retail Location: Available to Sell +45 units

    Retail Integration->>Retail Store: Retrieve ASN status change
    Retail Integration->>ChannelApe: ASN closed
    note over Retail Location: On Order -5 units

    ERP Integration->>ChannelApe: Receipt retrieved

    ERP Integration->>ERP: Transfer Order completed
    
    Retail Store->>Retail Store: Inspection completes

    Retail Store->>ChannelApe: Inventory Adjustment
    note over Retail Location: On Hold -10 units
    note over Retail Location: Available to Sell +6 units

    Retail Store->>ChannelApe: Inventory Adjustment
    note over Retail Damages Location: Available to Sell +4 units
```

</details>

## Damages to Warehouse

### 6. Some of the damages are refurbished

In this scenario 4 units are transferred from the retail store to the warehouse for refurbishment.

3 units are refurbished and returned to Available stock.

1 unit remains in damages.

<details open>
<summary>Show Diagram</summary>

```mermaid
sequenceDiagram
    autonumber
    participant ERP
    actor ERP Integration
    participant ChannelApe
    participant Warehouse Damages Location
    participant Warehouse Location
    participant Warehouse
    actor Retail Integration
    participant Retail Location
    participant Retail Damages Location
    participant Retail Store

    ERP->>ERP: Transfer Order created

    ERP Integration->>ERP: Transfer Order retrieved
    ERP Integration->>ChannelApe: Fulfillment Order created

    ChannelApe->>Retail Store: Fulfillment Order sent
    note over Retail Damages Location: Committed +4 units
    
    Retail Store->>Retail Store: Shipment created

    Retail Store->>ChannelApe: Fulfillment created
    note over Retail Damages Location: Committed -4 units
    note over Retail Damages Location: Available to Sell -4 units
    
    ERP Integration->>ChannelApe: Fulfillment retrieved
    ERP Integration->>ERP: Transfer Order updated
    ERP Integration->>ChannelApe: ASN created

    ChannelApe->>Warehouse: ASN sent
    note over Warehouse Damages Location: On Order +4 units

    Warehouse->>Warehouse: Receipt started

    Warehouse->>ChannelApe: Receipt Created
    note over Warehouse Damages Location: On Order -4 units
    note over Warehouse Damages Location: Available to Sell +4 units

    Warehouse->>Warehouse: Receipt completed

    Warehouse->>ChannelApe: ASN closed

    ERP Integration->>ChannelApe: Receipt retrieved

    ERP Integration->>ERP: Transfer Order completed

    Warehouse->>Warehouse: Refurbishment starts

    Warehouse->>Warehouse: Refurbishment completes

    Warehouse->>ChannelApe: Inventory Adjustment
    note over Warehouse Damages Location: Available to Sell -3 units

    Warehouse->>ChannelApe: Inventory Adjustment
    note over Warehouse Location: Available to Sell +3 units
```

</details>