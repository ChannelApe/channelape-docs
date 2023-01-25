# Online Orders
## Shopify Order Fulfillment
```mermaid
sequenceDiagram
    participant Shopify
    participant Virtual Location
    participant ChannelApe
    participant Warehouse Location
    participant Warehouse

    Shopify->>ChannelApe: Order Created
    note over Virtual Location: Committed +2 units

    ChannelApe->>Warehouse: Fulfillment Order Created
    note over Warehouse Location: Committed +2 units
    
    Warehouse->>ChannelApe: Shipment Created
    note over Warehouse Location: Committed -2 units
    note over Warehouse Location: Available to Sell -2 units
    
    ChannelApe->>Shopify: Fulfillment Created
    note over Virtual Location: Committed -2 units
    note over Virtual Location: Available to Sell -2 units
```