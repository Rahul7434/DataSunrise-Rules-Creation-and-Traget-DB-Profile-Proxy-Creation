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
