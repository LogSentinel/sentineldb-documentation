Migrating Existing Projects
===========================

It's easy to get started with SentinelDB for new projects, but in many cases there's an existing codebase with a standard relational database and migration may seem like a show-stopper. It isn't.

There is a straightforward migration path from your current database to SentinelDB for personal data:

1. Analyze your data and select only the the tables that have personal data in them. That would normally be the "users" table as well as related tables holding sensitive information. Nothing else has to be migrated.
2. Prepare your SentinelDB datastore and schema
3. Create a simple script that inserts all your existing data into SentinelDB. We support SQL INSERT statements, so a dump from your admin tool will work when used with our "upload SQL dump" functionality. If you want something more complex, you can write the insert statements against our API
4. In your code, whenever you have a User object (or any record with sensitive data), after fetching it from the relational database by ID, call SentinelDB by that ID to fetch the real data. This won't break any joins you have, as you keep your existing tables with only storing the ID column and possibly non-personal fields.
5. Change any queries against secondary indexes (e.g. select user by email) to be executed against SentinelDB using either our client tools or the RESTful API directly

You can have a parallel SentinelDB-and-relational-database period in production before switching off storing sensitive personal data in your relational database entirely.