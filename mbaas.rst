Mobile Backend as a Service
===========================

SentinelDB can be used as MBaaS (Mobile Backend as a Service). That roughly means you can directly invoke the SentinelDB APIs from your mobile applications rather than going through your own backend.

To do that, you should obtain the MBaaS credentials from the API Crednetials page – these credentials can only be used to register users and nothing else, so if someone manages to extract them from the mobile application, they won’t be able to do much.

After you register a user and log them in, store their token in the device (prefer secure storage), and then authenticate each request with the token stored:

.. code:: text
	
	Authorization: Bearer <token>

The user would only be allowed to obtain their own records.

Such a setup is ideal for applications that need to store personal or even medical data in the cloud and don’t want to take any risks in terms of compliance.

Obtaining the token
-------------------

The token is obtained by invoking the ``/api/oauth/token`` endpoint via POST and passing ``username``, ``password`` and ``datastoreId`` as request parameters.