****************
Local Deployment
****************

.. NOTE::

   This part of documentation is currently **under development**.

.. IMPORTANT::

   This deployment is inteded only for testing purposes and should not serve for real production use. If you want to provide VODAN-in-a-Box to others, visit :ref:`production-deployment`.

Requirements
============

- Docker Engine version 19.03

Setup
=====

1. Download or ``git clone`` repository https://github.com/VODAN-Tech/vodan-deployment-basic locally
2. Change working directory ``cd vodan-deployment-basic``
3. Run ``docker-compose up -d``

Persistence
-----------

Changing ports
--------------

If you need to change ports because you already use those for other services, you just need to adjust the mappings in ``docker-compose.yml`` file. For example, if you want to access FDP on other port than ``8081`` change the mapping ``8081:80`` to something else, e.g., ``8881:80``.

Usage
=====

When VODAN-in-a-Box is running, you can access the following services:

- http://localhost:8080 - DSW
- http://localhost:8081 - FDP
- http://localhost:8082 - BlazeGraph
- http://localhost:27017 - MongoDB
- http://localhost:3000 - DSW API

For both DSW and FDP, you can use default admin account ``albert.einstein@example.com`` with password ``password``. BlazeGraph and MongoDB are by default without authentication.

Notes
=====

For more information about docker-compose and its options, visit `Docker documentation <https://docs.docker.com/compose/>`_.

Various advanced deployment options of FAIR Data Point are well-described in `FAIR Data Point Reference Implementation Documentation <https://fairdatapoint.readthedocs.io>`_.

The main difference with respect to the :ref:`production-deployment` is the absence of proxy and certificates, with opened ports directly instead.
