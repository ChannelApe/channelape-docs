# Inventory Adjustments API

View our [Inventory Adjustments API documentation](inventory-adjustments/README.md).

# Create Stock Transfer Order

## Overview

This API is used to create a Stock Transfer Order.

ChannelApe uses Stock Transfer Orders to request transfer of goods from one location, the shipping party, to another, the receiving party.

ChannelApe will send notification to the shipping party using our [Stock Transfer Order webhook](../webhooks/README.md).

### Use Cases

1. Warehouse to warehouse stock transfer
1. Warehouse to store stock transfer
1. Store to store stock transfer
1. Store to warehouse stock transfer

### Request Properties

| Field                  | Type                 | Required | Description                                                                                                                                                                                  |
| ---------------------- | -------------------- | -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| businessId             | string               | Yes      | Business ID is unique to each ChannelApe customer environment.                                                                                                                               |
| channelId              | string               | Yes      | The ID of the channel the order belongs to. Channels are unique and created as-needed.                                                                                                       |
| channelOrderId         | string               | Yes      | A unique ID assigned to the order from an external system. Has a unique constraint enforced across all channels on a business. We recommend prefixing the internal ID to prevent collisions. |
| purchaseOrderNumber    | string               | Yes      | The order number that would be printed on a shipping label; uniqueness is not enforced.                                                                                                      |
| status                 | string               | Yes      | Should always be "OPEN" for Stock Transfer Orders.                                                                                                                                           |
| purchasedAt            | string               | Yes      | Date when the order was placed / purchased in ISO-8601 timestamp                                                                                                                             |
| alphabeticCurrencyCode | string               | Yes      | 3-character currency code used for currency conversions. Default to "USD" if unsure.                                                                                                         |
| customer               | object               | Yes      | shippingAddress is required. This address is used for rate arbitrage and printed the shipping label. The address is validated by NRI with failures resulting in the order being held.        |
| fulfillments           | array of Fulfillment | Yes      |                                                                                                                                                                                              |

#### Customer

| Name            | Type    | Required | Description                         |
| --------------- | ------- | -------- | ----------------------------------- |
| shippingAddress | Address | Yes      | Destination to receive the transfer |

#### Address

| Name         | Type   | Required | Description                                                |
| ------------ | ------ | -------- | ---------------------------------------------------------- |
| company      | string | Yes      | Company name associated with the destination address       |
| name         | string | Yes      | Full name associated with the destination address          |
| address1     | string | Yes      | First line of the destination address                      |
| address2     | string | No       | Second line of the destination address                     |
| city         | string | Yes      | City of the destination address                            |
| provinceCode | string | Yes      | ISO 3166-2 subdivision code of the destination address     |
| countryCode  | string | Yes      | ISO 3166-1 alpha-2 country code of the destination address |
| postalCode   | string | Yes      | Postal or ZIP code of the destination address              |
| phone        | string | No       | Phone number associated with the destination address       |
| firstName    | string | No       | First name associated with the destination address         |
| lastName     | string | No       | Last name associated with the destination address          |

#### Order Additional Fields

| Field (Name)                     | Type   | Required | Description                                                                                           |
| -------------------------------- | ------ | -------- | ----------------------------------------------------------------------------------------------------- |
| deliver_by_date                  | string | Yes      | ISO-8601 timestamp used in rate arbitrage to find a cost-effective delivery service                   |
| note_attributes_ship_from        | string | Yes      | The ChannelApe ID for the location shipping the goods                                                 |
| note_attributes_ship_to          | string | Yes      | The ChannelApe ID for the location that will receive the goods                                        |
| shipping_lines_title             | string | Yes      | Always "VENDOR"                                                                                       |
| sap_source_location              | string | Yes      | The SAP ID for the location shipping the goods                                                        |
| sap_destination_location         | string | Yes      | The SAP ID for the location that will receive the goods                                               |
| sap_source_storage_location      | string | Yes      | The SAP ID for the storage location where goods will be shipped from                                  |
| sap_destination_storage_location | string | Yes      | The SAP ID for the destination storage location                                                       |
| note_attribute_retail_order      | string | Yes      | Set this to "true" when shipping from NRI; when set, NRI follows shipping procedure for retail orders |

#### Line Items

Notes

- A line item cannot have 0 for the quantity so if an item has not shipped do not include it in the request.

| Field            | Type                     | Required | Description                                                                                       |
| ---------------- | ------------------------ | -------- | ------------------------------------------------------------------------------------------------- |
| id               | string                   | Yes      | ID of the line item from the system of origin; is needed in return messages like STO Fulfillment. |
| sku              | string                   | Yes      | Stock Keeping Unit used by the shipping party to identify the item that should be shipped.        |
| quantity         | integer                  | Yes      | Quantity to ship                                                                                  |
| additionalFields | array of AdditionalField | Yes      | Additional information about the line item.                                                       |

#### Line Item Additional Fields

| Field (Name)        | Type   | Required | Description                                                      |
| ------------------- | ------ | -------- | ---------------------------------------------------------------- |
| sap_unit_of_measure | string | Yes      | "PR" or "EA"; is needed in return messages like STO Fulfillment. |

### Example Request

| Method | Content-Type     | Endpoint                             |
| ------ | ---------------- | ------------------------------------ |
| POST   | application/json | https://api.channelape.com/v1/orders |

```json
{
  "businessId": "ec5fcd55-c275-4843-9641-d0fd44b9f173",
  "channelId": "eacc87ed-1e37-4883-bc53-935b032bf4b0",
  "channelOrderId": "STO4600000039",
  "purchaseOrderNumber": "4600000039",
  "status": "OPEN",
  "purchasedAt": "2023-04-01T00:00:00.000Z",
  "alphabeticCurrencyCode": "USD",
  "customer": {
    "shippingAddress": {
      "address1": "181 Columbus Ave.",
      "city": "New York",
      "countryCode": "US",
      "name": "Rothy's 181 Columbus",
      "postalCode": "10023",
      "provinceCode": "NY"
    }
  },
  "additionalFields": [
    {
      "name": "deliver_by_date",
      "value": "2023-04-08T00:00:00.000Z"
    },
    {
      "name": "note_attributes_ship_from",
      "value": "819"
    },
    {
      "name": "note_attributes_ship_to",
      "value": "969"
    },
    {
      "name": "shipping_lines_title",
      "value": "VENDOR"
    },
    {
      "name": "sap_source_location",
      "value": "1111"
    },
    {
      "name": "sap_destination_location",
      "value": "2222"
    },
    {
      "name": "sap_source_storage_location",
      "value": "1111"
    },
    {
      "name": "sap_destination_storage_location",
      "value": "2222"
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
          "name": "sap_unit_of_measure",
          "value": "PR"
        }
      ],
      "id": "000011",
      "quantity": 100,
      "sku": "002_110_BLK_5"
    },
    {
      "additionalFields": [
        {
          "name": "sap_unit_of_measure",
          "value": "PR"
        }
      ],
      "id": "000012",
      "quantity": 100,
      "sku": "002_110_BLK_6"
    }
  ]
}
```

# Confirm Shipment

## Overview

The Confirm Shipment API is used to confirm shipment for a Purchase Order or Stock Transfer Order you have fulfilled.

When shipment is confirmed, ChannelApe sends a Ship Confirmation notification to the shipping party using [webhooks](../webhooks/README.md).

### Use Cases

1. Confirming a Shipment for a Purchase Order
1. Confirming a Shipment for a Stock Transfer Order
1. Confirming a Shipment for a Sales Order

### Endpoint

This is an authenticated endpoint so your request must include your API Secret in the `x-channel-ape-authorization-token` header.

### Limitations

- Minimum of 1 fulfillment
- Maximum of 5000 fulfillments for entire Order

### Request Properties

| Field        | Type                 | Required | Description |
| ------------ | -------------------- | -------- | ----------- |
| fulfillments | array of Fulfillment | Yes      |             |

### Fulfillment

| Field            | Type                     | Required | Description                                                                                                                |
| ---------------- | ------------------------ | -------- | -------------------------------------------------------------------------------------------------------------------------- |
| id               | string                   | Yes      | Unique number used to identify the shipment the shipping party's system                                                    |
| locationId       | integer                  | Yes      | The ChannelApe ID for the location that shipped the order                                                                  |
| shippedAt        | DateTime                 | Yes      | ISO-8601 formatted time of shipment                                                                                        |
| status           | string                   | Yes      | Should always be "OPEN" for new shipments.                                                                                 |
| locationId       | string                   | Yes      | The corresponding ChannelApe location where the shipment originated used for inventory tracking                            |
| lineItems        | array of LineItem        | Yes      | Line items shipped in the shipment                                                                                         |
| trackingNumber   | string                   | Yes      | Tracking Number                                                                                                            |
| additionalFields | array of AdditionalField | Yes      | Used to capture and pass through other data requirements as needed. See requirements below. Can also be used for metadata. |
| trackingUrl      | string                   | No       | A URL that can be used to track the shipment if available.                                                                 |
| shippingCompany  | string                   | No       | Carrier used for delivery                                                                                                  |
| shippingMethod   | string                   | No       | Service used for delivery                                                                                                  |

#### Fulfillment Additional Fields

| Field (Name)         | Type   | Required | Description                                                                                                                                            |
| -------------------- | ------ | -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------ |
| warehouse_order_id   | string | Yes      | This is the warehouse_order_id that was included on the initial Fulfillment Order from ChannelApe                                                      |
| CartonNumber         | int    | Yes      | Used to identify a specific carton in a multi-carton shipment, similar to how a Line Number is used to identify a specific line on a multi-line order. |
| TotalOrderCartons    | int    | Yes      | Total count of all the cartons shipped for the same order.                                                                                             |
| LicensePlate         | string | No       | Number printed in plaintext on the carton label.                                                                                                       |
| Sscc                 | string | No       | Barcode on the carton label that is scanned as it's received into storage.                                                                             |
| EstimatedArrivalDate | string | No       | Date the shipment is expected to arrive as estimated at the time of shipment.                                                                          |

#### Fulfillment Line Items

Notes

- A line item cannot have 0 for the quantity so if an item has not shipped do not include it in the request.

| Field            | Type                     | Required | Description                                                                               |
| ---------------- | ------------------------ | -------- | ----------------------------------------------------------------------------------------- |
| id               | string                   | No       | ID of the line item as it appeared in the STO request.                                    |
| sku              | string                   | Yes      | Stock Keeping Unit used to identify the item shipped if the line item ID is not provided. |
| quantity         | integer                  | Yes      | Quantity shipped                                                                          |
| additionalFields | array of AdditionalField | No       | Additional information about the line item.                                               |

### Example Request

| Method | Content-Type     | Endpoint                                                                                         |
| ------ | ---------------- | ------------------------------------------------------------------------------------------------ |
| POST   | application/json | https://edi-300.channelapeservices.com/web/connector/stock-transfer-orders/shipments/webhook.rsb |

```json
{
  "fulfillments": [
    {
      "id": "208924061",
      "locationId": "819",
      "shippingCompany": "FEDEX",
      "shippingMethod": "GROUND",
      "trackingNumber": "123456789012345",
      "shippedAt": "2023-04-01T15:35:00.000Z",
      "status": "OPEN",
      "additionalFields": [
        {
          "name": "warehouse_order_id",
          "value": "4600000039"
        },
        {
          "name": "CartonNumber",
          "value": "1"
        },
        {
          "name": "TotalOrderCartons",
          "value": "5"
        },
        {
          "name": "LicensePlate",
          "value": "0386149425"
        },
        {
          "name": "Sscc",
          "value": "00001922492001525086"
        },
        {
          "name": "EstimatedArrivalDate",
          "value": "2023-04-08T00:00:00.000Z"
        }
      ],
      "lineItems": [
        {
          "quantity": 100,
          "sku": "002_110_BLK_5"
        },
        {
          "quantity": 100,
          "sku": "002_110_BLK_6"
        }
      ]
    }
  ]
}
```

# Send Advanced Ship Notice

| Method | Endpoint                                                                                                    |
| ------ | ----------------------------------------------------------------------------------------------------------- |
| POST   | https://edi-300.channelapeservices.com/web/connector/stock-transfer-orders/advanced-ship-notice/webhook.rsb |

### Example Request

```json
{
  "purchaseOrderNumber": "4600000039",
  "customer": {
    "shippingAddress": {
      "address1": "181 Columbus Ave.",
      "city": "New York",
      "countryCode": "US",
      "name": "Rothy's 181 Columbus",
      "postalCode": "10023",
      "provinceCode": "NY"
    }
  },
  "fulfillments": [
    {
      "id": "208924061",
      "status": "SHIPPED",
      "shippingCompany": "FEDEX",
      "shippingMethod": "GROUND",
      "trackingNumber": "123456789012345",
      "additionalFields": [
        {
          "name": "warehouse_order_id",
          "value": "4600000039"
        },
        {
          "name": "CartonNumber",
          "value": "1"
        },
        {
          "name": "TotalOrderCartons",
          "value": "5"
        },
        {
          "name": "LicensePlate",
          "value": "0386149425"
        },
        {
          "name": "Sscc",
          "value": "00001922492001525086"
        },
        {
          "name": "EstimatedArrivalDate",
          "value": "2023-04-08T00:00:00.000Z"
        },
        {
          "name": "sap_purchase_order_number",
          "value": "4600000039"
        },
        {
          "name": "sap_asn_number",
          "value": "0180000011"
        },
        {
          "name": "sap_handling_unit_number",
          "value": "4600000039001"
        },
        {
          "name": "country_of_origin",
          "value": "HK"
        }
      ],
      "lineItems": [
        {
          "additionalFields": [
            {
              "name": "sap_unit_of_measure",
              "value": "PR"
            },
            {
              "name": "sap_asn_line_number",
              "value": "000010"
            }
          ],
          "id": "000011",
          "quantity": 100,
          "sku": "002_110_BLK_5"
        },
        {
          "additionalFields": [
            {
              "name": "sap_unit_of_measure",
              "value": "PR"
            },
            {
              "name": "sap_asn_line_number",
              "value": "000020"
            }
          ],
          "id": "000012",
          "quantity": 100,
          "sku": "002_110_BLK_6"
        }
      ]
    }
  ]
}
```
