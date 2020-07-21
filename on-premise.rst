On-premise protection
=====================

Drivers
*******

The SentinelDB JDBC driver can help you encrypt data for an existing application by just placing our JDBC driver (or ODBC driver via the ODBC-JDBC bridge) and changing the connection string to include ``sentineldb:`` right after ``jdbc:``. Then you configure the tables and columns you want to encrypt in the SentinelDB console and we take care of the rest

Proxy
*****

If the drivers don't suit your usecase, your can use the SentinelDB Proxy which supports MySQL and Postgres native protocols. You only have to change the connection string to point to the proxy rather than the actual database and configure which tables and columns to encrypt.

Application-level
*****************

We provide SDKs that allow applications to do the encryption/decryption and lookup in the application itself, rather than relying on proxying (JDBC driver proxy or TCP proxy). This can be desirable for greenfield projects that need fine-grained control. Ultimately, it's only the applications that should need to be able to access raw sensitive data, as any other channels pose a risk for data leaks.

How it works
************

We take take each record and encrypt it before inserting and updating it, using a per-record AES256 key. When data is retrieved, we lookup the key based on the record ID and decrypt the data. In order to enable queries, we transform the string values (and most sensitive data is string) via HMAC (with a per-table or per-schema HMAC key). That makes makes a non-reversible "tag" or "keyword" that we can store in a separate column and query for it. Full-text search in a database shoul generally be avoided, but if needed, the text can be split into keywords or ngrams and each keyword/ngram is HMAC'ed separately and stored alongside the record ID for retrieval.

In encryption schemes, key management is crucial. We rely on envelope encryption where the master key is ideally stored in a hardware module (or a service like AWS KMS, GGP KMS and similar). Keys are never stored in plaintext on disk - we store them wrapped, and make them available in memory for e brief period during decryption, after which they are "shredded". In case of using an on-premise proxy, we recommend disabling ``ptrace`` (on Linux machines)

The chosen scheme presents a practical tradeoff between utility (speed, functionality) and privacy. Monitoring queries and results may in theory reveal some of the encrypted information, but leaking data at scale is prevented.

Benefits
********

What does that approach give you?

- Encrypting each record with a separate key, thus minimizing the risk for bulk data theft (access to keys is controlled and monitored and keys can't be exported in bulk)
- Encryption that is transparent for the application (except for the application-level integration option above)
- Little performance hit due to the speed of symmetric (AES) encryption
- Data sensitivity granularity - only encrypt sensitive data and leave other columns (e.g. numbers, dates, IDs) unencrypted
- Easy integration - you have to do two things - change a connection string and decide which columns store sensitive information