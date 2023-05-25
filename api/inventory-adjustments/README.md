# Inventory Adjustments

⚠️ Caution: Our API supports two different inventory operations.

This section describes how to adjust inventory balances in ChannelApe using the `ADJUST` operation.

The other is a `SET` operation which is used to complete overwrite ChannelApe's inventory balance.
If you're interested in learning more, check out about our [Inventory Management System](https://www.channelape.com/knowledge/channelape-inventory-management-system) knowledgebase article.

## Batch Adjustments API

### Overview

This API is used to enter inventory adjustments or inventory transfers in ChannelApe's Inventory Management System.
ChannelApe will assign a unique ID to the batch upon submission which is used to identify the related adjustments.

Additional documentation can be found at [docs.channelape.io](https://docs.channelape.io/#fa5b8800-7847-4168-a68b-6430e4f8fe2f)

### Endpoint

Send a `POST` request to `https://api.channelape.com/v1/batches`

### Inventory Transfer Example

You have two inventory locations:

1. Location named `Fillmore Available`, location ID `309`, for inventory you can sell
1. Location named `Fillmore Unsellable`, location ID `1049`, for inventory not in a sellable condition

You discover 1 unit of SKU `005_A02_NEOLME_` is damaged and transferred from Available to Unsellable.

cURL Example

```bash
curl --location -g 'https://api.channelape.com/v1/batches' \
--header 'Content-Type: application/json' \
--header 'x-channel-ape-authorization-token: YOUR_API_KEY' \
--data '{
  "businessId": "ec5fcd55-c275-4843-9641-d0fd44b9f173",
  "adjustments": [
    {
      "idempotentKey": "UNIQUE_ADJUSTMENT_ID_FROM_YOUR_SYSTEM",
      "sku": "005_A02_NEOLME_",
      "quantity": -1,
      "operation": "ADJUST",
      "locationId": 309,
      "inventoryStatus": "AVAILABLE_TO_SELL",
      "memo": "Visual Prop Disp +"
    },
    {
      "idempotentKey": "UNIQUE_ADJUSTMENT_ID_FROM_YOUR_SYSTEM",
      "sku": "005_A02_NEOLME_",
      "quantity": 1,
      "operation": "ADJUST",
      "locationId": 1049,
      "inventoryStatus": "AVAILABLE_TO_SELL",
      "memo": "Visual Prop Disp +"
    }
  ]
}'
```

You should receive a response like this with status code 201 if successful:

```json
{
  "id": "a9b4eaed-fe0a-42b6-a658-4ccb39167303",
  "type": "INVENTORY",
  "status": "COMPLETE",
  "createdAt": "2023-02-22T19:00:00.000Z",
  "updatedAt": "2023-02-22T19:00:00.000Z",
  "businessId": "ec5fcd55-c275-4843-9641-d0fd44b9f173"
}
```

This inventory transfer will then appear in inventory exports like the example [here](../../inventory-exports/adjustments.json#L129-L156).

### Inventory Adjustment Example

4 of the 5 damaged units of SKU `117_001_GGY_7` are damaged beyond repair and scrapped.

You are adjusting out 4 units from your `Fillmore Unsellable` location, ID `1049`, once scrapped.

cURL Example

```bash
curl --location -g 'https://api.channelape.com/v1/batches' \
--header 'Content-Type: application/json' \
--header 'x-channel-ape-authorization-token: your-api-key' \
--data '{
  "businessId": "ec5fcd55-c275-4843-9641-d0fd44b9f173",
  "adjustments": [
    {
      "idempotentKey": "UNIQUE_ADJUSTMENT_ID_FROM_YOUR_SYSTEM_2",
      "sku": "117_001_GGY_7",
      "quantity": -4,
      "operation": "ADJUST",
      "locationId": 1049,
      "inventoryStatus": "AVAILABLE_TO_SELL",
      "memo": "Scrapped"
    }
  ]
}'
```

### Inventory Audit Example

50 units of SKU `119_003_CRMS_13` were found at `Fillmore` (ID `309`) after performing a physical inventory count.

cURL Example

```bash
curl --location -g 'https://api.channelape.com/v1/batches' \
--header 'Content-Type: application/json' \
--header 'x-channel-ape-authorization-token: your-api-key' \
--data '{
  "businessId": "ec5fcd55-c275-4843-9641-d0fd44b9f173",
  "adjustments": [
    {
      "idempotentKey": "UNIQUE_ADJUSTMENT_ID_FROM_YOUR_SYSTEM_2",
      "sku": "119_003_CRMS_13",
      "quantity": 50,
      "operation": "SET",
      "locationId": 309,
      "inventoryStatus": "AVAILABLE_TO_SELL",
      "memo": "PI Audit"
    }
  ]
}'
```

### Headers

- Note: The `x-channel-ape-idempotent-key` header is not needed here as it is part of the adjustment in the request.

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
