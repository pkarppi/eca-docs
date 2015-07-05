
User data interface
*******************

Data returned from the API looks like this:

::

  {
    "username": "123abc",
    "first_name": "Teppo",
    "last_name": "Testaaja",
    "roles": [
      {
        "school": "17392",
        "role": "teacher",
        "group": "7A"
      },
      {
        "school": "17392",
        "role": "teacher",
        "group": "7B"
      }
    ]
    "attributes": [
      {
        "attribute1_id": "attribute1_data",
        "attribute2_id": "attribute2_data"
      }
    ]
  }

User can have multiple roles, and also multiple roles in one school.
As the RoleDB tries to model the real situation where one user can be
teacher and student in different shools this has to be here.
It is also possible to decide that only one role object per user
is acceptable when the data is imported to the database.

General fields:

username
  This is the OID. It follows the OID specification.

Fields in the roles dict are defined as follows:

school
  Official school ID.

role
  Either "teacher" or "student".

group
  The class or group for the user.

In addition to role data custom attributes can be added at runtime. These are installation specific and defined in the database.

Authentication to the API
=========================

Authentication to the API is based on tokens. You should send Authorization: Token abcd1234 header.

For example::

  curl -H "Authorization: Token 9c5d6df27105387b586286b06684ac2dcdbf09d3"  http://foo.example.com/api/1/user/

Attribute query
===============

The attribute query endpoint is meant to be used by the Auth Proxy to query for the attributes of single user.

The endpoint is ``/api/1/user?name=value`` which can be queried for the attributes. The result is JSON dict of data.

Query is made by GET parameters. Only one parameter is allowed. ``Not found`` is returned if:

* the parameter name is not recognized
* multiple results would be returned (only one result is allowed)
* no parameters are specified

In the query ``name`` is the parameter name used to filter users from the database. Name of the parameter is defined when new auth
sources are registered to the IdP. Name of the parameter can contain only a-z chars.
The list of valid filter names is available from IdP admins.
The value for the filter parameter is UTF-8 as urlencoded string.

User search query
-----------------

User objects can be searched by ``school``, ``group`` and ``username`` attributes.

Example query: ``/api/1/user/?school=Keskustan%20koulu&group=7A``

This returns all matches.



User data query
===============

This endpoint is used by the Auth Proxy.

The endpoint ``/api/1/user?name=value`` can be queried for the attributes. The result is JSON dict of data.

Query is made by GET parameters. Only one parameter is allowed. ``Not found`` is returned if:

  * the parameter name is not recognized
  * multiple results would be returned (only one result is allowed)
  * no parameters are specified

In the query ``name`` is the parameter name used to filter users from the database.
Name of the parameter is defined when new auth sources are registered to the IdP.
Name of the parameter can contain only a-z characters.
The value for the filter parameter is UTF-8 as urlencoded string.

For example, query ``/api/1/user?attribute1_id=attribute1_data`` would give the following response::

  {
    "username": "123abc",
    "first_name": "Teppo",
    "last_name": "Testaaja",
    "roles": [
      {
        "school": "17392",
        "role": "teacher",
        "group": "7A"
      }
    ]
    "attributes": [
      {
        "attribute1_id": "attribute1_data",
        "attribute2_id": "attribute2_data"
      }
    ]
  }

User can have multiple roles, and also multiple roles in one school. As the RoleDB tries to model the real situation
where one user can be teacher and student in different shools this has to be here. It is also possible to decide that
only one role object per user is acceptable when the data is imported to the database.

General fields:

username
  This is the OID. It follows the OID specification.

Fields in the ``roles`` dict are defined as follows:

school
  Official school ID.

role
  Either ``"teacher"`` or ``"student"``.

group
  The class or group for the user.

