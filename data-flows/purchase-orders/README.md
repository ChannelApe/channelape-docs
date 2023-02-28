# Purchase Orders
Purchase orders are used by brands to acquire more inventory from a supplier or manufacturer.

A Purchase Order typically includes:
- Header details
    - Vendor (Manufacturer / Supplier) name
    - Order number
    - Payment terms
- Line item details
    - SKU, quantity, and price
    - Ship-by date
    - Ship-to destination
    - Shipping terms

By using purchase orders, brands can also track their spending and manage their inventory.

## Shipments
Goods are shipped from their origin as soon as shipping terms can be satisfied and it's not uncommong for a single Purchase Order to be fulfilled over multiple shipments.

Each shipment typically includes:
- Header details
    - Purchase Order Number
    - A unique Shipment Number
    - Tracking numbers
    - Destination
    - Estimated arrival date
- Packaging details
    - Container hierarchy
    - Unique numbers for each container
- Line item details
    - Container reference
    - SKU and quantity

## Example
3,000 pairs of shoes are needed by November 1, 2023 according to the production schedule created by the Inventory Planning & Merchandising teams at ABC Co., a footwear & apparel brand.

Facts

- They know a 20 ft. container fits roughtly 3,500 shoes.
- They also know it takes about 2-4 weeks to transport, a delivery of this size will take about 1 week to receive and stock at the Manufacturer, and an error rate of 3%.

Steps

1. ABC Co. coordinates a Purchase Order for 3,100 units from their manufacturer in China and a ship-by date of September 15, 2023, with Flexport, their freight forwarding broker.
1. ABC Co. is given Flexport's invoice ID to track delivery progress.
1. The manufacturer loads 10 pallets into the container with each pallet containing ~310 pairs of shoes and the shipment departs for Port of Los Angeles.
1. ABC Co. gives their Manufacturer advanced notice of this shipment and its expected arrival date to ensure delivery goes smoothly.

### Opportunities
1. What if this Purchase Order contained a hundred line items and instead of 2 large POs a year, it was actually 20?
1. What if delivery is delayed?
1. How does ABC Co. know how many pairs of shoes a 20 ft. container can fit?
1. Or that it takes 2-4 weeks to transport and another week to put away at the Manufacturer?
1. How do they know their shrink rate is 3%?
1. How did they create their production schedule in the first place?

### Other Use Cases
#### Dropshipping
In some circumstances brands can arrange for the Purchase Order to be delivered directly to their customer, otherwise known as dropshipping.
#### SDQ Order
SDQ stands for Ship Destination Quantity.  An SDQ order is a Purchase Order that contains items that have varying ship-by dates and/or varying destinations.

## Purchase Order Sequence Diagram

```mermaid
sequenceDiagram
    autonumber
    participant Manufacturer
    actor User
    participant ChannelApe
    participant Warehouse Location
    participant Warehouse

    User->>Manufacturer: Create PO
    User-->>ChannelApe: Create PO
    Manufacturer->>Manufacturer: Create Shipment
    User->>Manufacturer: Get Shipment
    User-->>ChannelApe: Create Shipment
    ChannelApe->>Warehouse: Create Shipment
    note over Warehouse Location: +100 On Order
    Manufacturer-->>Warehouse: Shipment is Delivered
    Warehouse->>Warehouse: Receipt started
    ChannelApe->>Warehouse: Get Receipt
    note over Warehouse Location: -100 On Order
    note over Warehouse Location: +100 Available to Sell
    Warehouse->>Warehouse: Receipt finished
    User->>ChannelApe: Close PO
```