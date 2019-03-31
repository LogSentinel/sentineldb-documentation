Getting Started
===============
Overview
********
SentinelDB is a cloud-based datastore that ensures data protection and compliance with data-protection regulations like GDPR. It implements all data-protection best practices like encryption (per record), secure audit trail, anonymization and pseudonymization of data, two-factor authentication, fraud detection, as well as relying on a secure server infrastructure.

Main concepts
*************
Personal data
-------------
Personal data is any data that can be associated with a person that can be identified. In technical terms this means that once you have a “users” table, any record in any other table that has a “userId” reference, is also personal data (in addition to all the data in the “users” table).

Personal data store
-------------------
A personal data store is a module that is tasked with storing all personal data in a secure and compliant way. Many organizations choose to organize their internal systems around such a personal data store so that there is a single, well-protected place to keep personal data as opposed to having multiple systems that each has to implement all data protection measures. The personal data store is then accessed by all other systems when personal data is needed, but other systems do not store any personal data. SentinelDB can be seen as a Personal-data-store-as-a-service.

Data de-identification
----------------------
Data about a natural person, including medical data, should generally be protected. However, systems can choose to remove any characteristics from the data, which can be used to identify a natural person. That way data becomes anonymous and can be used for various purposes. More importantly, it doesn’t fall into the category of “personal data” when regulations (e.g. GDPR and HIPAA) are concerned. HIPAA Article 164.514(b) details how to de-identify medical data. One option is to remove all possible identifiers which renders the data unlinkable to a person.

Data de-identification is a useful mechanism for compliant data architecture. Organizations can store all personal information (all the identifiers that can be used to pinpoint a particular person) in a well-protected personal data store, and store de-identified data in multiple systems outside of that store. In simplified technical words, you keep only the user ID in the “users” table of each system and every other field is moved to a personal data store, where it can be retrieved via that ID.

User profile
------------
User profiles are the core entities of a data-protection focused system. The “users” table (or “user” object) holds all information that can be used to identify a person (names, email, personal identifier, passport number, SSN, address, etc.). Sometimes the structure is more complex, e.g. a user can have multiple addresses, which can be a separate table, but it’s part of the same user hierarchy.

Record
------
Records are pieces of data, parts of a personal data store, that are related to (or “owned by”) a user and should also be protected because they may contain identifying or sensitive information. A record can contain no identifying information and still be stored if the organization considers the data to be of high risk and in need of more thorough protection.

Some examples: direct messages can contain any information about the people involved, including identifying information, so it may be considered as a candidate for storage. Scanned documents where the contents are not technically parseable (except via OCR) can be stored as binary records. CVs usually contain personally identifiable information (PII) which cannot be easily stripped, so they can also be stored as records in a personal data store. Any other structured or unstructured data that may contain identifiable information can be stored as a record, associated with a given user.

The hard part that depends on the business case and the particular data in use is what entities to put in the personal data store as records associated with a given user and what to leave in existing systems, outside the personal data store. There is no recipe for this and decisions should be taken on a case-by-case basis.

Pseudonymization
----------------
Pseudonymization is a mechanism recommended by GDPR. In involves transforming data to a form that data subjects (people) cannot be identified, but that is reversible with the possession of a certain secret (e.g. a key). One approach would be to encrypt identifiers when exporting data. A personal data store can be queried for data and the response can be pseudonymized with a key, provided by the system that executes the query. Then the result can be provided to a third party (e.g. for analysis). After the analysis is done and the data is returned, the data can be de-pseudonymized by using the secret key.

Anonymization
-------------
Anonymization is the process of stripping all identifying personal data while keeping all non-identifying data. If a personal data store is used to store non-identifying data in the form of records, anonymization can mean removing all attributes of a user, but keeping the association between the user and their records. Anonymization can be done automatically after a certain period of time, which is a good practice – you don’t lose data that can yield business insights, but the identifying information is removed. Anonymization can sometimes be used to implement the right to be forgotten, but extra care should be taken, as records are preserved in that case. If all non-identifying data is stored outside the personal data store, then simply removing a user from the personal data store renders the data in other systems effectively anonymous.

Two-factor authentication
-------------------------
Two-factor authentication (2FA) is a mechanism for providing strong user authentication. It introduces an additional “factor” (element) to a traditional username/password authentication, thus reducing the risk of leaked credentials. 2FA schemes include the following factors: “something you know”, “something you have” and “something you are”. Usernames and passwords are “something you know”, smartphones and hardware tokens are “something you have”, and biometric data is “something you are”. In the context of a typical web application, a 2nd factor can be a smartphone with an OTP (one-time password) generator. Each time a user logs in, they should type their username and password and an additional 6-digit code to confirm that they possess the smartphone that was originally used to enroll the user in the 2FA scheme.

Strong authentication is key to data protection as it greatly reduces the risk of compromising personal accounts and extracting potentially sensitive information from them.

Authentication
**************
In order to use the API, you have to authenticate your calls. For that you need you need to `register <https://db.logsentinel.com/login#signup>`_ and obtain `API credentials <https://db.logsentinel.com/api-credentials>`_ . Then you should pass an Authorization header with each request:

.. code:: text

	Authorization: Basic <base64(organizationId:secret)>
	
In case you have mobile or desktop applications and want to store user-specific tokens, you can use OAuth to exchange a user’s username and password for an access token which is then passed using:

.. code:: text
	
	Authorization: Bearer <token>Storing data


Once you have authentication credentials, you can experiment through the `API console <https://db.logsentinel.com/api>`_ . Below is a step-by-step example using curl (we’ve trimmed the authentication header for better readability):

Search schema
*************

Data in SentinelDB is encrypted per record so in order to be able to search in the encrypted data, you have to define a schema. You can define it via the `search schema UI <https://db.logsentinel.com/search-schemas>`_ or via an API call. For example:

.. code:: text

	curl -X POST --header 'Content-Type: application/json' --header 'Accept: application/json' 
	--header 'Authorization: Basic NTBmOGNkYWYtNGUzZS00NDhhLWJmMDctOWRhODk5Nz...' -d '[ \
	  { \ 
		 "name": "customerName", \ 
		 "analyzed": true, \ 
		 "indexed": true, \ 
		 "visibilityLevel": "PUBLIC" \ 
	   } \ 
	 ]' 'https://api.db.logsentinel.com/api/search-schema/bc3f863b-796b-4ecc-96aa-abf0acea04a4/RECORD?recordType=Order'

	 
This creates a schema for a record of type ``Order`` with one field – the ``customerName``. You will be able to search by the customer name after inserting records. If you want to be able to search users by attributes other then their email, ID or username, you can define a search schema for your users as well.

.. note::

    Search schemas may appear flat, as you can only specify a list of fields, but in case you are storing a nested structure (e.g. ``address.primary.streetName``), you can use the dot notation to specify that a nested field is indexed and searchable
	

You can create and modify search schemas by API calls or via a dedicated UI from the dashboard. Each schema field has the following properties:

* ``name`` - the name of the field. In flat structures it is a simple name, but a dot notation can also be used in case of nested structures (as shown above, for example ``address.primary.streetName``)
* ``indexed`` - specifies whether the field is indexed and therefore searchable.
* ``analyzed`` - specifies whether keywords should be extracted from the field (useful for full-text fields). Note that due to the encrypted nature of the search, stemming or other keyword analysis is not performed.
* ``visibility`` - ``PUBLIC``, ``PRIVATE`` or ``PROTECTED``. The visibility is a property that instructs SentinelDB what fields to return for queries. Only public ones are returned by default. If private or protected fields are required to be returned, this should be explicitly specified in the query. This is useful for protecting sensitive data from accidentally being fetched and displayed on public pages.


Inserting data
**************

First, we create a user. The ``bc3f863b-796b-4ecc-96aa-abf0acea04a4`` parameter is the datastore ID in which we want to store the user (and then the record). We supply an arbitrary JSON for attributes as well as a few predefined fields like email and password:

.. code:: text

	curl -X POST --header 'Content-Type: application/json' --header 'Accept: application/json' 
	--header 'Authorization: Basic NTBmOGNkYWYtNGUzZS00NDhhLWJmMDctOWRhODk5Nz...' -d '{ \ 
	   "attributes": { \ 
		 "firstName": "John", \ 
		 "lastName": "Smith", \ 
		 "city": "London" \ 
	   }, \ 
	   "email": "john.smith%40example.com", \ 
	   "password": "password", \ 
	   "username": "john.smith%40example.com" \ 
	 }' 'https://db.logsentinel.com/api/user/datastore/bc3f863b-796b-4ecc-96aa-abf0acea04a4'


Then we create a new record (which in this case is a simple order). Note that we specify the type=Order parameter as well as the ownerId parameter. The ID of the owner is the ID that the “create user” query generated.


.. code:: text

	curl -X POST --header 'Content-Type: application/json' --header 'Accept: application/json' 
	--header 'Authorization: Basic NTBmOGNkYWYtNGUzZS00NDhhLWJmMDctOWRhODk5Nz...' -d '{ \ 
		   "customerName": "John Smith", \
		   "value": 120, \ 
		   "items": ["pizza"] \
		} \ 
	 }' 'https://db.logsentinel.com/api/record/datastore/bc3f863b-796b-4ecc-96aa-abf0acea04a4?ownerId=ab3f863b-796b-4ecc-96aa-abf0acea05a4&type=Order'

	 
Now we have a user with one order. If we want to retrieve all order for a given user, we just query SentinelDB. In this case we don’t specify any additional filtering criteria, so the body is an empty query ``{}``. (If we wanted to search by an indexed field (e.g. ``customerName``, we would be able to do that by specifying a list of key-value filters)

.. code:: text

	curl -X POST --header 'Content-Type: application/json' --header 'Accept: application/json' 
	--header 'Authorization: Basic NTBmOGNkYWYtNGUzZS00NDhhLWJmMDctOWRhODk5Nz...' 
	-d '{}' 'https://db.logsentinel.com/api/search/records/Order/datastore/bc3f863b-796b-4ecc-96aa-abf0acea04a4?pageSize=20'
	
