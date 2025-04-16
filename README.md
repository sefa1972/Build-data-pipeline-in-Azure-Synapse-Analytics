# Build-data-pipeline-in-Azure-Synapse-Analytics

This project demonstrates how to build a data pipeline in **Azure Synapse Analytics** that loads product data from a text file into a **dedicated SQL pool**. The pipeline is designed to perform **insert and update (upsert)** operations by matching records based on a product key.

---

## Scenario

The source data is a `.txt` file containing product information. The destination is a table named `DimProduct` in a dedicated SQL pool.

The objective is to:
- Load the data from the file.
- Match existing records based on `ProductAltKey`.
- Insert new records and update existing ones (Type 1 Slowly Changing Dimension).

---

## Prerequisites

- Azure subscription
- Azure Synapse Analytics workspace provisioned
- Dedicated SQL pool created
- Data lake storage with access to the product text file
- `DimProduct` table created in the dedicated SQL pool

---

## DimProduct Table Schema

```sql
CREATE TABLE [dbo].[DimProduct](
    [ProductKey] [int] IDENTITY NOT NULL,
      NULL,
      NULL,
      NULL,
      NULL,
    [Listprice] [MONEY] NULL,
    [Discontinued] [BIT] NULL)
WITH
(
    DISTRIBUTION = HASH(ProductAltKey),
    CLUSTERED COLUMNSTORE INDEX
);
