# Assignment

## Implement Unity Catalog

1. Set up Unity Catalog Metastore in an Azure Databricks Environment. 
2. Create the root storage account for the metastore. 
3. Create the azure databricks access connector. 
4. Create the metastore in azure databricks account console. 
5. Create catalog and managed table. 
6. Create external table. 
7. Provide row level security and column level filtering using the dynamic view.


## Solution

<span style="color:red">Complete help of ChatGPT</span>

To implement Unity Catalog in an Azure Databricks environment, follow these steps. Unity Catalog is a governance solution for data in Databricks, providing fine-grained controls and enabling the management of access policies for your data.

### Step 1: Set Up Unity Catalog Metastore in Azure Databricks

**Prerequisites:**
- Azure Databricks Workspace
- Azure Storage Account
- Azure Active Directory (AAD) with appropriate permissions

### Step 2: Create the Root Storage Account for the Metastore

1. **Create an Azure Storage Account:**
   - Navigate to the Azure portal.
   - Create a new Storage Account.
   - Configure the storage account with the desired settings (e.g., region, performance).
   - Make sure to create a container within the storage account to store Unity Catalog data.

2. **Generate an Access Key for the Storage Account:**
   - Go to the Storage Account.
   - Under "Security + networking," select "Access keys."
   - Copy one of the access keys (you'll need it later).

### Step 3: Create the Azure Databricks Access Connector

1. **Create an Azure Databricks Access Connector:**
   - Navigate to the Azure portal.
   - Search for "Databricks Access Connector" and create a new one.
   - Link it to your Azure Storage Account by providing the storage account name and access key.

2. **Assign Roles to the Access Connector:**
   - In the Azure portal, assign the `Storage Blob Data Contributor` role to the access connector for your storage account.

### Step 4: Create the Metastore in Azure Databricks Account Console

1. **Navigate to the Azure Databricks Account Console:**
   - Go to the Databricks workspace.
   - In the left sidebar, click on the "Settings" gear icon and select "Admin Console."
   - Under the "Unity Catalog" section, click "Metastores."

2. **Create a New Metastore:**
   - Click "Create Metastore."
   - Enter the name and select the Azure Storage Account and the Access Connector.
   - Complete the setup by clicking "Create."

### Step 5: Create Catalog and Managed Table

1. **Create a Catalog:**
   - In the Databricks workspace, use SQL commands in a notebook to create a catalog.
   - Example:
     ```sql
     CREATE CATALOG my_catalog;
     ```

2. **Create a Managed Table:**
   - Within the catalog, create a schema and a managed table.
   - Example:
     ```sql
     CREATE SCHEMA my_schema;
     CREATE TABLE my_schema.my_managed_table (
         id INT,
         name STRING
     );
     ```

### Step 6: Create an External Table

1. **Create an External Table:**
   - Store data in an external storage location and register it with Unity Catalog as an external table.
   - Example:
     ```sql
     CREATE TABLE my_schema.my_external_table
     USING DELTA
     LOCATION 'abfss://<container>@<storage-account>.dfs.core.windows.net/external-table-data';
     ```

### Step 7: Provide Row-Level Security and Column-Level Filtering Using Dynamic View

1. **Create a Dynamic View:**
   - Create a view that enforces row-level security based on user attributes.
   - Example:
     ```sql
     CREATE VIEW my_schema.secure_view AS
     SELECT id, name
     FROM my_schema.my_managed_table
     WHERE user() = 'specific_user';
     ```

2. **Implement Column-Level Security:**
   - Use SQL functions to mask or filter columns based on conditions.
   - Example:
     ```sql
     CREATE VIEW my_schema.column_masked_view AS
     SELECT id, 
            CASE 
               WHEN user() = 'specific_user' THEN name 
               ELSE 'REDACTED' 
            END AS name
     FROM my_schema.my_managed_table;
     ```

### Step 8: Verify and Manage Access

1. **Assign Permissions:**
   - Use the `GRANT` command to assign permissions to users or groups.
   - Example:
     ```sql
     GRANT SELECT ON my_schema.secure_view TO user_name;
     ```

2. **Test the Setup:**
   - Test the row-level security and column filtering by accessing the views as different users.
