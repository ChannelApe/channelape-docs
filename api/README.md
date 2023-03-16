# Inventory Adjustments

⚠️ Caution: Our API supports two different inventory operations.

This section describes how to adjust inventory balances in ChannelApe using the `ADJUST` operation.  

The other is a `SET` operation which is used to complete overwrite ChannelApe's inventory balance.
If you're interested in learning more, check out about our [Inventory Management System](https://www.channelape.com/knowledge/channelape-inventory-management-system) knowledgebase article.

## Individual Adjustment

### Overview

Use this API to create one-off adjustments.

Additional documentation can be found at [docs.channelape.io](https://docs.channelape.io/#7fb11288-1d07-4aa9-97f4-b6a66429e404).

### Endpoint

Send a `POST` request to `https://api.channelape.com/v1/inventories/quantities/adjusts`

### Example cURL Request

```bash
curl --location -g 'https://api.channelape.com/v1/inventories/quantities/adjusts' \
--header 'Content-Type: application/json' \
--header 'x-channel-ape-authorization-token: your-api-key' \
--header 'x-channel-ape-idempotent-key: some-unique-id-in-your-system' \
--data '{
  "locationId": 309,
  "sku": "117_001_GGY_7",
  "inventoryStatus": "AVAILABLE_TO_SELL",
  "memo": "Reason: PI",
  "quantity": "-1"
}'
```

### Headers

| Header                            | Value                                                                                    |
| --------------------------------- | ---------------------------------------------------------------------------------------- |
| content-type                      | application/json                                                                         |
| x-channel-ape-authorization-token | Your API token                                                                           |
| x-channel-ape-idempotent-key      | A unique id for the adjustment that ChannelApe will use to filter duplicate adjustments. |

### Data Requirements

| Field           | Type       | Required | Description                                                                |
| --------------- | ---------- | -------- | -------------------------------------------------------------------------- |
| locationId      | string     | Yes      | The corresponding ChannelApe location where the adjustment should be made. |
| sku             | string     | Yes      | The SKU of the item you want to adjust.                                    |
| inventoryStatus | string     | Yes      | The status for the balance being adjusted.                                 |
| memo            | string     | Yes      | Put the reason why the adjustment was made.                                |
| quantity        | signed int | Yes      | The relative change in quantity (+/-).                                     |

### Inventory Statuses
| Status            |
| ----------------- |
| AVAILABLE_TO_SELL |
| COMMITTED         |
| ON_ORDER          |
| RESERVE           |
| ON_HOLD           |
| ON_HAND           |

## Batch Adjustment

### Overview

Use this API process related adjustments like a transfer.
ChannelApe will assign a unique ID to the batch upon submission which is used to identify the related adjustments.

Additional documentation can be found at [docs.channelape.io](https://docs.channelape.io/#fa5b8800-7847-4168-a68b-6430e4f8fe2f)

### Endpoint

Send a `POST` request to `https://api.channelape.com/v1/batches`

### Example cURL Request

```bash
curl --location -g 'https://api.channelape.com/v1/batches' \
--header 'Content-Type: application/json' \
--header 'x-channel-ape-authorization-token: your-api-key' \
--data '{
  "businessId": "938a69a8-9ef2-4faa-b485-dad486aba56e",
  "adjustments": [
    {
      "idempotentKey": "123456789",
      "inventoryItemId": "703",
      "quantity": 10,
      "operation": "ADJUST",
      "locationId": 51,
      "inventoryStatus": "ON_ORDER",
      "memo": "Reason the adjustment was made."
    },
    {
      "idempotentKey": "123456789",
      "sku": "ABC-123",
      "quantity": 12,
      "operation": "SET",
      "locationId": 51,
      "inventoryStatus": "COMMITTED",
      "memo": "Reason the adjustment was made."
    }
  ]
}'
```

### Headers

* Note: The `x-channel-ape-idempotent-key` header is not needed here as it is part of the adjustment in the request.

| Header                            | Value            |
| --------------------------------- | ---------------- |
| content-type                      | application/json |
| x-channel-ape-authorization-token | Your API token   |

### Data Requirements

| Field       | Type                     | Required | Description                                                       |
| ----------- | ------------------------ | -------- | ----------------------------------------------------------------- |
| businessId  | string                   | Yes      | The ID of the business where these adjustments should be applied. |
| adjustments | array of BatchAdjustment | Yes      | A list of adjustments to apply.                                   |

#### BatchAdjustment

| Field           | Type       | Required | Description                                                                              |
| --------------- | ---------- | -------- | ---------------------------------------------------------------------------------------- |
| idempotentKey   | string     | Yes      | A unique id for the adjustment that ChannelApe will use to filter duplicate adjustments. |
| locationId      | string     | Yes      | The corresponding ChannelApe location where the adjustment should be made.               |
| sku             | string     | Yes      | The SKU of the item you want to adjust.                                                  |
| quantity        | signed int | Yes      | The relative change in quantity (+/-).                                                   |
| operation       | string     | Yes      | Should always be "ADJUST" for adjustments.                                               |
| memo            | string     | Yes      | Put the reason why the adjustment was made.                                              |
| inventoryStatus | string     | Yes      | The status for the balance being adjusted.                                               |

### Inventory Statuses
| Status            |
| ----------------- |
| AVAILABLE_TO_SELL |
| COMMITTED         |
| ON_ORDER          |
| RESERVE           |
| ON_HOLD           |
| ON_HAND           |

# Confirm Shipment

## Overview

This Endpoint is used to confirm shipment of a Purchase Order or Stock Transfer Order.

### Use Cases

1. Confirming a Shipment for a Purchase Order
1. Confirming a Shipment for a Stock Transfer Order
1. Confirming a Shipment for a Sales Order

### Endpoint

An authenticated endpoint is provisioned by environment.

Because it is authenticated you must provide your API Secret Key in the `x-channel-ape-authorization-token` header along with your request.

### Request Properties

| Property     | Value            |
| ------------ | ---------------- |
| Protocol     | HTTP             |
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

### Example Request

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
        "locationId": "819",
        "shippingCompany": "FEDEX",
        "shippingMethod": "GROUND",
        "trackingNumber": "1234123412341234",
        "shippedAt": "2023-02-14T00:00:00.000Z",
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

# Create Stock Transfer Order

### Example Request

```json
{
  "businessId": "ec5fcd55-c275-4843-9641-d0fd44b9f173",
  "channelId": "eacc87ed-1e37-4883-bc53-935b032bf4b0",
  "channelOrderId": "R_2935999",
  "purchaseOrderNumber": "R_2935999",
  "status": "OPEN",
  "purchasedAt": "2023-02-13T15:45:11.361Z",
  "alphabeticCurrencyCode": "USD",
  "customer": {
    "additionalFields": [
      {
        "name": "anything",
        "value": "you add here will be passed through"
      }
    ],
    "shippingAddress": {
      "additionalFields": [],
      "address1": "181 Columbus Ave.",
      "city": "New York",
      "countryCode": "US",
      "name": "Rothy's 181 Columbus",
      "postalCode": "10023",
      "province": "New York",
      "provinceCode": "NY"
    }
  },
  "additionalFields": [
    {
      "name": "VendorPartyInternalID",
      "value": "1018"
    },
    {
      "name": "SenderPartyInternalID",
      "value": "123456789"
    },
    {
      "name": "RecipientPartyInternalID",
      "value": "1234567890123"
    },
    {
      "name": "ShippingDateTimePeriod",
      "value": "2023-02-13T19:00:00Z"
    },
    {
      "name": "ArrivalDateTimePeriod",
      "value": "2022-02-13T19:00:00Z"
    },
    {
      "name": "note_attributes_ship_from",
      "value": "819"
    },
    {
      "name": "shipping_lines_title",
      "value": "VENDOR"
    },
    {
      "name": "note_attribute_retail_order",
      "value": "true"
    }
  ],
  "lineItems": [
    {
      "additionalFields": [
        {
          "name": "DeliveryQuantityTypeCode",
          "value": "EA"
        },
        {
          "name": "HTSCode",
          "value": "4202.92.3131"
        },
        {
          "name": "CountryOfOrigin",
          "value": "CN"
        },
        {
          "name": "StockTransferOrderReferenceID",
          "value": "2936000"
        },
        {
          "name": "StockTransferOrderReferenceTypeCode",
          "value": "814"
        },
        {
          "name": "StockTransferOrderReferenceItemID",
          "value": "1"
        },
        {
          "name": "StockTransferOrderReferenceItemTypeCode",
          "value": "74"
        },
        {
          "name": "fulfillable_quantity",
          "value": "4"
        },
        {
          "name": "Z_PurchaseOrder_ODR",
          "value": "R_2935999-02"
        },
        {
          "name": "OutboundDeliveryExecution.ID",
          "value": "5194651"
        },
        {
          "name": "Item.ID",
          "value": "1"
        }
      ],
      "id": "1",
      "quantity": 4,
      "sku": "125_001_BBPLD_OS",
      "taxes": []
    },
    {
      "additionalFields": [
        {
          "name": "DeliveryQuantityTypeCode",
          "value": "PR"
        },
        {
          "name": "HTSCode",
          "value": "6404.19.3760"
        },
        {
          "name": "CountryOfOrigin",
          "value": "CN"
        },
        {
          "name": "StockTransferOrderReferenceID",
          "value": "2935999"
        },
        {
          "name": "StockTransferOrderReferenceTypeCode",
          "value": "814"
        },
        {
          "name": "StockTransferOrderReferenceItemID",
          "value": "2"
        },
        {
          "name": "StockTransferOrderReferenceItemTypeCode",
          "value": "74"
        },
        {
          "name": "fulfillable_quantity",
          "value": "4"
        },
        {
          "name": "Z_PurchaseOrder_ODR",
          "value": "R_2935999-01"
        },
        {
          "name": "OutboundDeliveryExecution.ID",
          "value": "5194550"
        },
        {
          "name": "Item.ID",
          "value": "2"
        }
      ],
      "id": "2",
      "quantity": 4,
      "sku": "117_001_GGY_7",
      "taxes": []
    }
  ]
}
```
