# Webhooks

## Overview

This document describes the webhooks available in ChannelApe and their usage.

## Receiving Webhooks

ChannelApe will send the webhook to the HTTPs endpoint provided by your application.

### Responding to Webhooks

Your endpoint should return a successful response code to acknowledge its receipt.

Status codes >= 200 and <= 399 are treated as a successful delivery and will not be retried.

#### Retry Policy

For all other response codes or a failure to respond in general, ChannelApe will retry webhooks[^1] every `15 minutes` until successful delivery or a user intervenes.

[^1]: ChannelApe will always include the most up-to-date copy of the object and as such, the contents of the payload may change between attempts.

### Authentication and Security

Although ChannelApe does not support authenticated endpoints, a key and secret can be configured for each endpoint.

The key will be included as a query parameter in the request and its secret for the value.

The secret is never displayed to end users.

## Webhooks

1. Stock Transfer Order
1. Advanced Ship Notice
1. Receipt Confirmation

### Stock Transfer Order

ChannelApe's Order Management System sends this to the party responsible for fulfilling the Stock Transfer Order.

#### Timing

Stock Transfer Orders are evaluated in batches by ChannelApe's Order Management System every `15 minutes`.

### Advanced Ship Notice

This webhook is intended for the party that will receive the shipment when an Advanced Ship Notice is created.

#### Timing

It is sent upon creation of an Advanced Ship Notice.

### Receipt Confirmation

This webhook is intended for the party that created the Advanced Ship Notice to both confirm and describe the goods received.

#### Timing

It is sent upon the creation of a Receipt Confirmation.

Multiple partial receipts will result in multiple Receipt Confirmations.

#### Order Level

| Name                | Type                    | Description                                                              |
| ------------------- | ----------------------- | ------------------------------------------------------------------------ |
| additionalFields    | dict of AdditionalField | Additional fields are normalized into an object with the name as the key |
| customer            | Customer                | Object containing customer information                                   |
| lineItems           | array of LineItem       | Line Items on the order                                                  |
| locationId          | string                  | ChannelApe Location ID of the location associated with the request       |
| purchasedAt         | Date                    | Date and time the order was purchased                                    |
| purchaseOrderNumber | string                  | Unique identifier for the purchase order                                 |
| shippingCompany     | string                  | Name of the shipping company used for the order                          |
| shippingMethod      | string                  | Shipping method used for the order                                       |
| status              | string                  | Current status of the order                                              |
| warehouseOrderId    | string                  | Unique identifier for the warehouse order                                |
| warehouseId         | string                  | ID of the warehouse associated with the order                            |
| warehouseCode       | string                  | Code for the warehouse associated with the order                         |
| warehouseName       | string                  | Name of the warehouse associated with the order                          |
| warehouseCompany    | string                  | Company associated with the warehouse for the order                      |
| warehouseZipcode    | string                  | Zipcode of the warehouse associated with the order                       |

#### Order Level Additional Fields

| Name                      | Source | When         | Value                              | Usage                                                                     |
| ------------------------- | ------ | ------------ | ---------------------------------- | ------------------------------------------------------------------------- |
| note_attributes_ship_to   | S4     | STO Creation | A ChannelApe Location ID           | Tells ChannelApe which inventory location will ship the goods.            |
| WERKS                     | S4     | STO Creation | SAPs's Store / DC Destination      | Mapped for Boomi's convenience.                                           |
| LGORT                     | S4     | STO Creation | SAP's Destination Storage Location | Mapped for Boomi's convenience.                                           |
| note_attributes_ship_from | S4     | STO Creation | A ChannelApe Location ID           | Tells ChannelApe which inventory location will receive the shipped goods. |
| LIFNR                     | S4     | STO Creation | Supplier Number                    | Mapped for Boomi's convenience.                                           |
| Sscc                      | NRI    | STO Shipment | GS1-128 Number                     | Will be barcoded and printed on carton labels from manfucaturer           |
| ShipmentNumber            | NRI    | STO Shipment | Number                             | Used to uniquely identify a shipment from NRI.                            |
| LicensePlate              | NRI    | STO Shipment | Alphanumeric with hyphens          | Currently barcoded and printed on retail carton labels.                   |
| CartonNumber              | NRI    | STO Shipment | Incremental integer                | The carton number in a multi-carton shipment. e.g. 1, 2, 3, 4...          |

#### Customer

| Name             | Type                     | Description                                               |
| ---------------- | ------------------------ | --------------------------------------------------------- |
| additionalFields | array of AdditionalField | An array of additional fields for the customer if present |
| name             | string                   | Name of the customer                                      |
| firstName        | string                   | First name of the customer                                |
| lastName         | string                   | Last name of the customer                                 |
| email            | string                   | Email address of the customer                             |
| phone            | string                   | Phone number of the customer                              |
| billingAddress   | Address                  | Billing address of the customer                           |
| shippingAddress  | Address                  | Shipping address of the customer                          |

#### Address

| Name             | Type                     | Description                                    |
| ---------------- | ------------------------ | ---------------------------------------------- |
| company          | string                   | Company name associated with the address       |
| name             | string                   | Full name associated with the address          |
| phone            | string                   | Phone number associated with the address       |
| firstName        | string                   | First name associated with the address         |
| lastName         | string                   | Last name associated with the address          |
| address1         | string                   | First line of the address                      |
| address2         | string                   | Second line of the address                     |
| city             | string                   | City of the address                            |
| province         | string                   | Subdivision or state of the address            |
| provinceCode     | string                   | ISO 3166-2 subdivision code of the address     |
| countryCode      | string                   | ISO 3166-1 alpha-2 country code of the address |
| country          | string                   | Country of the address                         |
| postalCode       | string                   | Postal or ZIP code of the address              |
| additionalFields | array of AdditionalField | Additional fields for the address              |

#### LineItem

| Name             | Type                    | Description                                                               |
| ---------------- | ----------------------- | ------------------------------------------------------------------------- |
| id               | string                  | A unique identifier for the lineItem                                      |
| sku              | string                  | The SKU of the item.                                                      |
| upc              | string                  | The UPC (universal product code) of the item.                             |
| title            | string                  | The title of the item.                                                    |
| quantity         | number                  | The number of units of the item being purchased.                          |
| reserveQuantity  | number                  | The number of units of the item reserved for future orders.               |
| presaleQuantity  | number                  | The number of units of the item that are being sold as part of a presale. |
| vendor           | string                  | The vendor or manufacturer of the item.                                   |
| grams            | number                  | The weight of the item in grams.                                          |
| shippingMethod   | string                  | The method of shipping for the line item.                                 |
| restockType      | string                  | If this was a return                                                      |
| giftCardCode     | string                  | The code for any gift card applied to the line item.                      |
| giftCardId       | string                  | The unique identifier for any gift card applied to the line item.         |
| price            | number                  | The price of a single unit of the item.                                   |
| shippingPrice    | number                  | The price of shipping for the line item.                                  |
| shippingTax      | number                  | The tax on shipping for the line item.                                    |
| taxes            | array of Tax            | Any applicable taxes for the line item.                                   |
| additionalFields | dict of AdditionalField | Additional fields are normalized into an object with the name as the key  |

#### Item Level Additional Fields

| Name                                    | Source     | Usage                                                                                                    |
| --------------------------------------- | ---------- | -------------------------------------------------------------------------------------------------------- |
| DeliveryQuantityTypeCode                | SAP ByD    | Provided by SAP ByD. Only mapped so that ChannelApe can send asynchronous communucation back to SAP ByD. |
| HTSCode                                 | SAP ByD    | Provided by SAP ByD. Only mapped so that ChannelApe can send asynchronous communucation back to SAP ByD. |
| CountryOfOrigin                         | SAP ByD    | Provided by SAP ByD. Only mapped so that ChannelApe can send asynchronous communucation back to SAP ByD. |
| StockTransferOrderReferenceID           | SAP ByD    | Provided by SAP ByD. Only mapped so that ChannelApe can send asynchronous communucation back to SAP ByD. |
| StockTransferOrderReferenceTypeCode     | SAP ByD    | Provided by SAP ByD. Only mapped so that ChannelApe can send asynchronous communucation back to SAP ByD. |
| StockTransferOrderReferenceItemID       | SAP ByD    | Provided by SAP ByD. Only mapped so that ChannelApe can send asynchronous communucation back to SAP ByD. |
| StockTransferOrderReferenceItemTypeCode | SAP ByD    | Provided by SAP ByD. Only mapped so that ChannelApe can send asynchronous communucation back to SAP ByD. |
| fulfillable_quantity                    | ChannelApe | Legacy field. Not used.                                                                                  |
| Z_PurchaseOrder_ODR                     | SAP ByD    | Provided by SAP ByD. Only mapped so that ChannelApe can send asynchronous communucation back to SAP ByD. |
| OutboundDeliveryExecution.ID            | SAP ByD    | Provided by SAP ByD. Only mapped so that ChannelApe can send asynchronous communucation back to SAP ByD. |
| Item.ID                                 | SAP ByD    | Provided by SAP ByD. Only mapped so that ChannelApe can send asynchronous communucation back to SAP ByD. |

### Example Payload

```json
{
  "purchaseOrderNumber": "R_2935999",
  "warehouseOrderId": "R_2935999",
  "channelOrderId": "R_2935999",
  "purchasedAt": "2023-02-13T19:00:00.000Z",
  "shippingCompany": "FEDEX",
  "shippingMethod": "GROUND",
  "locationId": "819",
  "warehouseCode": "LK",
  "warehouseCompany": "NRI",
  "warehouseName": "NRI - Available",
  "warehouseId": "LK",
  "warehouseZipcode": "91761",
  "trackingNumber": "208924061",
  "status": "OPEN",
  "additionalFields": {
    "VendorPartyInternalID": "1018",
    "SenderPartyInternalID": "123456789",
    "RecipientPartyInternalID": "1234567890123",
    "ShippingDateTimePeriod": "2023-02-13T19:00:00Z",
    "ArrivalDateTimePeriod": "2023-02-13T19:00:00Z",
    "note_attributes_ship_from": "819",
    "shipping_lines_title": "VENDOR",
    "note_attribute_retail_order": "true",
    "shipment_id": "208924061",
    "warehouse_order_id": "R_2935999",
    "Sscc": "00001922492001525086",
    "ShipmentNumber": "20892406",
    "LicensePlate": "0386149425",
    "CartonNumber": "1",
    "Length": "24",
    "Width": "14",
    "Height": "14",
    "Weight": "18.38",
    "TotalOrderWeight": "18.38",
    "TotalOrderCartons": "1",
    "ClientTotalFreightCharge": "140.00",
    "TotalOrderCubeVolume": "2.54"
  },
  "customer": {
    "additionalFields": [],
    "shippingAddress": {
      "name": "Upper West Side (NY)",
      "address1": "181 Columbus Ave",
      "city": "New York",
      "postalCode": "10023",
      "province": "New York",
      "provinceCode": "NY",
      "countryCode": "US"
    }
  },
  "lineItems": [
    {
      "additionalFields": {
        "DeliveryQuantityTypeCode": "EA",
        "HTSCode": "4202.92.3131",
        "CountryOfOrigin": "CN",
        "StockTransferOrderReferenceID": "2935999",
        "StockTransferOrderReferenceTypeCode": "814",
        "StockTransferOrderReferenceItemID": "1",
        "StockTransferOrderReferenceItemTypeCode": "74",
        "fulfillable_quantity": "4",
        "Z_PurchaseOrder_ODR": "R_2935999-01",
        "OutboundDeliveryExecution.ID": "5194550",
        "Item.ID": "1"
      },
      "id": "1",
      "quantity": 4,
      "sku": "125_001_BBPLD_OS",
      "taxes": []
    },
    {
      "additionalFields": {
        "DeliveryQuantityTypeCode": "PR",
        "HTSCode": "6404.19.3760",
        "CountryOfOrigin": "CN",
        "StockTransferOrderReferenceID": "2935999",
        "StockTransferOrderReferenceTypeCode": "814",
        "StockTransferOrderReferenceItemID": "2",
        "StockTransferOrderReferenceItemTypeCode": "74",
        "fulfillable_quantity": "4",
        "Z_PurchaseOrder_ODR": "R_2935999-01",
        "OutboundDeliveryExecution.ID": "5194550",
        "Item.ID": "2"
      },
      "id": "2",
      "quantity": 4,
      "sku": "117_001_GGY_7",
      "taxes": []
    }
  ]
}
```
