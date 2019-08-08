SQL Support
===========
SentinelDB supports a subset of the SQL syntax. Below are a few examples of how to use SQL queries. They are sent to a special SQL API endpoint, or can be used in the dashboard.

Select statements
*****************

Example
--------
.. code-block:: sql

 SELECT *, foo, bar
 FROM "3aa42837-6de4-4203-a8c9-b9696d4408eb".records.Order
 WHERE foo=aaa AND bar=bbb
 ORDER BY foo DESC, bar
 LIMIT 10
 

Explanation
-----------

* The ``SELECT`` clause can contain * and any other literal. If object is record special select words are ‘ *‘ , ‘id’, ‘ownerid’, ‘updated’. If object is user special select words are ‘‘, ‘id’, ‘updated’, ‘username’, ‘password’, ’email’, ‘status’‘*’ extracts the whole user/record represented as json.these words extract fields of the object itself and not its body that comes from the client.All other select words extract info from the body of the object. As it is json dot notation can be used. Example: ``foo.bar.bass``
* The ``FROM`` clause must be in the form ``.users`` or ``.records``. The default type name is None (when no type is provided None is used)
* The ``WHERE`` clause supports list of conditions separated by AND word. Each condition is in the form field=value. As in SELECT clause special words conditions are checked against the object fields and not against the body fields.Other fields conditions are checked against object’s body only if they are made searchable (with search schema)
* The ``ORDER BY`` clause can contain list of fields separated by ‘,’ ASC and DESC sorting is allowed for every field
* The ``LIMIT`` clause expects numeric value for limiting results

Count
-----------

Count aggregation can be used in the ``SELECT`` clause . Single count(*) is optimal and returns the count without limits (of course filtering by where clause is applied). If there is count by field ( ``SELECT count(foo) FROM ...`` ) only not NULL values are counted and there is a limit of 10000 entries. Multiple counts can be selected ( ``SELECT count(foo), count(bar) FROM ...`` ) If count aggregation is mixed with normal field (``SELECT count(foo), bar, bazz FROM ...``), counts work as normal and other fields have random value from selected rows. This behaviour is similar to MySQL database. 

Update statements
*****************

Example
--------
.. code-block:: sql

 UPDATE "3aa42837-6de4-4203-a8c9-b9696d4408eb".records.Order
    SET 'some.thing'=something
    WHERE id='953df9c1-5917-4ff0-a5e0-604c07071324'
 

Explanation
-----------

* The ``UPDATE`` clause is the same as FROM clause from Select statement
* The ``SET`` clause contains list of key-value pairs separated by ``,``. It uses the same special words as the select clause. If fields are not special dot notation creates new json structure in the body ``SET 'foo.bar'=green`` results in ``{"foo":{"bar" : "green"}}``
* The ``WHERE`` clause is the same as in the Select statement. If it contains only id, it is fast, otherwise all matching objects are matched so it might be slower.

Delete statements
*****************

Example
-------
.. code-block:: sql

 DELETE "3aa42837-6de4-4203-a8c9-b9696d4408eb".records.Order
     WHERE id='953df9c1-5917-4ff0-a5e0-604c07071324'
 

Explanation
-----------

* The ``DELETE`` clause is the same as in the UPDATE and FROM cases
* The ``WHERE`` is the same as in the UPDATE statement including fast processing by id

Insert statements
*****************

Example
-------
.. code-block:: sql

 INSERT INTO "3aa42837-6de4-4203-a8c9-b9696d4408eb".records.Order
     (ownerId, foo, bar)
     VALUES ('4aa42837-6de4-4203-a8c9-b9696d4408eb', green, yellow)
 

Explanation
-----------

* The ``INSERT`` clause is the same like UPDATE and FROM + added list of field names. Special field names are the same as in other clauses
* The ``VALUES`` caluse contains the values of the fields in the INSERT clause

Common syntax
*************
When field or value contains ``-``, ``.`` or other special characters it must be quoted with ``"`` or ``'``. In other cases quotes are not mandatory, but can be used.

Prepared statements
*******************

All SQL API endpoints (select, update, delete, insert) support prepared statements. To keep APIs stateless, parameter values are always expected with the query itself. The number of ``?`` and the number of params must be the same.

Examples:

Select
------
.. code:: text

 {"query":"select  id,foo,bar, updated, ownerId from '3aa42837-6de4-4203-a8c9-b9696d4408eb'.records.Order where id=?  and updated > ?",
  "params":["953df9c1-5917-4ff0-a5e0-604c07071324", ""]
 }
 

Update
------
.. code:: text

 {"query":"update '3aa42837-6de4-4203-a8c9-b9696d4408eb'.records.Order set 'some.thing'=? where id=?",
  "params" :["foo","5095dfb5-5bbf-4a15-8d7d-12d66a3a4f2c"]
 }
 

Delete
------
.. code:: text

 {"query":"delete '3aa42837-6de4-4203-a8c9-b9696d4408eb'.records.Order where id=?",
  "params":["5095dfb5-5bbf-4a15-8d7d-12d66a3a4f2c"]
 }
 

Insert
------
.. code:: text

 {"query":"insert into '3aa42837-6de4-4203-a8c9-b9696d4408eb'.records.Order (foo, bar, 'bass.bar') values (?,?,?) ",
  "params":["pink", "green", "yellow"]
 }


