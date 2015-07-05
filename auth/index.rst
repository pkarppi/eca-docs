
Authentication
**************

.. toctree::

  interface

EduCloud Auth's central component is called Auth Proxy (IdP)
that is the main gateway between Service Providers (SPs)
and Authentication Providers (APs).
Proxy IdP hides the varying technologies of multiple APs behind one standard interface from SPs' perspective:
regardless of the AP, Proxy IdP returns the authenticated user's OID ("oppija ID" in Finnish) for the SP.

The SPs can request authentication and attributes from Proxy IdP solely using the (front-channel) SAML 2.0 Web SSO Profile.

User attributes can be queried from the Data Service.

<Image of relationships between the services>

<Image of sequence diagram of authentication flow>

Data as SAML attributes
=======================

Data coming from Data Service should be directly visible to all service providers as SAML attributes.
There are two attributes:

educloud.oid
  This is same as the username field in the API.

educloud.data
  Contains whole JSON document coming from the API. It is base64 encoded.


