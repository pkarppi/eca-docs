
Connector Service
*****************

The Connector Service is used for adding authentication methods for users
holding student ID ("oppija ID" in Finnish, OID for short) in the system.

When new user is invited to the system as Invitee.
The invitation happens by the Invitator.
Invitator must be existing user in the system.

Each user can have multiple authentication methods in use from the set of supported methods.

The Connector Service is using Auth Proxy to authenticate users.
The results are stored in the Data Service.

The Connector Service does not have any interfaces. It is used be the users with a browser.


