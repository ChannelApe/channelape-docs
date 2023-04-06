# Inventory Exports

There are two kinds of inventory exports:

1. Intraday Adjustments
1. Inventory True Up

## Intraday Adjustments

The example [here](adjustments.json) demonstrates several scenarios involving both warehouse and store.

| Scenario                                                                                | Batch ID                             |
| --------------------------------------------------------------------------------------- | ------------------------------------ |
| Inventory Transfer from Available to Sell to Damaged                                    | 40d8b268-8747-4971-919e-2bf441f12028 |
| Another Inventory Transfer from Available to Sell to Damaged occurring at the same time | 7fc557d2-ec5e-4928-96e3-a3ae18ac8481 |
| Inventory Transfer from Available to Sell to Samples                                    | 7fc557d2-ec5e-4928-96e3-a3ae18ac8481 |
| Inventory is adjusted out of Damaged                                                    | 973b4fed-84de-440d-9e4e-6d47e3f1558c |
| Damaged inventory is repaired and transferred back to Available to Sell                 | e3a4dbe7-e2dc-48d2-a7a6-78e2a59aadb0 |
| Inventory is put on display at the Fillmore retail store                                | a9b4eaed-fe0a-42b6-a658-4ccb39167303 |

The examples [here](nri-test-adjustments-2023-04.json) show real examples of warehouse inventory adjustments.

| Sceanrio                                                                                             | Batch ID                             |
| ---------------------------------------------------------------------------------------------------- | ------------------------------------ |
| Cycle Count: Adjust 3 units out of 119_003_CRMS_9.5                                                  | c9c6830a-58e6-43e2-8bf6-b0bb88375eec |
| Damaged: Adjust 2 units out of available and into damaged for 119_003_CRMS_10.5                      | d699f73a-c2e4-4f6b-8b2d-76594fbc8fd3 |
| Inventory Count: Adjust for 5 units of 130_012_CHAIT_13                                              | ca007f34-0d0e-4119-81d3-83432fea13dc |
| Master Carton Error: Adjust for 4 units of 093_043_CMC_OS                                            | 11ae7a6b-a5d3-44e9-92b1-ab1b6de7a3c6 |
| Physical Count: Adjust 3 units out of 093_034_BBGHM_OS                                               | 4cd3e6d2-9077-4436-b3cc-09f897d60002 |
| Receiving Error: Adjust for 2 units of 001_289_BSL_9.5                                               | b14d2f96-700b-4094-9e4b-0c060d3d201b |
| Short Shipped: Adjust for 5 units of 001_289_BSL_10.5                                                | cdf4a1e2-a3b6-487d-b044-21b2399ca4b1 |
| Warehouse Transfer: Adjust for 5 units out of available warehouse and into damaged for 002_110_BLK_9 | c20f8a9d-8aea-4291-a429-9894298bde09 |

## Inventory True Up

An inventory true up is a snapshot of the inventory balances in ChannelApe's IMS at the time it is created.

By default this snapshot will include an entry for every product in every location and that entry will contain the balance of inventory in each status, including 0.

In the example [here](inventory-export.json), you will find the inventory balance for two products of different types (Unit of Measure `PR` and `EA`) at 4 different locations:

1. NRI - Available
1. NRI - Damaged
1. Fillmore
1. Fillmore (Unsellable)