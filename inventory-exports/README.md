# Inventory Exports

There are two kinds of inventory exports:

1. Intraday Adjustments
1. Inventory True Up

## Intraday Adjustments

See an example [here](adjustments.json)

This example demonstrates a few scenarios

| Scenario                                                                                | Batch ID                             |
| --------------------------------------------------------------------------------------- | ------------------------------------ |
| Inventory Transfer from Available to Sell to Damaged                                    | 40d8b268-8747-4971-919e-2bf441f12028 |
| Another Inventory Transfer from Available to Sell to Damaged occurring at the same time | 7fc557d2-ec5e-4928-96e3-a3ae18ac8481 |
| Inventory Transfer from Available to Sell to Samples                                    | 7fc557d2-ec5e-4928-96e3-a3ae18ac8481 |
| Inventory is adjusted out of Damaged                                                    | 973b4fed-84de-440d-9e4e-6d47e3f1558c |
| Damaged inventory is repaired and transferred back to Available to Sell                 | e3a4dbe7-e2dc-48d2-a7a6-78e2a59aadb0 |
| Inventory is put on display at the Fillmore retail store                                | a9b4eaed-fe0a-42b6-a658-4ccb39167303 |

## Inventory True Up
