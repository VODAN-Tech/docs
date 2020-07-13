.. _configuration:

**********************
Advanced Configuration
**********************

To work with *VODAN in a Box* you are not required to change anything in the included ``docker-compose.yml`` nor configuration files. For some specific use cases you might want to make some of the following changes.

Persistence
===========

In the basic setup, persistence is assured using mounted folders (bind mounts):

- ``./mongo/data`` - for MongoDB (used by both FDP and CRF Wizard)
- ``./blazegraph`` - for BlazeGraph triple store (used both by FDP and as CRF-in-RDF data storage)

This allows you to easily work with data used by *VODAN in a Box*. For example, you can clear those folders (while it is not running) to start over. In some cases you might want to use `Docker volumes <https://docs.docker.com/storage/volumes/>`_ instead. Using Docker volumes is recommended when using Docker for Windows due to common problems related to mounting Windows folders into Linux containers.

.. code-block:: yaml

   # ...

   mongo:
     image: mongo:4.2.3
     restart: always
     ports:
       - 27017:27017
     environment:
       MONGO_INITDB_DATABASE: wizard
     volumes:
        - mongoData:/data/db  # <- USING DOCKER VOLUME
       - ./mongo/init-mongo.js:/docker-entrypoint-initdb.d/init-mongo.js:ro

   # ...

   blazegraph:
     image: metaphacts/blazegraph-basic:2.2.0-20160908.003514-6
     ports:
       - 8085:8080
     volumes:
       - blazegraphData:/blazegraph-data    # <- USING DOCKER VOLUME

   # ...

   volumes:
     mongoData:
     blazegraphData:


To avoid persistence totally (i.e. all data will be lost after ``docker-compose down``). Just comment out or delete lines related to mounting volumes in ``docker-compose.yml```:

.. code-block:: yaml

   # ...

   mongo:
     image: mongo:4.2.3
     restart: always
     ports:
       - 27017:27017
     environment:
       MONGO_INITDB_DATABASE: wizard
     volumes:
       # - ./mongo/data:/data/db
       - ./mongo/init-mongo.js:/docker-entrypoint-initdb.d/init-mongo.js:ro

   # ...

   blazegraph:
     image: metaphacts/blazegraph-basic:2.2.0-20160908.003514-6
     ports:
       - 8085:8080
     #volumes:
     #  - ./blazegraph:/blazegraph-data

CRF Data Submission
===================

To simplify the setup, *VODAN in a Box* uses the same triple store and the same namespace for both FAIR Data Point data and data of submitted CRFs. You can easily change this behavior using a configuration file ``submission-service/config.yml``. All you need to have is URL of SPARQL endpoint to be used for dat submission. Additionally, if you want to maitain metadata in the FAIR Data Point you need to have a URL to distribution to be updated on submission.

.. code-block:: yaml

   triple-store:
     sparql-endpoint: http://my-triple.store/repository/my-crf-repo/sparql  # <- change to your SPARQL endpoint
     auth:  # <- only if triple store uses auth
       method: BASIC  # <- authentication method: BASIC (default) or DIGEST
       username: usernameToMyTripleStore  # <- change to your triple store username
       password: passwordToMyTripleStore  # <- change to your triple store password
     graph:  # !! do not change this section
       named: true
       type: http://purl.org/vodan/whocovid19crfsemdatamodel/who-covid-19-rapid-crf

   fdp:
     token: a274793046e34a219fd0ea6362fcca61a001500b71724f4c973a017031653c20  # !! do not change this
     distribution: http://fdp_client/distribution/<distribution_uuid>  # <- change UUID (obtained from FAIR Data Point)



Do not forget to restart *VODAN in a Box* after making the changes using ``docker-compose down && docker-compose up -d``. 

Changing ports
==============

If you need to change ports because you already use those for other services, you just need to adjust the mappings in ``docker-compose.yml`` file. For example, if you want to access BlazeGraph on other port than ``8085`` change the mapping ``8085:8080`` to something else, e.g. ``8885:8080``.

.. code-block:: yaml

   # ...

   blazegraph:
     image: metaphacts/blazegraph-basic:2.2.0-20160908.003514-6
     ports:
       - 8885:8080  # <- USING 8885 INSTEAD OF 8085
     volumes:
       - ./blazegraph:/blazegraph-data
