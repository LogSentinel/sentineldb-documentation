Code examples
=============

Below are some code example for basic SentinelDB functionality:

Creating a user profile
***********************

.. content-tabs::

	.. tab-container:: java
		:title: Java
		
		.. code-block:: java
		
			SentinelDBClient client = SentinelDBClientBuilder.create(orgId, secret).build();
			
			Map<String, String> attributes = new HashMap<>();
			attributes.put("firstName", "John");
			attributes.put("lastName", "Smith");
			attributes.put("city", "London");
			
			UserRequest user = new UserRequest();
			user.setEmail("john.smith@example.com");
			user.setPassword("password");
			user.setAttributes(gson.toJson(attributes));
			
			UUID id = client.getUserActions().createUser(datastoreId, user, null).getId();
		
	.. tab-container:: php
		:title: PHP
	
	.. tab-container:: python
		:title: Python
	
	.. tab-container:: nodejs
		:title: Node.js

		.. code-block:: javascript
		
			var https = require('https');
			var data = JSON.stringify({
			  'email': 'john.smith@example.com',
			  'password': 'password'
			  'attributes': {
				'firstName': 'John',
				'lastName': 'Smith',
				'city': 'London'
			  }
			});

			var auth = 'Basic ' + Buffer.from(ORG_ID + ':' + ORG_SECRET).toString('base64')

			var options = {
			  host: 'api.db.logsentinel.com',
			  path: '/user/datastore/' + DATASTORE_ID,
			  method: 'POST',
			  headers: {
				'Content-Type': 'application/json; charset=utf-8',
				'Content-Length': data.length
				'Authorization': auth;
			  }
			};

			var req = https.request(options, function(res) {
			  var id = JSON.parse(response.body).id
			  //...
			});

			req.write(data);
			req.end();
