**********
Components
**********

VODAN-in-a-Box consists of two significant services:

- `Data Stewardship Wizard (DSW) <https://ds-wizard.org>`_ adjusted to serve as Wizard for filling and maintaining electronic case report forms (eCRF),
- `FAIR Data Point (FDP) <https://github.com/FAIRDataTeam/FAIRDataPoint>`_ to maintain metadata about eCRFs created in DSW.

To support it, there are other services included:

- BlazeGraph triple store for FDP and optionally also to store eCRFs,
- MongoDB used by both DSW and FDP,
- JSON server providing controlled vocabulary for filling answers,
- submission service that handles storing eCRFs in triple store and updating metadata in FDP,
- RabbitMQ for queueing generation of an eCRF to a RDF document using DSW document worker,
- (optionally) Nginx proxy for :ref:`production-deployment`.
