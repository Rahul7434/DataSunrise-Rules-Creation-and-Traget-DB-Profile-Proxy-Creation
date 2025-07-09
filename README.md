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
