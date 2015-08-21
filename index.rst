
Educloud Alliance Technical Documentation
*****************************************

Educloud Alliance is creating a standard which defines how different service providers
can communicate and share information.

The goals and principles of the standard are:

* Always think about the user first. The standard must be making their life easier.
* Create documented and open interfaces.
* Use as much existing documentation, interfaces, and field tested technology as possible.
* All and everything in :term:`ECA` must be open and free for everybody to use.
* Create reference implementation of the standard.
* Each service can be maintained by different entity and they can have different goals
* Make it possible for every service to connect to all other services in the ecosystem
* Create a standard which as many as possible is believing.

And some open questions:

* What is the top level goal why :term:`ECA` is creating a standard?
* What it means to create a standard?
* What we try to standardize?
* Who is the target audience for the standard?

.. image:: standard.png

`RFC 2119`__ is used to define common vocabulary for requirements.

__ https://www.ietf.org/rfc/rfc2119.txt

Stories
=======

The standard is modeled based on the first principle about the user coming first.
A story from the actors point of view explains how the standard is affecting
the life of the actor, what benefits there are, and how the standard is affecting
the user interface and user experience.

For user the whole system should look and feel coherent and there should not be
inconsistencies or places where the user does not know where in the overall
system she currently is.


Services
========

The standard is based on a service oriented architecture where functionality
is split to services. The services defined in the standard are implementing
the stories defined in the standard. The standard is accompanied by reference
implementation which shows in practice how the standard is meant to be working.
The reference implementation is not meant to be production system and it is
not designed as such.

.. image:: services.png

User authentication, identification and profile data
----------------------------------------------------

All other services need to know something about the user. Different services
need different data about the user, but all of them need to authenticate and/or
identify the user.

:doc:`Auth Source <auth/index>`
  Authenticates the user when the user wants to open a session in one of the
  services. Auth Sources are handled by the Auth Proxy.

:doc:`Auth Proxy <auth/index>`
  Common interface for services to use different Auth Sources.
  Provides single sign-on for services.

:doc:`Connector <connector/index>`
  Connects user authentication source and user identity together.
  This makes it possible for the user to identify with multiple
  authentication sources and still have only one identity.
  Only the authentication source knows the credentials for the user.

:doc:`Data <data/index>`
  Common source of user data to all other services.
  Mainly used by the connector to query users and store
  the connection between authentication source and user identity.

Learning material
-----------------

Learning material is produced by the :term:`CMS` and used in the :term:`LMS`.

:doc:`Bazaar <bazaar/index>`
  Service which lets the user to browse and buy material from :term:`CMS` to :term:`LMS`.


Interfaces
==========

All communication between services must be happen thru interfaces which are
defined in the standard. Interfaces should be based on existing technology
which is already widely used. Interfaces should be easy to understand and
implement by all parties.

* What interfaces are needed for achieving the goals and the standard?
* What level are the interfaces described?

:doc:`Authentication attributes study <auth/auth_study>`, and first proposal for
authentication attributes.

.. image:: bus.png

:doc:`Auth IF <auth/interface>`
  User authentication is done by common interface.
  The auth system has :term:`SP` and :term:`IdP` components.

:doc:`Data IF <data/interface>`
  Data Service provides an interface to query for user data from Data Providers.

:doc:`LMS IF <bazaar/interface>`
  Between :term:`Bazaar` and :term:`LMS`.

:doc:`CMS IF <bazaar/interface>`
  Between term:`Bazaar` and :term:`CMS`.


Infrastructure
==============

The standard would not be complete without defining how the system as a whole
is working and how the reference implementation is built. The production
system can be different.

Contributions
=============


Read more about :doc:`contributions <contributions>`.

