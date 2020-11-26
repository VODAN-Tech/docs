.. _production-deployment:

*********************
Production Deployment
*********************

.. IMPORTANT::

   This deployment is intended for production use. If you want to just test *VODAN in a Box* locally, visit :ref:`local-deployment`.

Requirements
============

- `Docker Engine <https://docs.docker.com/get-docker/>`_ version 19.03 (or higher)
- `Docker Compose <https://docs.docker.com/compose/install/>`_ version 1.25 (or higher)
- Domain and DNS records set for providing *VODAN in a Box*:

  - ``dsw.your-domain.tld`` - for CRF Wizard (DSW)
  - ``api.dsw.your-domain.tld`` - for CRF Wizard API (DSW API)
  - ``fdp.your-domain.tld`` - for FAIR Data Point
  - ``sparql.your-domain.tld`` - for Triple Store (CRF data)
  
- `certbot <https://certbot.eff.org>`_

Setup
=====

Get VODAN in a Box
------------------

`Download <https://github.com/VODAN-Tech/vodan-deployment-production/archive/master.zip>`_ or ``git clone`` repository https://github.com/VODAN-Tech/vodan-deployment-production locally.

The folder ``vodan-deployment-production`` we call *VODAN in a Box* root directory. It consists all necessary configuration files and ``docker-compose.yml``.

Configure domains and secrets
-----------------------------

There are several things that you need to configure before running *VODAN in a Box* for production deployment. In files, look for comments marked with ``(!)``:

1. ``server_name`` and ``ssl_certificate`` values in ``proxy/nginx/agraph.conf``, ``proxy/nginx/dsw.conf``, and ``proxy/nginx/fdp.conf`` with your domain names. Those need to have valid DNS records pointing to that server.
2. ``docker-compose.yml`` -  ``API_URL`` (``dsw_client`` service) to your value for ``api.dsw.your-domain.tld``
3. ``dsw-server/application.yml`` - ``clientUrl`` to your value for  ``dsw.your-domain.tld``, then ``secret``, ``serviceToken``, and ``email`` section according to the comments there
4. ``fdp/application.yml`` - ``clientUrl`` to your value for ``fdp.your-domain.tld`` and then , ``persistentUrl``, ``secret``, ``serviceToken``, and ``secret-key`` (JWT)
5. ``allegrograph/agraph.cfg`` - set **strong password** and optionally change username using ``SuperUser`` directive, the same credentials must be configured in ``submission-service/config.yml``

Obtain SSL certificates
-----------------------

Before providing *VODAN in a Box* you need also to get SSL certificates to be able to use HTTPS. We recommend using Let's Encrypt but you can use any other way and change Nginx proxy configuration accordingly.

1. Comment out ``include`` lines at the end of ``proxy/nginx/nginx.conf``
2. Start the proxy service

.. code-block:: shell

  docker-compose up -d proxy

3. Get certificates for your domains:

.. code-block:: shell

  sudo certbot certonly --webroot -w ./proxy/letsencrypt -d dsw.your-domain.tld

.. code-block:: shell

  sudo certbot certonly --webroot -w ./proxy/letsencrypt -d api.dsw.your-domain.tld

.. code-block:: shell

  sudo certbot certonly --webroot -w ./proxy/letsencrypt -d fdp.your-domain.tld

.. code-block:: shell

  sudo certbot certonly --webroot -w ./proxy/letsencrypt -d sparql.your-domain.tld

4. Create certificate file for AllegroGraph (it needs to merge ``cert.pem`` and ``privkey.pem`` obtained by Let's Encrypt into a single file):

.. code-block:: shell

  sudo cat /etc/letsencrypt/live/sparql.your-domain.tld/cert.pem  /etc/letsencrypt/live/sparql.your-domain.tld/privkey.pem > ./allegrograph/cert.pem

5. Stop the proxy service

.. code-block:: shell

  docker-compose down

6. Uncomment lines at the end of ``proxy/nginx/nginx.conf``

7. Set up automatic certificate renewal using cronjob: ``/etc/cron.d/certbot``

.. code-block:: shell

  0 4 * * *   root   perl -e 'sleep int(rand(43200))' && certbot -q renew && docker restart vodan-deployment-production_proxy_1

If getting certificates fail, it can be caused by incorrectly set DNS records. Optionally, verify if Nginx container is running and view its logs. You can use other options to setup certificates renewal according to `Certbot documentation <https://certbot.eff.org/docs/using.html#renewing-certificates>`_. The example above tries to renew certificates every day at 4 AM and then restarts the proxy container. The name of docker container may differ if you do not use the same folder name as we do in this guide.

First start
-----------

1. Start *VODAN in a Box* (and wait a bit until all services start).

.. code-block:: shell

   docker-compose up -d

2. Navigate to ``dsw.your-domain.tld``, login using ``albert.einstein@example.com`` with password ``password`` and change default user accounts with **strong passwords**.
3. In ``sparql.your-domain.tld``, create a repository ``crf`` in catalog ``/`` and create other users with permissions according to your needs (see `AllegroGraph documentation <https://franz.com/agraph/support/documentation/current/managing-users.html#Managing-users-with-AGWebView:-general-comments>`_ for details). For example, create an *anonymous* user with only *read* permissions to catalog */* and repository *crf*.
4. Navigate to ``fdp.your-domain.tld`` and login again as ``albert.einstein@example.com`` and change default user accounts with **strong passwords**.
5. In ``fdp.your-domain.tld``, create and publish catalog, dataset, and distribution representing CRF data based on your use case.
6. Update ``submission-service/config.yml`` with :abbr:`UUID (Universally Unique Identifier)` of your distribution URL from FDP, e.g. from ``https://fdp.vodan.fairdatapoint.org/distribution/3335345b-ee66-4678-ab73-74a4b6ea1bee`` it would be ``3335345b-ee66-4678-ab73-74a4b6ea1bee``. (If you used different than ``crf`` repository name in triple store, change ``sparql-endpoint`` accordingly.)
7. Restart *VODAN in a Box* and wait a bit until all services start up (depending on your hardware, less than a minute).

.. code-block:: shell

   docker-compose down
   docker-compose up -d

8. Verify setup by creating CRF, saving it, creating a report, and submitting a report.

🎉 After this, your *VODAN in a Box* is ready to be used!

To check if everything is working, you can use ``docker-compose logs`` and ``docker-compose ps`` commands.

⚙️ For additional configuration options, see :ref:`configuration`.

Update
======

1. Stop *VODAN in a Box*
2. Overwrite configurations and ``docker-compose.yml`` or simply ``git pull``
3. Check if there are new configuration values to be changed according to your setup (marked with ``(!)`` comments)
4. Start *VODAN in a Box* again


From root directory of ``vodan-deployment-production``:

.. code-block:: shell

   docker-compose down
   git pull
   docker-compose up -d

This may need you to ``git stash`` your changes and then ``git stash pop`` them (and eventually solve git conflicts).

Notes
=====

For more information about docker-compose and its options, visit `Docker documentation <https://docs.docker.com/compose/>`_.

Various advanced deployment options of FAIR Data Point are well-described in `FAIR Data Point Reference Implementation Documentation <https://fairdatapoint.readthedocs.io>`_. Similarly, for more details about DSW which used as CRF Wizard, see `Data Stewardship Wizard documentation <https://docs.ds-wizard.org>`_.

The main difference with respect to the :ref:`local-deployment` is the adding Nginx proxy, certificates, and other additional security.
