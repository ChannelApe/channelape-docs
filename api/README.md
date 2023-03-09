# Inventory Adjustments

⚠️ Caution: Inventory can be changed using an adjust or set oepration.  
Make sure you're using the correct operation.

Documentation for both of these operations is publicly available.

- [Inventory Adjustment](https://docs.channelape.io/#7fb11288-1d07-4aa9-97f4-b6a66429e404)
- [Inventory Set](https://docs.channelape.io/#e5638e1d-4f0c-49a8-a4ad-9291d2889e9f)

If you're interested in learning more, check out about our [Inventory Management System](https://www.channelape.com/knowledge/channelape-inventory-management-system) knowledgebase article.

### Data Requirements

#### Adjustment

| Field           | Type       | Required | Description                                                                |
| --------------- | ---------- | -------- | -------------------------------------------------------------------------- |
| locationId      | string     | Yes      | The corresponding ChannelApe location where the adjustment should be made. |
| sku             | string     | Yes      | The SKU of the item you want to adjust.                                    |
| inventoryStatus | string     | Yes      | The status for the balance being adjusted.                                 |
| quantity        | signed int | Yes      | The relative change in quantity (+/-).                                     |

#### Inventory Statuses

| Status            |
| ----------------- |
| AVAILABLE_TO_SELL |
| COMMITTED         |
| ON_ORDER          |
| RESERVE           |
| ON_HOLD           |
| ON_HAND           |

### Example Payload

```json
{
  "locationId": 10,
  "sku": "Some-New-Sku",
  "inventoryStatus": "ON_HOLD",
  "quantity": "15",
  "effectiveAt": "2019-10-25T19:07:49.088Z"
}
```

# Confirm Shipment

## Overview

This Endpoint is used to confirm shipment of a Purchase Order or Fulfillment Order.

### Use Cases

1. Confirming a Shipment for a Purchase Order
1. Confirming a Shipment for a Stock Transfer Order
1. Confirming a Shipment for a Sales Order

### Endpoint

An authenticated endpoint is provisioned by environment.

Because it is authenticated you must provide your API Secret Key in the `x-channel-ape-authorization-token` header along with your request.

### Request

| Property     | Value            |
| ------------ | ---------------- |
| Method       | POST             |
| Content-Type | application/json |

#### Structure

```
orderUpdate
    - fulfillments
        - additionalFields
        - lineItems
            - additionalFields
```

### Limitations

- Minimum of 1 fulfillment
- Maximum of 5000 fulfillments for entire Order

### Data Requirements

#### Request Root

| Field       | Type                | Required | Description                                                                               |
| ----------- | ------------------- | -------- | ----------------------------------------------------------------------------------------- |
| orderUpdate | list of OrderUpdate | Yes      | Used to update multiple orders with shipment details. At least 1 orderUpdate is required. |

#### OrderUpdate

| Field               | Type                 | Required | Description |
| ------------------- | -------------------- | -------- | ----------- |
| id                  | string               | No       |             |
| purchaseOrderNumber | string               | No       |             |
| channelOrderId      | string               | No       |             |
| fulfillments        | array of Fulfillment | Yes      |             |

#### Fulfillment

| Field            | Type                     | Required | Description                                                                                                                |
| ---------------- | ------------------------ | -------- | -------------------------------------------------------------------------------------------------------------------------- |
| shippedAt        | DateTime                 | Yes      | ISO-8601 formatted time of shipment                                                                                        |
| status           | string                   | Yes      | Will always be "OPEN"                                                                                                      |
| locationId       | string                   | Yes      | The corresponding ChannelApe location where the shipment originated used for inventory tracking                            |
| lineItems        | array of LineItem        | Yes      | Line items shipped in the shipment                                                                                         |
| additionalFields | array of AdditionalField | Yes\*    | Used to capture and pass through other data requirements as needed. See requirements below. Can also be used for metadata. |
| trackingNumber   | string                   | No\*     | Tracking Number - Only required if shipping with SSCC-18                                                                   |
| id               | string                   | No\*     | Only required if unable to provide additionalFields.warehouse_order_id                                                     |
| shippingCompany  | string                   | No\*     | Carrier used for shipment - Only required to track inbound deliveries                                                      |
| shippingMethod   | string                   | No\*     | Delivery service used - Only required to track inbound deliveries                                                          |

##### Required Additional Fields

| Field                               | Type               | Required | Description                                                                                       |
| ----------------------------------- | ------------------ | -------- | ------------------------------------------------------------------------------------------------- |
| additionalFields.warehouse_order_id | Warehouse Order ID | Yes      | This is the warehouse_order_id that was included on the initial Fulfillment Order from ChannelApe |
| additionalFields.shipment_id        | Shipment ID        | Yes      | Unique identifier for the shipment taken from your system                                         |

#### LineItem

Notes

- A line item cannot have 0 for the quantity so if an item has not shipped do not include it in the request.

| Field            | Type                     | Required | Description                                 |
| ---------------- | ------------------------ | -------- | ------------------------------------------- |
| sku              | string                   | Yes      | SKU                                         |
| quantity         | integer                  | Yes      | Quantity shipped                            |
| additionalFields | array of AdditionalField | No       | Additional information about the line item. |

#### AdditionalField

| Field | Type   | Required | Description                   |
| ----- | ------ | -------- | ----------------------------- |
| name  | string | Yes      | Name of the additional field  |
| value | string | No       | Value of the additional field |

#### Optional Additional Fields (Example)

| Name                     | Description                                                           | Required |
| ------------------------ | --------------------------------------------------------------------- | -------- |
| Sscc                     | SSCC code - Only required to enable automate receipts with warehouse. | No       |
| ShipmentNumber           | Shipment Number - Only required if using SSCC-18                      | No\*     |
| CartonNumber             | Carton Number - Only required if using SSCC-18                        | No\*     |
| Length                   | Carton Length                                                         | No       |
| Width                    | Carton Width                                                          | No       |
| Height                   | Carton Height                                                         | No       |
| Weight                   | Total Order Weight                                                    | No       |
| TotalOrderWeight         | Total Order Weight                                                    | No       |
| TotalOrderCartons        | Total Order Cartons                                                   | No       |
| ClientTotalFreightCharge | Total Freight Charge (amount)                                         | No       |
| TotalOrderCubeVolume     | Total Order Cube Volume                                               | No       |

### Example Request Payload

```json
{
  "orderUpdate": {
    "fulfillments": [
      {
        "id": "208924061",
        "additionalFields": [
          {
            "name": "shipment_id",
            "value": "208924061"
          },
          {
            "name": "warehouse_order_id",
            "value": "R_2935999"
          },
          {
            "name": "Sscc",
            "value": "00001922492001525086"
          },
          {
            "name": "ShipmentNumber",
            "value": "20892406"
          },
          {
            "name": "CartonNumber",
            "value": "1"
          },
          {
            "name": "Length",
            "value": "24"
          },
          {
            "name": "Width",
            "value": "14"
          },
          {
            "name": "Height",
            "value": "14"
          },
          {
            "name": "Weight",
            "value": "18.38"
          },
          {
            "name": "TotalOrderWeight",
            "value": "18.38"
          },
          {
            "name": "TotalOrderCartons",
            "value": "1"
          },
          {
            "name": "ClientTotalFreightCharge",
            "value": "140.00"
          },
          {
            "name": "TotalOrderCubeVolume",
            "value": "2.54"
          }
        ],
        "locationId": "309",
        "shippingCompany": "FEDEX",
        "shippingMethod": "GROUND",
        "trackingNumber": "1234123412341234",
        "shippedAt": "2023-03-01T17:30:00Z",
        "status": "OPEN",
        "lineItems": [
          {
            "quantity": 4,
            "sku": "125_001_BBPLD_OS"
          },
          {
            "quantity": 4,
            "sku": "117_001_GGY_7"
          }
        ]
      }
    ]
  }
}
```
