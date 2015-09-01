
Edustore Distributed API
=========================

draft v0.1

.. contents::


Signing and Authentication 
==========================

There are two supported methods:

1. Header HMAC256 AUTH token in headers and JSON body (Similar: http://docs.aws.amazon.com/AmazonS3/latest/dev/RESTAuthentication.html)
2. GET/POST request with params=<json>&authentication=<client_id:hash>

1 Adding material
==============================


1.1 Searching and adding material to the LMS
--------------------------------------------

Learning material can be added with three use methods:

1. User open Edustore repository directly.
2. | User select materials from the Edustore repository to the LMS by using: 
   | - Drag and Drop
   | - Window to window
   | NOTE: These material selection methods cannot be used from mobile devices (Windows Phone, iOS, Android). 
3. User selects material from integrated embedded Edustore JS module, which can be included to any LMS. This method uses drag and drop.


API workflow for selecting material from Edustore directly:

1. LMS makes a JSON POST request to the Edustore with params from table 1, using header-based auth.
2. Edustore replies with unique URL and LMS forwards user to the unique url.  
3. User selects learning material from the Edustore and Edustore forwards user back to the add_resource_callback_url, with resource details(ie. name, description).

Params table 1:

==========================  ================================================ ======= =========
Param                       Description                                      Type    Required
==========================  ================================================ ======= =========
first_name                  First name of user.                              alpha   required
last_name                   Last name of user.                               alpha   required
email                       Email of user.                                   email   optional
user_id                     User id from                                     alpha   required
context_id                  Course or page identifier. Must be unique.       alpha   required
context_title               Course or page title                             alpha   required
role                        Role (enum student, teacher, admin)              alpha   required
school                      Name of the school                               alpha   required
school_id                   School's identifier in the national level.       alpha   required
city                        City's name                                      alpha   required
city_id                     City's unique identifier in the national level   alpha   required
oid                         User oid                                         alpha   optional
add_resource_callback_url   LMS callback URL, for adding material to the LMS url     optional
cancel_url                  Forwarding user back to  LMS                     url     optional
==========================  ================================================ ======= =========


1.1 Material drag and drop
--------------------------

LMS should implement drop functionality directly and parse links with "edustore.fi/<base64 encoded json>.
Base64 encoded JSON has basic information about the resource: name, description and uid.


2 Viewing material 
===================

Use case:
Person wants to view selected material/resource. 

API workflow:

1. LMS makes a JSON POST request to the Edustore with params from table 2, using header-based auth.
2. Edustore replies with unique url and LMS forwards user to the unique url.  
3. User is forwarded to the resource(if resource is internal pdf, videos, epub, scorms) or forwarded to the publisher system. Publisher should implement 2.2 part of the specs.


Edustore integrated player?

1. Resource is located at the Edustore server(pdf, epub, scorm) -> resource is shown using integrated player
2. Resource is located at publishers server step 2.2 			-> user is forwarded to the publishers server

2.1 LMS to Edustore
--------------------

Workflow:

1. LMS makes a JSON POST request to the Edustore with params from table 2, using header-base authentication.
2. Edustore replies with unique url and LMS forwards user to the unique url.  
3. Edustore displays or forwards user to the selected material.

Edustore does all the license and logging functionality.

Params table 2:

==========================  ================================================  =======  =========
Param                       Description                                       Type     Required
==========================  ================================================  =======  =========
first_name                  First name of user.                                        required
last_name                   Last name of user.                                         required
email                       Email of user.                                             optional
user_id                     User id from                                               required
context_id                  Course or page identifier. Must be unique.                 required
context_title               Course or page title                                       required
role                        Role (enum student, teacher, admin)                        required
school                      Name of the school                                         required
school_id                   School's identifier in the national level.                 required
city                        City's name                                                required
city_id                     City's unique identifier in the national level             required
oid                         User oid                                                   optional
resource_uid                Resource Edustore UID                            
return_url                  Forwarding user back to the LMS                            optional
==========================  ================================================  =======  =========


2.2 Edustore to CMS
-------------------

Edustore forwards user to the publisher CMS system after authentication and license checking is done. 

Use case:
Forwarding user from Edustore to the CMS.

API workflow:
1. Edustore generates json payload with params from table 3, using GET/POST authentication.
2. Edustore forwards user to the publisher viewing API with 

Params table 3: CMS Viewing API parameters

==========================  ==================================================================================  ======= ========
Param                       Description                                                                         Type    Required
==========================  ==================================================================================  ======= ========
first_name                  First name of user.
last_name                   Last name of user
email                       Email of user.
user_id                     Edustore user id 
context_id                  Edustore unique context id
context_title               Course or page title.
role                        Role (enum student, teacher, admin)
school                      Name of the school
school_id                   School's identifier in the national level.
city                        City's name
city_id                     City's unique identifier at the national level
oid                         OID https://confluence.csc.fi/download/attachments/8127300/Oppijanumero+ja+OID.pdf 
resource_uid                Edustore resource uid   
publisher_material_id       Edustore publisher unique identifier
resource_url                Edustore resource url
demo_mode                   demo mode (0, 1)
preview                     preview mode(0, 1) 
return_url                  url for forwarding user back to LMS
token_date                  When the token was generated. Datetime in GMT time. Datetime format is ISO 8601.
==========================  ==================================================================================  ======= ========




3 Central Learning Record Store 
================================

3.1 xAPI
--------



@TODO




3 Edustore CMS API
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
