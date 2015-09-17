
Bazaar API
=========================

draft v0.2

.. contents::


JSON
====

JSON Character encoding is always UTF-8. 

Signing and Authentication 
==========================

There are two authencation types:

1. Header HMAC256 AUTH token in headers and JSON body (Similar: http://docs.aws.amazon.com/AmazonS3/latest/dev/RESTAuthentication.html)

JSON body is encoded using UTF-8.

Example authenticated Edustore request:: 
   
   POST /api/v1/cms/browse HTTP/1.1
   Host: bazaar.gov
   Date: Mon, 3 Jul 2015 12:00:52 +0300
   Authentication: EDUSTORE CLIENTID:d8a928b2043db77e340b523547bf16cb4aa483f0645fe0a290ed1f20aab76257

   {"first_name":"Teppo","last_name":"Testaaja","email":"Teppo","user_id":123,"context_id":123,"context_title":"DETAILS","role":"student","school":"Koulu","school_id":1235,"city":"Helsinki","city_id":"0123456-7","oid":null,"add_resource_callback_url":"","cancel_callback_url":""}

2. GET request with params=<json>&authentication=<EDUSTORE client_id:hash>

JSON is encoded using https://www.ietf.org/rfc/rfc4627.txt. All parameters are urlencoded.

Example forwarding GET request::

  GET /cms/api/?params=%7B%22first_name%22%3A%22Teppo%22%2C%22last_name%22%3A%22Testaaja%22%2C%22email%22%3A%22Teppo%22%2C%22user_id%22%3A123%2C%22context_i  d%22%3A123%2C%22context_title%22%3A%22DETAILS%22%2C%22role%22%3A%22student%22%2C%22school%22%3A%22Koulu%22%2C%22school_id%22%3A1235%2C%22city%22%3A%22Helsinki%22%2  C%22city_id%22%3A%220123456-7%22%2C%22resource_uid%22%3A%22123-123-13-123%22%2C%22return_url%22%3A%22https%3A%5C%2F%5C%2Flms.gov%5C%2F%22%2C%22created%22%3A%222015  -9-17T15%3A46%3A04%2B03%3A00%22%7D&authentication=EDUSTORE+CLIENTID%3Ad8a928b2043db77e340b523547bf16cb4aa483f0645fe0a290ed1f20aab76257 HTTP/1.1
  Host: publisher.com
  Date: Mon, 3 Jul 2015 12:00:52 +0300
  


1 Adding material
==============================


1.1 Searching and adding material to the LMS
--------------------------------------------

Learning material can be added with three use methods:

1. User open Bazaar repository directly.
2. | User select materials from the Bazaar repository to the LMS by using: 
   | - Drag and Drop
   | - Window to window
   | NOTE: These material selection methods cannot be used from mobile devices (Windows Phone, iOS, Android). 
3. User selects material from integrated embedded Bazaar JS module, which can be included to any LMS. This method uses drag and drop.


API workflow for selecting material from Bazaar directly:

1. LMS makes a JSON POST request to the Bazaar with params from table 1, using header-based auth.
2. Bazaar replies with unique URL and LMS forwards user to the unique url.  
3. User selects learning material from the Bazaar and Bazaar forwards user back to the add_resource_callback_url, with resource details(ie. name, description).

Params table 1:

==========================  ================================================ ============ =========
Param                       Description                                      Type         Required
==========================  ================================================ ============ =========
first_name                  First name of user.                              alpha 1-255   required
last_name                   Last name of user.                               alpha 1-255   required
email                       Email of user.                                   email         optional
user_id                     User id from                                     alpha 1-128   required
context_id                  Course or page identifier. Must be unique.       alpha 1-128   required
context_title               Course or page title                             alpha 1-128   required
role                        Role (enum student, teacher, admin)              enum          required
school                      Name of the school                               alpha 1-128   required
school_id                   School's identifier in the national level.       alpha 5-10    required
city                        City's name                                      alpha 1-64    required
city_id                     City's unique identifier in the national level   alpha 1-10    required
oid                         User oid                                         alpha 1-32    optional
add_resource_callback_url   LMS callback URL, for adding material to the LMS url           optional
cancel_url                  Forwarding user back to  LMS                     url           optional
==========================  ================================================ ============ =========



1.1 Material drag and drop
--------------------------

LMS should implement drop functionality directly and parse links with "Bazaar.fi/<base64 encoded json>.
Base64 encoded JSON has basic information about the resource: name, description and uid.


2 Viewing material 
===================

Use case:
Person wants to view selected material/resource. 

API workflow:

1. LMS makes a JSON POST request to the Bazaar with params from table 2, using header-based auth.
2. Bazaar replies with unique url and LMS forwards user to the unique url.  
3. User is forwarded to the the publisher system. Publisher should implement 2.2 part of the specs.


2.1 LMS to Bazaar
--------------------

Workflow:

1. LMS makes a JSON POST request to the Bazaar with params from table 2, using header-base authentication.
2. Bazaar replies with unique url and LMS forwards user to the unique url.  
3. Bazaar displays or forwards user to the selected material.

Bazaar does all the license and logging functionality.

Params table 2(Bazaar CMS Interface):

==========================  ================================================  ============  =========
Param                       Description                                       Type           Required
==========================  ================================================  ============  =========
first_name                  First name of user.                               alpha 1-255   required
last_name                   Last name of user.                                alpha 1-255   required
email                       Email of user.                                    email         optional
user_id                     User id from LMS                                  alpha 1-255   required
context_id                  Course or page identifier. Must be unique.        alpha 1-128   required
context_title               Course or page title                              alpha 1-128   required
role                        Role (enum student, teacher, admin)               enum          required
school                      Name of the school                                alpha 1-128   required
school_id                   School's identifier in the national level.        alpha 5-10    required
city                        City's name                                       alpha 1-64    required
city_id                     City's unique identifier in the national level    alpha 1-10    required
oid                         User oid                                          alpha 1-32    optional
resource_uid                Resource Bazaar UID                               128bit UID    required
return_url                  Forwarding user back to the LMS                   url           optional
==========================  ================================================  ============  =========


2.2 Bazaar to CMS
-------------------

Bazaar forwards user to the publisher CMS system after authentication and license checking is done. 

Use case:
Forwarding user from Bazaar to the CMS.

API workflow:
1. Bazaar generates json payload with params from table 3, using GET authentication.
2. Bazaar forwards user to the publisher viewing API with 

Params table 3: Bazaar CMS Viewing API parameters

==========================  ==================================================================================  ============ ========
Param                       Description                                                                         Type         Required
==========================  ==================================================================================  ============ ========
first_name                  First name of user.                                                                 alpha 1-255  required
last_name                   Last name of user.                                                                  alpha 1-255  required       
email                       Email of user.                                                                      email        optional 
user_id                     Bazaar user id                                                                      bigint       required
context_id                  Bazaar unique context id                                                            bigint       required                                               
context_title               Course or page title.                                                               alpha 1-128  required
role                        Role (enum student, teacher, admin)                                                 enum         required
school                      Name of the school                                                                  alpha 1-128  required
school_id                   School's identifier in the national level.                                          alpha 1-12   required
city                        City's name                                                                         alpha 1-64   required
city_id                     City's unique identifier at the national level                                      alpha 5-10   required  
oid                         OID https://confluence.csc.fi/download/attachments/8127300/Oppijanumero+ja+OID.pdf  alpha 1-32   required
resource_uid                Bazaar resource uid                                                                 128bit UID   required
publisher_material_id       Bazaar publisher unique identifier                                                  alpha 1-128  required
resource_url                Bazaar resource url                                                                 url          required
demo_mode                   demo mode (0, 1)                                                                    int          required 
preview                     preview mode(0, 1)                                                                  int          required
return_url                  url for forwarding user back to LMS                                                 url          required
token_date                  When the token was generated. Datetime in GMT time. Datetime format is ISO 8601.    datetime     required
==========================  ==================================================================================  ============ ========




3 Central Learning Record Store 
================================

3.1 xAPI
--------



@TODO




3 Bazaar CMS API
==================

@TODO

4 Logging API
=============

@TODO


5 License API
=============

@TODO


Glossary of Terms
=================

@TODO

API = Application Programming Interface

URL = Uniform Resource Identifier

LMS = Learning Management System

CMS = Content Management System

JSON = JavaScript Object Notation

JS = JavaScript
