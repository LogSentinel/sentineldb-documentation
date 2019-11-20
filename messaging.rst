Messaging
========

Messaging system
**************

The messaging system allows you to send both SMS and emails via our API.

.. note::

    First you will need to configure SendGrid and Twilio credentials for you organization via our `UI <https://db.logsentinel.com/messaging>`_ before you can use the message system.

Then you can send messages via our API. Below are some curl-based examples of API calls for sending SMSes and emails:

Email example:


.. code:: text

    curl "http://db.logsentinel.com/api/message/email?recipientUserId=b05e7c49-3b2a-4aab...&templateId=4c726758-0391-4f1d-...&datastoreId=7fdbbb5f-eec0-469b...&senderEmail=sample@logsentinel.com&subject=sample" \
    -X POST \
    -H "Content-Type: application/json" \
    -H "Authorization: Basic NWFlZTI0OTctNDU4ZS00NjU4LWI5NDItNjQzOTNkOTZhN2I4OjF..."


SMS example:


.. code:: text

     curl "http://db.logsentinel.com/api/message/sms?recipientUserId=b05e7c49-3b2a-4aab...&templateId=4c726758-0391-4f1d-...&datastoreId=7fdbbb5f-eec0-469b..." \
    -X POST \
    -H "Content-Type: application/json" \
    -H "Authorization: Basic NWFlZTI0OTctNDU4ZS00NjU4LWI5NDItNjQzOTNkOTZhN2I4OjF..."


There is also the option to create and manage multiple templates (all of which are using `Pebble syntax <https://pebbletemplates.io/>`_). The templates can all be configured vie the `template UI <https://db.logsentinel.com/messaging>`_ or via an API call. For example:


.. code:: text

    curl "https://db.logsentinel.com/api/template" \
        -X POST \
        -d "{\n  \"name\": \"Sample Template\",\n  \"content\": \"This is a sample template\"\n}" \
        -H "Content-Type: application/json" \
        -H "Authorization: Basic NWFlZTI0OTctNDU4ZS00NjU4LWI5NDItNjQzOTNkOTZhN2I4OjF..."


This request creates a sample template with the name "Sample Template" and content "This is a sample template". As mentioned above, templates are also using `Pebble syntax <https://pebbletemplates.io/>`_ and below you can see an example of a template using `Pebble syntax <https://pebbletemplates.io/>`_.


.. code:: text

    curl "https://db.logsentinel.com/api/template" \
        -X POST \
        -d "{\n  \"name\": \"Sample Template with Pebble syntax\",\n  \"content\": \"Hello {{ user.username }}, your email is {{ user.email }}\"\n}" \
        -H "Content-Type: application/json" \
        -H "Authorization: Basic NWFlZTI0OTctNDU4ZS00NjU4LWI5NDItNjQzOTNkOTZhN2I4OjF..."

The Pebble syntax evaluates the ``user.username`` to the username of the recipient the email/SMS is send to (same with the ``user.email``).

There are a couple of "objects" like that and here is the full list of them.

* ``user`` - contains the general information about an user (here is a list of all the properties available)
    * ``.email``- gets the email of the user
    * ``.username``- gets the username of the user
    * ``.version``- gets the version of the user
    * ``.status``- gets the status of the user
    * ``.phoneNumber``- gets the phoneNumber of the user
    * ``.datastoreId``- gets the datastoreId of the user
    * ``.twoFactorAuthKey``- gets the twoFactorAuthKey of the user
    * ``.deleted``- checks if the user is deleted
* ``userAttributes`` - contains specific information about the user (you can access the content of this object with ``userAttribute.get("sample")``)
* ``record`` - contains the body of a given record (you can access the content of this object with ``record.get("sample")``)
* ``rawRecord`` - contains the general information about a record (here is a list of all the properties available)
    * ``.version``- gets the version of the record
    * ``.datastoreId``- gets the datastoreId of the record
    * ``.ownerId``- gets the ownerId of the record
    * ``.type``- gets the type of the record
    * ``.deleted``- checks if the record is deleted
    * ``.binary``- checks if the record is binary


For more information on how to use the template and messaging API you can check our `API Reference <https://api.db.logsentinel.com/api>`_
