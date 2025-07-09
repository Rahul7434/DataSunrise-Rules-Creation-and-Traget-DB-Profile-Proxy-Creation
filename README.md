# DataSunrise-Rules-Creation-and-Traget-DB-Profile-Proxy-Creation

## Setting Up Database in DataSunrise
```
Step 1: Create a Target Database Profile
- Go to Configurations > Databases in DataSunrise.
- Click "Add Database" to start adding your database info.
- Fill in the following:
    • Logical Name: Any name you choose.
    • Database Type: For example, select PostgreSQL.
    • Hostname: Type in the database’s IP address.
    • Port: Enter the port the database listens on (e.g., 5434).
- Click "Test Connection" to check if DataSunrise can talk to the database.

Step 2: Create a Proxy (for Security & Masking)
- Go to the Capture Mode section.
- Click "Add Proxy" to set up a DataSunrise proxy.
- Set the following:
    • Proxy IP Address (in "Listen on IP Address" field)
    • Proxy Port (in "Listen on Port" field — must be different from DB port, e.g., 54321)
- Click "Save" to finish creating the database profile.

Step 3: Connect Using PGAdmin
- In PGAdmin or any DB client, create a new connection.
- Use the proxy IP and proxy port instead of the database's actual IP and port.
- This ensures all traffic is routed through DataSunrise for monitoring and protection.

Quick Tip:
Client apps usually connect through the proxy, not the actual database. The proxy mimics the database’s behavior while providing security features like masking.

```
## Audit Rule Creation
```
Setting Up Database Audit in DataSunrise

Scenario: Audit all database queries.

Step 1: Create an Audit Rule
- Go to Data Audit > Rules section.
- Click "Add Rule" to create a new audit rule.
- Fill in the following under Main section:
    • Database Type: PostgreSQL (as example)
    • Database Instance: Pick the one you want to audit
    • Logical Rule Name: Any name you prefer
  *Note: You can select multiple instances of the same DB type. If none selected, rule applies to all of that type.

- Keep "Audit" as the default action.
- Check "Log Query Results" to log the DB's responses.
- Leave "Filter Sessions" default to audit all source IPs.
- Leave "Filter Statements" default to capture all DML queries.

Step 2: View Audit Results
- Send a test query (Example from PGAdmin):
    SELECT * FROM public.customers;

- Database will return the table data.

- In Web Console:
    • Go to Audit > Transactional Trails
    • Click on event ID to see details
    • You'll see:
        - SQL Query
        - Session Info
        - Query Results

Advanced Features for Audit Data:
- Integration with Elasticsearch & Kibana:
    • For advanced analytics and visual audit dashboards

- Integration with SIEM systems:
    • For real-time alerts and centralized monitoring

- Use Transactional Trails:
    • To view full query details and outputs
```
## Security Rule Creation
```
Setting Up Database Security in DataSunrise

Scenario: Prevent unauthorized changes to a database table.

Step 1: Create a Security Rule
- Go to Data Security > Rules section.
- Click "Add Rule" to create a new Security Rule.
- In General Settings:
    • Choose the target database (type: PostgreSQL)
    • Select database instance(s) or leave empty to apply rule to all of that type.
    • Give the rule a logical name.

- In Action Settings:
    • "Block" action is selected by default. Keep it as is.

- In Filter Statements:
    • Select "Object Group" filter.
    • Check INSERT, UPDATE, DELETE boxes to block modifications.

- In Process Query to Database Objects:
    • Specify the table: customers
    • This ensures the rule only targets changes to the customers table. Queries to other tables won't be affected.

Step 2: Blocking Results (Demo)
- From PGAdmin, run the following query:
    UPDATE public.customers
    SET "Last Name"='Burnwood'
    WHERE "Last Name"='Wade';

- Result:
    • The query gets blocked.
    • Error shown: "ERROR: The query is blocked"

Step 3: View Security Events
- Go to Data Security > Events
- Click on an event to see details:
    • SQL query info
    • Session details
    • Block action confirmation


```
## Setting Up Dynamic Data Masking Rule in DataSunrise
```
Setting Up Dynamic Data Masking Rule in DataSunrise

Step 1: Create the Rule
- Go to Masking > Rules
- Click "Add Rule" to begin setup

Step 2: Fill Main Section (General Settings)
- Provide basic details for the rule (e.g., name, target database)

Step 3: Set Action Subsection Parameters
- Keep Row Count:
    • Check this to allow masked columns in GROUP BY, WHERE, etc.
- Mask SELECTs Only:
    • Check this to mask only SELECT queries
- Action:
    • Choose how to handle modification queries on masked columns (e.g., block updates on masked Email column)

Step 4: Configure Filter Sessions
- Set rules for which sessions to apply masking to (e.g., IPs, users)

Step 5: Masking Settings Subsection
- Masking Settings By:
    • Choose mode: Mask Columns, Hide Rows, or Mask Data
    • For Mask Data, configure Event Tagging first

- Columns to Mask:
    • Click "Select" and choose columns manually
    • Use +Add RegEx options to match schema/table/column by pattern

- Masking Method (Mask Data only):
    • Choose data obfuscation method (refer to internal list)

- Hide Rows/Objects:
    • Select the target table/collection
    • Specify conditions to hide data (e.g., LastName ≠ 'Smith')

- Condition Examples:
    • SQL: LastName = 'Smith' (others hidden)
    • MongoDB: LastName matches 'Smith'
    • DynamoDB: LastName Equal String 'Smith'

- Data Filter:
    • Choose objects and info types to be masked
    • Data is masked only if info type matches the query

Note:
- If selected column has relationships (e.g., via Primary Key), system suggests masking related columns too

Step 6: Disable Rule (if not active)
- Check box to keep rule inactive

Step 7: Add Tags (optional)
- For rule organization and tracking

Step 8: Save Rule
- Click "Save Rule" to apply settings

```
## Periodic Data Discovery Task
```
Setting Up Periodic Data Discovery in DataSunrise

Purpose:
Automatically scan for sensitive data at scheduled intervals and update Object Groups.

Step 1: Create a New Task
- Go to Data Discovery > Periodic Data Discovery
- Click "+New Periodic Task"

Step 2: Select a Server
- Choose the server where the task will run

Step 3: Fill General Settings & Search Parameters

Search Parameters:
- Database Instance: Choose which DB to scan
- Credentials: Select user credentials for DB connection
- Save Results in Object Group: Optional — choose a group to store results

Search Strategy (Select Strategy drop-down):
- Select top rows: Scan only top rows defined by Number of Analyzed Rows
- Select random rows: Scan random set of rows
- Select all rows: Scan entire table

Column Match Strategy:
- Choose how to match column data to search filters

Min Percentage of Match:
- Minimum % of matched rows to consider a column sensitive

Number of Analyzed Rows:
- How many rows to scan from each table

Multiprocess Parameters:
- Execution Strategy:
    • Single Server
    • Multiple (stable servers) — for parallel scanning

Database Objects to Search:
- Database: Choose which DB to scan
- Schema: Select schema to target
- Search In: Pick object types to search (e.g. Table, View, Snapshot, etc.)
- Skip Objects:
    • Task Definition — manually choose objects to skip
    • Object Group — auto-skip based on previous group

Tag Criteria on Scanning:
- Use logical operators to filter tagged objects:
    • "&" AND, "|" OR, "!" NOT, "()" Grouping
    • Key-value filters with RegEx (value is optional)

Supported on:
- Amazon S3, Azure Cloud Storage, Google Cloud

Search Criteria:
- Search By:
    • Security standards
    • Existing Information types (e.g., credit cards, names)

- Search for Attributes:
    • Select required attribute(s) to match sensitive info

Startup Frequency:
- Frequency: Choose how often task should run (e.g., Manual, Daily, Weekly)

```
