***************
Usage Scenarios
***************

The **VODAN in a Box (ViaB)** toolset can be used in the following scenarios:

Data-entry only
---------------

In this scenario, only the VODAN DSW and its COVID-19 semantic data model are used as a data-entry tool. Users can fill in the DSW's web form with data to report COVID-19 cases. 

Normally, this scenario is indicated for the cases where metadata about the data does not need to be published.

Those use cases are require user to be logged in in VODAN DSW:

Create eCRF
~~~~~~~~~~~

1. Select :guilabel:`CRFs` from the left menu
2. Press :guilabel:`Create` button
3. Fill-in identifier and press :guilabel:`Save` button
4. Fill CRF with data you have, :guilabel:`Save` or :guilabel:`Discard` changes accordingly

Update eCRF
~~~~~~~~~~~

1. Select :guilabel:`CRFs` from the left menu
2. Find by name the CRF you want to edit
3. Fill CRF with new data you have, :guilabel:`Save` or :guilabel:`Discard` changes accordingly

Submit eCRF
~~~~~~~~~~~

1. Open CRF you want to submit
2. Press :guilabel:`Create Report`
3. Press :guilabel:`Create` (optionally, you can name the report, e.g. "My report - v0.1")
4. Press three dots on the right for the new report and press :guilabel:`Submit`
5. Select the triple store you want to use and press :guilabel:`Submit`


Metadata publication only
-------------------------

In this scenario, only the VODAN FDP is used to publish the pandemic-related content. This option is indicated for the cases where data have already been captured and only the FAIR metadata about them need to be published.

Data could have been made available from other CRF-entry tools and extracted directly from databases of information systems such as electronic health record systems.

Data-entry and metadata publication (complete package)
------------------------------------------------------

In this scenario, the whole **VODAN in a Box** is used, covering the data capture, semantic data generation and metadata publishing. 
