# Webhooks

## Overview

This documentation explains the different types of notifications ChannelApe can send.

### Use Cases
1. Receive notification when a new Purchase Order is created
1. Receive notification when a new Stock Transfer Order is created
1. Receive notification when a new Sales Order is created
1. Receive notification when a Purchase Order ships
1. Receive notification when a Stock Transfer Order ships
1. Receive notification when a Sales Order ships

### Receiving Webhooks

In order to receive a webhook your application must provide an HTTPS endpoint.

### Security

A security token can be used to protect your endpoint.
ChannelApe will include a query parameter named `@authtoken` in the webhook notification request with its value set to the token provided.

#### Structure

TODO
```
root node
  - additionalFields
  - customer
  - ...
```

### Example Payload

```json
{
  "additionalFields": {
    "closed_at": "null",
    "number": "345",
    "note": "Check for fraud",
    "token": "889d29b22f9c6055d9844a41dd537e58",
    "taxes_included": "false",
    "financial_status": "paid",
    "total_discounts": "0.00",
    "total_line_items_price": "2000.00",
    "buyer_accepts_marketing": "false",
    "name": "CAWO11345",
    "cancelled_at": "null",
    "user_id": "79729033458",
    "location_id": "null",
    "browser_ip": "174.54.139.218",
    "order_number": "1345",
    "processing_method": "direct",
    "source_name": "shopify_draft_order",
    "tags": "bogo, new customer, summer sale, test123",
    "order_status_url": "https://channelape-warehouse-onboarding-1.myshopify.com/61758701810/orders/889d29b22f9c6055d9844a41dd537e58/authenticate?key=8c02536e3736fcde95d0eb27fffa2041",
    "updated_at": "2022-08-08T07:00:09.000-04:00",
    "shipping_lines_title": "express",
    "shipping_lines_code": "express",
    "shipping_lines_source": "shopify",
    "created_at": "2022-08-08T07:00:07.000-04:00",
    "processed_at": "2022-08-08T07:00:05.000-04:00",
    "risk_recommendation": "accept",
    "apple_pay": "false",
    "warehouse_order_id": "CAWO11345"
  },
  "customer": {
    "additionalFields": [
      {
        "name": "id",
        "value": "6026118824178"
      },
      {
        "name": "accepts_marketing",
        "value": "false"
      },
      {
        "name": "orders_count",
        "value": "3"
      },
      {
        "name": "total_spent",
        "value": "0.00"
      },
      {
        "name": "state",
        "value": "disabled"
      },
      {
        "name": "billing_address_company",
        "value": "ABC 123 Inc."
      }
    ],
    "billingAddress": {
      "additionalFields": [],
      "address1": "202 Wyoming Avenue",
      "address2": "Suite 100",
      "city": "Maplewood",
      "company": "ABC 123 Inc.",
      "country": "United States",
      "countryCode": "US",
      "firstName": "John",
      "lastName": "Davies",
      "name": "John Davies",
      "postalCode": "07040",
      "province": "New Jersey",
      "provinceCode": "NJ"
    },
    "email": "rjdavis@channelape.com",
    "firstName": "R.J.",
    "lastName": "Davis",
    "phone": "+15705556799",
    "shippingAddress": {
      "additionalFields": [],
      "address1": "224 Wyoming Ave",
      "address2": "Suite 100",
      "city": "Scranton",
      "country": "United States",
      "countryCode": "US",
      "firstName": "R.J.",
      "lastName": "Davis",
      "name": "R.J. Davis",
      "postalCode": "18503",
      "province": "Pennsylvania",
      "provinceCode": "PA"
    }
  },
  "lineItems": [
    {
      "additionalFields": {
        "variant_id": "42186875306226",
        "title": "ChannelApe Running Shoes",
        "variant_title": "9",
        "product_id": "7484045623538",
        "requires_shipping": "true",
        "taxable": "true",
        "gift_card": "false",
        "variant_inventory_management": "shopify",
        "fulfillable_quantity": "6",
        "total_discount": "0.00",
        "fulfillment_service": "manual"
      },
      "id": "12303922528498",
      "price": "125.00",
      "quantity": 6,
      "sku": "CARSBLKM-09",
      "grams": "680",
      "title": "ChannelApe Running Shoes - 9"
    },
    {
      "additionalFields": {
        "variant_id": "42186875338994",
        "title": "ChannelApe Running Shoes",
        "variant_title": "10",
        "product_id": "7484045623538",
        "requires_shipping": "true",
        "taxable": "true",
        "gift_card": "false",
        "variant_inventory_management": "shopify",
        "fulfillable_quantity": "8",
        "total_discount": "0.00",
        "fulfillment_service": "manual"
      },
      "id": "12303922561266",
      "price": "125.00",
      "quantity": 8,
      "sku": "CARSBLKM-10",
      "grams": "680",
      "title": "ChannelApe Running Shoes - 10"
    },
    {
      "additionalFields": {
        "variant_id": "42186875371762",
        "title": "ChannelApe Running Shoes",
        "variant_title": "11",
        "product_id": "7484045623538",
        "requires_shipping": "true",
        "taxable": "true",
        "gift_card": "false",
        "variant_inventory_management": "shopify",
        "fulfillable_quantity": "2",
        "total_discount": "0.00",
        "fulfillment_service": "manual"
      },
      "id": "12303922594034",
      "price": "125.00",
      "quantity": 2,
      "sku": "CARSBLKM-11",
      "grams": "680",
      "title": "ChannelApe Running Shoes - 11"
    }
  ],
  "locationId": "596",
  "purchasedAt": "2022-08-08T11:00:07.000Z",
  "purchaseOrderNumber": "CAWO11345",
  "shippingCompany": "FEDEX",
  "shippingMethod": "GROUND",
  "status": "NEW",
  "warehouseOrderId": "CAWO11345",
  "warehouseCode": "KYSPL2",
  "warehouseCompany": "United Parcel Service",
  "warehouseName": "UPS East Coast",
  "warehouseId": "UPSF",
  "warehouseZipcode": "40165",
  "channelOrderId": "4850616631538"
}
```