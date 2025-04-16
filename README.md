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

🔄 Pipeline Design
Create a pipeline

Name: Load Product Data Pipeline

Activity: Add a Data Flow activity.

Configure Data Flow

Source 1 (File): Product text file from ADLS

Source 2 (Lookup Table): DimProduct table in the dedicated SQL pool

Lookup Transformation

Join: Outer Join on ProductID (source) = ProductAltKey (table)

Output: Combined dataset with matched and unmatched records

Alter Row Transformation

Insert: When ProductKey IS NULL (new product)

Update (Upsert): When ProductKey IS NOT NULL (existing product)

Sink

Destination: DimProduct table

Write behavior: Upsert (based on Alter Row rules)

🧪 Debug & Run
Use Debug mode in the data flow canvas to verify logic.

Use the Add Trigger > Trigger Now option to run the pipeline manually.

📊 Monitor
Go to the Monitor section in Synapse Studio.

Check the status of the pipeline in Pipeline Runs.

Ensure the pipeline completed successfully.

✅ Verify Results
Run a query on the DimProduct table to view loaded data:

sql
Copy
Edit
SELECT TOP 100 * FROM dbo.DimProduct;
🧹 Clean Up
After completing the lab or deployment, delete unnecessary Azure resources to avoid charges:

Synapse workspace

Dedicated SQL pool

Storage containers (if temporary)

📁 Folder Structure
cpp
Copy
Edit
/product-pipeline
├── README.md
├── pipeline-definition.json (optional export)
├── dataflow-definition.json (optional export)
└── sample-data/
    └── products.txt
📎 Notes
This pipeline implements a Type 1 SCD strategy (overwrite existing records).

Uses Azure Synapse Data Flows, not Mapping Data Flows in ADF.

🧑‍💻 Author
Sefa Öztürk
IT Trainee | Azure Data Engineer in progress
“Discipline from the Air Force, logic from Data.”
