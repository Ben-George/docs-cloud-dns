.. _POST_createZone_v2__account_id__zones_zones:

Create a zone
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code::

    POST /v2/{TENANT_ID}/zones

This operation provisions a new DNS zone, based on the configuration defined
in the request object. 

If the corresponding request cannot be fulfilled because of insufficient or invalid data, 
an ``HTTP 400`` (Bad Request) error response is returned with information about the 
failure in the body of the response. Failures in the validation process are 
non-recoverable and require you to correct the cause of the failure and resend the request.

..  note:: 

    - This operation returns an asynchronous response. For information about how
      asynchronous operations work, see 
      :ref:`Synchronous and asynchronous responses<cdns-dg-synch-asynch>`. 

    - This operation takes a few minutes to become effective on our name servers. For 
      more information about DNS propagation, see :ref:`DNS propagation<cdns-dg-propagation>`. 

If you attempt to create a zone that already exists, the API returns an exception 
saying that the zone already exists.

When a zone is created, and no time-to-live (TTL) value is specified, the minimum value of 
300 seconds is used as the default. When a record is added without a specified TTL, the TTL 
value is shown as empty. When the zone and/or record TTL is supplied by the user, either 
via a create or update operation, the TTL values must be 300 seconds or more.


The following table shows the possible response codes for this operation.

..  note:: 

    When executed, this operation will show the *initial* ``202 Accepted`` response for 
    the asynchronous call and indicate that the task has been accepted for processing. 

+---------+-----------------------+---------------------------------------------+
| Response| Name                  | Description                                 |
| code    |                       |                                             |
+=========+=======================+=============================================+
| 202     | Accepted              | The request was accepted for                |
|         |                       | processing, but the processing has not      |
|         |                       | completed.                                  |
+---------+-----------------------+---------------------------------------------+
| 400     | Bad Request           | The request is missing one or more          |
|         |                       | elements, or the values of some elements    |
|         |                       | are invalid.                                |
+---------+-----------------------+---------------------------------------------+
| 401     | Unauthorized          | You are not authorized to complete this     |
|         |                       | operation. This error can occur if the      |
|         |                       | request is submitted with an invalid        |
|         |                       | authentication token.                       |
+---------+-----------------------+---------------------------------------------+
| 404     | Not Found             | The requested item was not found.           |
+---------+-----------------------+---------------------------------------------+
| 409     | Already Exists        | The item already exists.                    |
+---------+-----------------------+---------------------------------------------+
| 413     | Over Limit            |The request exceeds the rate limit or quota. |
+---------+-----------------------+---------------------------------------------+
| 503     | Service Unavailable   | The service is not available.               |
+---------+-----------------------+---------------------------------------------+

The following table shows the URI parameters for the request.

+-----------------------+---------+---------------------------------------------+
| Name                  | Type    | Description                                 |
+=======================+=========+=============================================+
| ``{TENANT_ID}``       | ​String | The account ID of the account owner.        |
+-----------------------+---------+---------------------------------------------+

The following table shows the body parameters for the request.

+-----------------------+------------+---------------------------------------------+
| Name                  | Type       | Description                                 |
+=======================+============+=============================================+
| ``name``              | ​String    | The name for the zone, which cannot be      |
|                       | (Required) | changed. Must be a valid zone (domain) name.|
+-----------------------+------------+---------------------------------------------+
| ``email``             | ​String    | Email address to use for contacting the zone|
|                       | (Required) | administrator.                              |
+-----------------------+------------+---------------------------------------------+
| ``ttl``               | Integer    | Time-to-live numeric value in seconds. The  |
|                       | (Optional) | minimum value is 300 seconds.               |
+-----------------------+------------+---------------------------------------------+
| ``description``       | ​String    | A description of the zone.                  |
|                       | (Optional) |                                             |
+-----------------------+------------+---------------------------------------------+

**Example: Create a zone, request**

.. code::  

    POST /v2/123456/zones HTTP/1.1
    Host: global.dns.rackspacecloud.com
    Accept: application/json
    Content-Type: application/json

    {
        "name": "example.org.",
        "email": "joe@example.org",
        "ttl": 7200,
        "description": "This is an example zone."
    }

 
**Example: Create a zone, response**

.. code::  

    HTTP/1.1 201 Created
    Content-Type: application/json

    {
        "id": "a86dba58-0043-4cc6-a1bb-69d5e86f3ca3",
        "pool_id": "572ba08c-d929-4c70-8e42-03824bb24ca2",
        "project_id": "123456",
        "name": "example.org.",
        "email": "joe@example.org",
        "ttl": 7200,
        "serial": 1404757531,
        "status": "ACTIVE",
        "description": "This is an example zone.",
        "masters": [],
        "type": "PRIMARY",
        "transferred_at": null,
        "version": 1,
        "created_at": "2014-07-07T18:25:31.275934",
        "updated_at": null,
        "links": {
          "self": "https://global.dns.api.rackspacecloud.com/v2/123456/zones/a86dba58-0043-4cc6-a1bb-69d5e86f3ca3"
        }
    }
