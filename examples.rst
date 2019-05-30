Code examples
=============

Below are some code example for basic SentinelDB functionality:

Creating a user profile
***********************

.. content-tabs::

	.. tab-container:: java
		:title: Java
		
		The Java example uses the `sentineldb-java-client <https://github.com/LogSentinel/sentineldb-java-client/>`_ 
		
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
		
		.. code-block:: php
		
			$data = <<<EOT
			{
			  "email": "john.smith@example.com",
			  "password": "password",
			  "attributes": {
				"firstName": "John",
				"lastName": "Smith",
				"city": "London"
			  }
			}
			EOT;
			
			$curl = curl_init();
			curl_setopt($curl, CURLOPT_POST, 1);
			curl_setopt($curl, CURLOPT_POSTFIELDS, $data);
			
			curl_setopt($curl, CURLOPT_URL, 'https://api.db.logsentinel.com/api/user/datastore/' + datastoreId);
			curl_setopt($curl, CURLOPT_HTTPHEADER, array(
				'Content-Type: application/json'
			));
			curl_setopt($curl, CURLOPT_RETURNTRANSFER, 1);
			curl_setopt($curl, CURLOPT_HTTPAUTH, CURLAUTH_BASIC);
			curl_setopt($curl, CURLOPT_USERPWD, $ORG_ID . ":" . $SECRET);
			
			// EXECUTE:
			$result = curl_exec($curl);
		
	
	.. tab-container:: python
		:title: Python
		
		.. code-block:: python
			
			import requests
			url = 'https://api.db.logsentinel.com/api/user/datastore/' + datastoreId;
			data = '''{
			  "email": "john.smith@example.com",
			  "password": "password",
			  "attributes": {
				"firstName": "John",
				"lastName": "Smith",
				"city": "London"
			  }
			}'''
			
			response = requests.post(url, auth=HTTPBasicAuth(orgId, secret), data=data, headers={"Content-Type": "application/json"})

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
			  path: '/api/user/datastore/' + DATASTORE_ID,
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

Creating a record
***********************

.. content-tabs::

	.. tab-container:: java
		:title: Java
		
		The Java example uses the `sentineldb-java-client <https://github.com/LogSentinel/sentineldb-java-client/>`_ 
		
		.. code-block:: java
		
			SentinelDBClient client = SentinelDBClientBuilder.create(orgId, secret).build();

			Map<String, String> attributes = new HashMap<>();
			attributes.put("name", "Record name");
			attributes.put("details", "Some details");
			attributes.put("code", "LDR354");

			UUID id = client.getRecordActions().createRecord(gson.toJson(attributes), datastoreId, "ActorId", null, "Document").getId(); 
			
	.. tab-container:: php
		:title: PHP
		
		.. code-block:: php
			
			$data = <<<EOT				
				{
				  "name": "Record Name",
				  "details": "Some details",
				  "code": "LDR354"
				}
			EOT;
			
			$curl = curl_init();
			curl_setopt($curl, CURLOPT_POST, 1);
			curl_setopt($curl, CURLOPT_POSTFIELDS, $data);
			
			curl_setopt($curl, CURLOPT_URL, 'https://api.db.logsentinel.com/api/record/datastore/' + datastoreId + '?type=Document');
			curl_setopt($curl, CURLOPT_HTTPHEADER, array(
				'Content-Type: application/json'
			));
			curl_setopt($curl, CURLOPT_RETURNTRANSFER, 1);
			curl_setopt($curl, CURLOPT_HTTPAUTH, CURLAUTH_BASIC);
			curl_setopt($curl, CURLOPT_USERPWD, $ORG_ID . ":" . $SECRET);
			
			// EXECUTE:
			$result = curl_exec($curl);
			
	.. tab-container:: Python
		:title: Python
		
		.. code-block:: python
		
			import requests
			url = 'https://api.db.logsentinel.com/api/record/datastore/' + datastoreId + '?type=Document';
			data = '''{
			  "name": "Record Name",
			  "details": "Some details",
			  "code": "LDR354"
			}'''

			response = requests.post(url, auth=HTTPBasicAuth(orgId, secret), data=data, headers={"Content-Type": "application/json"})
			
	.. tab-container:: nodejs
		:title: Node.js

		.. code-block:: javascript
		
			var https = require('https');
			var data = JSON.stringify({
			   'name': 'Record Name',
			   'details': 'Some details',
			   'code': 'LDR354'
			});

			var auth = 'Basic ' + Buffer.from(ORG_ID + ':' + ORG_SECRET).toString('base64')

			var options = {
			  host: 'api.db.logsentinel.com',
			  path: '/api/record/datastore/' + DATASTORE_ID + '?type=Document',
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
			
Searching records
***********************

.. content-tabs::

	.. tab-container:: java
		:title: Java
		
		The Java example uses the `sentineldb-java-client <https://github.com/LogSentinel/sentineldb-java-client/>`_ 
		
		.. code-block:: java
		
			SentinelDBClient client = SentinelDBClientBuilder.create(orgId, secret).build();

			Map<String, String> searchRecordRequest = new HashMap<>();
			searchRecordRequest.put("code", "LDR354");
			searchRecordRequest.put("name", "Record name");

			long start = System.currentTimeMillis() - 1000*60*60*24L;
			long end = System.currentTimeMillis();
			int pageNumber = 0;
			int pageSize = 10;
			String type = "Document";
			SearchSchemaField.VisibilityLevelEnum visibilityLevel = SearchSchemaField.VisibilityLevelEnum.PUBLIC;

			List<Record> searchResult = client.getSearchActions().searchRecords(
				datastoreId, searchRecordRequest, type, null, end, pageNumber, pageSize, null, start, visibilityLevel);
				
	.. tab-container:: php
		:title: PHP
		
		.. code-block:: php
		
			$searchReuqest = <<<EOT
			{
			  "name": "Record Name",
			  "code": "LDR354"
			}
			EOT;
			
			$start = time() * 1000 - 1000*60*60*24L;
			$end = time() * 1000;
			$pageNumber = 0;
			$pageSize = 10;
			$type = 'Document';
			$visibilityLevel = 'PUBLIC';
			
			$curl = curl_init();
			curl_setopt($curl, CURLOPT_POST, 1);
			curl_setopt($curl, CURLOPT_POSTFIELDS, $searchReuqest);
			
			curl_setopt($curl, CURLOPT_URL, 'https://api.db.logsentinel.com/api/search/records/' + type  + '/datastore/' + datastoreId + '?start=' + start + &end=' + end + '&pageNumber=' + pageNumber + '&pageSize=' + pageSize + '&visibilityLevel=' + visibilityLevel);
			curl_setopt($curl, CURLOPT_HTTPHEADER, array(
				'Content-Type: application/json'
			));
			curl_setopt($curl, CURLOPT_RETURNTRANSFER, 1);
			curl_setopt($curl, CURLOPT_HTTPAUTH, CURLAUTH_BASIC);
			curl_setopt($curl, CURLOPT_USERPWD, $ORG_ID . ":" . $SECRET);
			
			// EXECUTE:
			$result = curl_exec($curl);
			
	.. tab-container:: Python
		:title: Python
		
		.. code-block:: python
		
			import requests
			import time
			
			searchRequest = '''
				{
				  "name": "Record Name",
				  "code": "LDR354"
				}
			'''

			start = int(round(time.time() * 1000)) - 1000*60*60*24L;
			end  = int(round(time.time() * 1000));
			pageNumber = 0;
			pageSize = 10;
			type = 'Document';
			visibilityLevel = 'PUBLIC';

			url = 'https://api.db.logsentinel.com/api/search/records/' + type  + '/datastore/' + datastoreId + '?start=' + start + &end=' + end + '&pageNumber=' + pageNumber + '&pageSize=' + pageSize + '&visibilityLevel=' + visibilityLevel;

			response = requests.post(url, auth=HTTPBasicAuth(orgId, secret), data=searchRequest, headers={"Content-Type": "application/json"})
			
	.. tab-container:: nodejs
		:title: Node.js

		.. code-block:: javascript
		
			var https = require('https');
			
			var searchRequest = JSON.stringify({
			  "name": "Record Name",
			  "code": "LDR354"
			});

			var start = new Date().getTime() - 1000*60*60*24L;
			var end  = new Date().getTime()
			var pageNumber = 0;
			var pageSize = 10;
			var type = 'Document';
			var visibilityLevel = 'PUBLIC';


			var auth = 'Basic ' + Buffer.from(ORG_ID + ':' + ORG_SECRET).toString('base64')

			var options = {
			  host: 'api.db.logsentinel.com',
			  path: '/api/search/records/' + type  + '/datastore/' + datastoreId + '?start=' + start + &end=' + end + '&pageNumber=' + pageNumber + '&pageSize=' + pageSize + '&visibilityLevel=' + visibilityLevel,
			  method: 'POST',
			  headers: {
				'Content-Type': 'application/json; charset=utf-8',
				'Content-Length': data.length
				'Authorization': auth;
			  }
			};

			var req = https.request(options, function(res) {
			  var result = JSON.parse(response.body)
			  //...
			});

			req.write(searchRequest);
			req.end();
			
Searching users
***********************

.. content-tabs::

	.. tab-container:: java
		:title: Java
		
		The Java example uses the `sentineldb-java-client <https://github.com/LogSentinel/sentineldb-java-client/>`_ 
		
		.. code-block:: java
		
			SentinelDBClient client = SentinelDBClientBuilder.create(orgId, secret).build();

			Map<String, String> searchUserRequest = new HashMap<>();
			searchUserRequest.put("city", "London");
			searchUserRequest.put("name", "John");

			long start = System.currentTimeMillis() - 1000*60*60*24L;
			long end = System.currentTimeMillis();
			int pageNumber = 0;
			int pageSize = 10;
			String type = "Document";
			SearchSchemaField.VisibilityLevelEnum visibilityLevel = SearchSchemaField.VisibilityLevelEnum.PUBLIC;

			List<Record> searchResult = client.getSearchActions().searchUsers(
                datastoreId, searchUserRequest, end, pageNumber, pageSize, null, start, visibilityLevel);
				
	.. tab-container:: php
		:title: PHP
		
		.. code-block:: php
		
			$searchReuqest = <<<EOT
			{
			  "city": "London",
			  "name": "John"
			}
			EOT;
			
			$start = time() * 1000 - 1000*60*60*24L;
			$end = time() * 1000;
			$pageNumber = 0;
			$pageSize = 10;
			$type = 'Document';
			$visibilityLevel = 'PUBLIC';
			
			$curl = curl_init();
			curl_setopt($curl, CURLOPT_POST, 1);
			curl_setopt($curl, CURLOPT_POSTFIELDS, $searchReuqest);
			
			curl_setopt($curl, CURLOPT_URL, 'https://api.db.logsentinel.com/api/search/users/datastore/' + datastoreId + '?start=' + start + &end=' + end + '&pageNumber=' + pageNumber + '&pageSize=' + pageSize + '&visibilityLevel=' + visibilityLevel);
			curl_setopt($curl, CURLOPT_HTTPHEADER, array(
				'Content-Type: application/json'
			));
			curl_setopt($curl, CURLOPT_RETURNTRANSFER, 1);
			curl_setopt($curl, CURLOPT_HTTPAUTH, CURLAUTH_BASIC);
			curl_setopt($curl, CURLOPT_USERPWD, $ORG_ID . ":" . $SECRET);
			
			// EXECUTE:
			$result = curl_exec($curl);
			
	.. tab-container:: Python
		:title: Python
		
		.. code-block:: python
		
			import requests
			import time
			
			searchRequest = '''
				{
				  "city": "London",
				  "name": "John"
				}
			'''

			start = int(round(time.time() * 1000)) - 1000*60*60*24L;
			end  = int(round(time.time() * 1000));
			pageNumber = 0;
			pageSize = 10;
			type = 'Document';
			visibilityLevel = 'PUBLIC';

			url = 'https://api.db.logsentinel.com/api/search/users/datastore/' + datastoreId + '?start=' + start + &end=' + end + '&pageNumber=' + pageNumber + '&pageSize=' + pageSize + '&visibilityLevel=' + visibilityLevel;


			response = requests.post(url, auth=HTTPBasicAuth(orgId, secret), data=searchRequest, headers={"Content-Type": "application/json"})
			
	.. tab-container:: nodejs
		:title: Node.js

		.. code-block:: javascript
		
			var https = require('https');
		
			var searchRequest = JSON.stringify({
			   "city": "London",
			   "name": "John"
			});

			var start = new Date().getTime() - 1000*60*60*24L;
			var end  = new Date().getTime()
			var pageNumber = 0;
			var pageSize = 10;
			var type = 'Document';
			var visibilityLevel = 'PUBLIC';


			var auth = 'Basic ' + Buffer.from(ORG_ID + ':' + ORG_SECRET).toString('base64')

			var options = {
			  host: 'api.db.logsentinel.com',
			  path: '/api/search/records/datastore/' + datastoreId + '?start=' + start + &end=' + end + '&pageNumber=' + pageNumber + '&pageSize=' + pageSize + '&visibilityLevel=' + visibilityLevel,
			  method: 'POST',
			  headers: {
				'Content-Type': 'application/json; charset=utf-8',
				'Content-Length': data.length
				'Authorization': auth;
			  }
			};

			var req = https.request(options, function(res) {
			  var result = JSON.parse(response.body)
			  //...
			});

			req.write(searchRequest);
			req.end();


Batch insert
***********************

.. content-tabs::

	.. tab-container:: java
		:title: Java
		
		The Java example uses the `sentineldb-java-client <https://github.com/LogSentinel/sentineldb-java-client/>`_ 
		
		.. code-block:: java
		
			SentinelDBClient client = SentinelDBClientBuilder.create(orgId, secret).build();

			List<BatchRequestItem> batch = new ArrayList<>();

			Map<String, String> attributes = new HashMap<>();
			attributes.put("firstName", "John");
			attributes.put("lastName", "Smith");
			attributes.put("city", "London");
			BatchRequestItem batchRequestItem = new BatchRequestItem();
			batchRequestItem.setProperties(attributes);
			batchRequestItem.setEntityType(BatchRequestItem.EntityTypeEnum.RECORD);
			batch.add(batchRequestItem);

			attributes = new HashMap<>();
			attributes.put("firstName", "Jane");
			attributes.put("lastName", "Smith");
			attributes.put("city", "Dublin");

			batchRequestItem = new BatchRequestItem();
			batchRequestItem.setProperties(attributes);
			batchRequestItem.setEntityType(BatchRequestItem.EntityTypeEnum.RECORD);
			batch.add(batchRequestItem);

			client.getBatchApi().batchInsert(batch, datastoreId);
			
	.. tab-container:: php
		:title: PHP
		
		.. code-block:: php
		
			$data = <<<EOT
			[{
			  "entityType": "RECORD",
			  "properties": {
				"name": "Record Name",
				"details": "Some details",
				"code": "LDR354"
			  }
			},
			{
			  "entityType": "RECORD",
			  "properties": {
				"name": "Another Record Name",
				"details": "Some other details",
				"code": "HFS322"
			  }
			}]
			EOT;
			
			$curl = curl_init();
			curl_setopt($curl, CURLOPT_POST, 1);
			curl_setopt($curl, CURLOPT_POSTFIELDS, $data);
			
			curl_setopt($curl, CURLOPT_URL, 'https://api.db.logsentinel.com/api/batch/insert/' + datastoreId );
			curl_setopt($curl, CURLOPT_HTTPHEADER, array(
				'Content-Type: application/json'
			));
			curl_setopt($curl, CURLOPT_RETURNTRANSFER, 1);
			curl_setopt($curl, CURLOPT_HTTPAUTH, CURLAUTH_BASIC);
			curl_setopt($curl, CURLOPT_USERPWD, $ORG_ID . ":" . $SECRET);
			
			// EXECUTE:
			$result = curl_exec($curl);
			
	.. tab-container:: Python
		:title: Python

		.. code-block:: python
		
			import requests
		
			url = 'https://api.db.logsentinel.com/api/batch/insert/' + datastoreId;
			data = '''[{
			  "entityType": "RECORD",
			  "properties": {
				"name": "Record Name",
				"details": "Some details",
				"code": "LDR354"
			  }
			},
			{
			  "entityType": "RECORD",
			  "properties": {
				"name": "Another Record Name",
				"details": "Some other details",
				"code": "HFS322"
			  }
			}]
			'''

			response = requests.post(url, auth=HTTPBasicAuth(orgId, secret), data=data, headers={"Content-Type": "application/json"})
			
	.. tab-container:: nodejs
		:title: Node.js

		.. code-block:: javascript
		
			var https = require('https');
			var data = JSON.stringify([{
			  "entityType": "RECORD",
			  "properties": {
				"name": "Record Name",
				"details": "Some details",
				"code": "LDR354"
			  }
			},
			{
			  "entityType": "RECORD",
			  "properties": {
				"name": "Another Record Name",
				"details": "Some other details",
				"code": "HFS322"
			  }
			}]);

			var auth = 'Basic ' + Buffer.from(ORG_ID + ':' + ORG_SECRET).toString('base64')

			var options = {
			  host: 'api.db.logsentinel.com',
			  path: '/api/batch/insert/' + datastoreId,
			  method: 'POST',
			  headers: {
				'Content-Type': 'application/json; charset=utf-8',
				'Content-Length': data.length
				'Authorization': auth;
			  }
			};

			var req = https.request(options, function(res) {
			  var result = JSON.parse(response.body)
			  //...
			});

			req.write(data);
			req.end();
			
			
