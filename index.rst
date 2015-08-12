
Educloud Alliance Technical Documentation
*****************************************

Summary of

* why
* what
* how

Educloud Alliance is creating a standard architecture how different service providers
can communicate and share information. The system consists of mainly interface
descriptions, protocols and overall description how the highlevel system works.

The standard is accompanied by reference implementation which shows in
practice how the system is meant to be working. The reference implementation
is not meant to be production system and it is not designed to such.


Goals
=====

* Always think about the user first. User is the student, pupil, learner. Everything must be making their life easier.
* Create documented and open interfaces to key infrastructure.
* Use as much existing documentation, interfaces, and field tested technology as possible.
* All and everything in :term:`ECA` must be open and free for everybody to use.
* Create reference implementation of all documentation.
* Make it possible for every service to connect to all other services in the ecosystem
* Create a standard which as many as possible is believing.

And some questions:

* What is the top level goal why :term:`ECA` is creating a standard?
* What it means to create a standard?
* What we try to standardize?
* Who is the target audience for the standard?

Architecture
============

* What components the architecture has?
* Which level they are documented?

<Here is an image>

ECA is using a service oriented architecture where each service can be maintained
separatedly. Each service does only one thing.

Services can be grouped to the following groups:

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
  
:doc:`Data <auth/auth_study.rst>`
  Authentication attributes study, and first proposal for authentication attributes.


Learning material
-----------------

Learning material is produced by the :term:`CMS` and used in the :term:`LMS`.

:doc:`Bazaar <bazaar/index>`
  Service which lets the user to browse and buy material from :term:`CMS` to :term:`LMS`.


Interfaces
==========

* What interfaces are needed for achieving the goals and the standard?
* What level are the interfaces described?

<Here is an image>

:doc:`Auth IF <auth/interface>`
  User authentication is done by common interface.
  The auth system has :term:`SP` and :term:`IdP` components.

:doc:`Data IF <data/interface>`
  Data Service provides an interface to query for user data from Data Providers.

:doc:`LMS IF <bazaar/interface>`
  Between :term:`Bazaar` and :term:`LMS`.

:doc:`CMS IF <bazaar/interface>`
  Between term:`Bazaar` and :term:`CMS`.

Contributions
=============


Read more about :doc:`contributions <contributions>`.

