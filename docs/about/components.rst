**********
Components
**********

VODAN-in-a-Box consists of two significant services:

- `Data Stewardship Wizard (DSW) <https://ds-wizard.org>`_ adjusted to serve as Wizard for filling and maintaining electronic case report forms (eCRF),
- `FAIR Data Point (FDP) <https://fairdatapoint.org>`_ to maintain metadata about eCRFs created in DSW.

To support it, there are other services included:

- AllegroGraph triple store for eCRF data and queries,
- BlazeGraph triple store for FDP,
- MongoDB used by both DSW and FDP,
- JSON server providing controlled vocabulary for filling answers,
- Submission Service that handles storing eCRFs in triple store and updating metadata in FDP,
- RabbitMQ for queueing generation of an eCRF to a RDF document using DSW document worker,
- (optionally) Nginx proxy for :ref:`production-deployment`.
