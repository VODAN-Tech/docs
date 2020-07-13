.. _local-deployment:

****************
Local Deployment
****************

.. IMPORTANT::

   This deployment is intended only for testing and demonstration purposes and should not serve for real production use. If you want to provide *VODAN in a Box* as a service, visit :ref:`production-deployment`.

Requirements
============

- `Docker Engine <https://docs.docker.com/get-docker/>`_ version 19.03 (or higher)
- `Docker Compose <https://docs.docker.com/compose/install/>`_ version 1.25 (or higher)

Setup
=====

1. `Download <https://github.com/VODAN-Tech/vodan-deployment-basic/archive/master.zip>`_ or ``git clone`` repository https://github.com/VODAN-Tech/vodan-deployment-basic locally
2. Change working directory to the root folder ``vodan-deployment-basic``
3. Use `docker-compose <https://docs.docker.com/compose/>`_ to start *VODAN in a Box*


.. code-block:: shell

   git clone https://github.com/VODAN-Tech/vodan-deployment-basic.git
   cd vodan-deployment-basic
   docker-compose up -d

For additional configuration options, see :ref:`configuration`.

Usage
=====

When *VODAN in a Box* is running, you can access the following services:

- http://localhost:8080 - CRF Wizard (DSW)
- http://localhost:8081 - FAIR Data Point (FDP)
- http://localhost:8085/blazegraph - `BlazeGraph <https://blazegraph.com>`_
- http://localhost:27017 - MongoDB (for MongoDB clients)
- http://localhost:3000 - CRF Wizard API

For both CRF Wizard and FDP, you can use default admin account ``albert.einstein@example.com`` with password ``password``. BlazeGraph and MongoDB are without any authentication.

- To start *VODAN in a Box*, use ``docker-compose up -d`` in the root directory.
- To stop *VODAN in a Box*, use ``docker-compose down`` in the root directory.
- To restart *VODAN in a Box*, use first ``docker-compose down`` and then ``docker-compose up -d`` again.
- To see running services of *VODAN in a Box* and their status, use ``docker-compose ps``.
- For debugging and investigating logs, use ``docker-compose logs`` (or ``docker-compose logs -f``).

Optionally, you can also use separate `AllegroGraph <https://franz.com/agraph/support/documentation/current/agraph-introduction.html>`_ for submitted CRF data. To do that, simply uncomment ``agraph`` section in ``docker-compose.yml`` and update ``submission-service/config.yml``. Then, you will be able to access it on http://localhost:10035. Of course, you can similarly set any other triple store of your choice.

Update
======

1. Stop *VODAN in a Box*
2. Overwrite configurations and ``docker-compose.yml`` or simply ``git pull``
3. Start *VODAN in a Box* again


From root directory of ``vodan-deployment-basic``:

.. code-block:: shell

   docker-compose down
   git pull
   docker-compose up -d


Notes
=====

For more information about docker-compose and its options, visit `Docker documentation <https://docs.docker.com/compose/>`_.

Various advanced deployment options of FAIR Data Point are well-described in `FAIR Data Point Reference Implementation Documentation <https://fairdatapoint.readthedocs.io>`_.

The main difference with respect to the :ref:`production-deployment` is the absence of proxy and certificates, with opened ports directly instead.
