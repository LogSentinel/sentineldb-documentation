Security Architecture
=====================

SentinelDB relies on five major security components:

* **Encryption per record** – we encrypt every record with a separate key, using AES-256, thus ensuring that no data breach can occur even in case access is compromised. We do not  store any data unencrypted even in our search engine

* **Secure key hierarchy** – we have a multi-level hierarchy of encryption keys and a master key stored in a hardware security module (HSM). Each key is wrapped by its parent key and no key is stored in plain-text

* **Blockchain audit trail** – leveraging our award-winning Sentinel Trails product, we store an unmodifiable audit trail of all events related to data access and modification. We also periodically compare the current state of the database with the audit trail to guarantee the integrity of data

* **Fraud detection** – multiple layers of fraud detection are employed to detect and block any potential breaches, disguised as legitimate traffic, through the RESTful API

* **Hardended infrastructure security** – we rely on Amazon Web Services for a secure and compliant underlying infrastructure, but we improve on that by utilizing multi-factor authentication as well as secure audit trail for each administrative access. All data is encrypted in transit and at rest.

Below is an overview of the SentinelDB architecture:

.. image:: https://d381qa7mgybj77.cloudfront.net/wp-content/uploads/2018/11/SENTINELDB_How_It_Works_01-768x356.png
