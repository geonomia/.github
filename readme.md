# geonomia

The *geonomia* project produces structured representations of collecting trips derived from GBIF-mediated specimen occurrence data, enabling batch georeferencing through improved locality interpretation. Treating collecting trips as a unit of analysis and curation also supports clearer attribution of collecting activity.

## Rationale

*Georeferencing* is a "string-to-thing" task, where the description of a locality specified on a collecting event must be turned into mappable coordinates. Collecting localities were visited by people moving through time and space as specimens were collected, so there is a strong relationship between the identities of the people responsible for the collecting event (listed in recordedBy), and the localities that they visited as they conducted their fieldwork. 

Precise localities are important as they enable species presence to be mapped in time and space, essential for species distribution mapping and conservation assessments. Also, as GBIF supports spatial querying of the occurrence data in its data portal, a spatial polygon is the part of the download criteria for x% of downloads used in scientific analyses. (todo calculate).

This initiative aims to group the botanical specimen data mediated by GBIF into collecting trips, so that localities visited along a collecting trip itinerary can be georeferenced together, using contributions from either the collectors themselves, or experts in the interpretation of localities for a particular region. 

## Background

Georeferencing is a complex and expert process, expensive to implement at scale. Post-accession georeferencing is often done as a step in a scientific analysis, which if scoped by taxon (for example for conservation assessment), can lose the context in which the collecting event was made. 
Herbarium mass digitisation projects have captured a lot of data about the collecting events associated with herbarium specimens, sometimes with textual descriptions of localities, sometimes with lat/long coordinates.
As botanical collecting events were conducted by a person (primary collector) who assigned a sequential collecting number to their field work, we can organise the work of an individual collector in its correct sequence. This recovers valuable context for the interpretation of textual locality descriptions, as adjacent collecting events are more likely to be conducted in closeby localities.
The [Bionomia](https://bionomia.net) project has labelled specimen data from GBIF with precise collector (and identifier) identity. Here, we do not attempt to determine a single identity for the complete career of a collector (as we are motivated primarily by simplifying georeferencing, it is enough for us to group collecting trips) - but it is possible for us to post updates to Bionomia to attribute all specimens from a particular collecting trip to the primary collector. An estimation of the number of updates that could be made to Bionomia is made as part of each processing run.

## Examples

(A scatter plot of recordNumber against eventDate for a selected familyname.)

(A scatter plot of recordNumber against eventDate for a selected familyname, with different collecting trip clusters indicated.)

## Structure

The geonomia projects are organised as follows:


- **geonomia** (organisation) - top-level information
    - **geonomia-processor** - clustering pipeline
    - **geonomia-web** - web interface for crowd-sourced georeferencing contributions
    - **geonomia-service** - configuration and usage protocol for an Open Refine compatible reconciliation service to allow users to (1) locate specimen duplicates from their own datasets and (2) locate similar collecting events for help with georeferencing
    - **geonomia-community** - Abstract submission and planning for the iDigBio "Digital Data in Biodiversity Research" conference
    - **geonomia-gbif-rationale** - code which analyses the use of spatial features in delimiting downloads of GBIF-mediated specimen data

## Roadmap

### In active development
- **Crowd-sourced contributions:** integration with a web user interface to accept crowd-sourced georeferences, allowing users to comment on and verify the contributions of others (similar to iNaturalist) (Steve Bachman). [geonomia-web](https://github.com/geonomia/geonomia-web)

- **Collecting event and/or collecting activity reconciliation service**
Expose the clustered specimen occurrence data to respond to Open Refine reconciliation requests to collecting event (matching on `recordedBy_firstFamilyName` and `recordNumber`) and collecting activity (matching on `recordedBy_firstFamilyName`, `countryCode` and `eventDate`). The latter can be generalised from precise day to month. This will allow users to resolve specimen duplicates in their own datasets, and to gain a picture of which collectors were operating in time/space of interest (useful for label interpretation). A usage protocol in development with colleagues from the Forest Research Institute Malaysia. (Nicky Nicolson). [geonomia-service](https://github.com/geonomia/geonomia-service)

- **Evaluation metrics and community feedback** Discussion session abstract for the iDigBio online conference Digital Data in Biodiversity Research (early June 2026) (Ashleigh Whittaker).
[geonomia-community](https://github.com/geonomia/geonomia-community)

### Planned
- **GBIF interface integration** a browser plugin similar to the [Bionomia browser extensions](https://bionomia.net/integrations) which decorate GBIF data portal pages with links to collector profiles. On an individual occurrence page, this would indicate when the specimen occurrence has been allocated to a collecting trip and provide a link to the cluster metadata (in Datasette) and crowd-sourcing platform. On a dataset page, this would display statistics (x records in this dataset (y percent) are present in z collecting trip clusters), and provide direct links to the appropriate canned queries in the Datasette instance. 
(TODO add mockup screenshot). [geonomia-extension](https://github.com/geonomia/geonomia-extension)

### Experimental

- **Auto-generated study area information - text summaries and spatial polygons:** Assess the performance of an LLM processing step to generate a text summary and GeoJSON polygon of the study area for each cluster. Example prompt:
    ```
    These localities were visited in the order presented as part of 
    a botanical collecting trip in [country]. 
    They are presented to you in CSV format, with the following DarwinCore format headers:
    - gbifID
    - locality
    - decimalLatitude
    - decimalLongitude
    - eventDate
    - recordNumber
    Generate: 
    1. A description of the study area and likely travelling route 
    and 
    2. A GeoJSON polygon which covers the study area. 
    When choosing the boundaries of the polygon, choose generalisation 
    (a wider area) rather than false precision (a smaller area). 
    ```
Verify the LLM results with comparison to known data and/or review by the actual collectors involved. 

## Use cases

- As a **GBIF data user**, I want to understand the collecting context of an occurrence record so that I can explore related records from the same collecting trip and recover details about habitats, landscape features and associated species.
- As a **conservation assessor**, I want to know the extent of occurrence of my target species by georeferencing locality descriptions, using collecting trips to group and interpret related records together
- As a **Bionomia scribe**:
    - I want to know which collectors with family name "Smith" were active in (country) on (date)
    - I want to bulk attribute occurrence records to recordedBy identities for their collectors by processing all occurrences in a collecting trip as a batch
- As a **collection data manager**, I want to assign collector identities to my specimens in batches, using collecting trips as a unit of work.
- As an **AI transcription tool developer**, I want to use collecting trip clusters as context for RAG (retrieval augmented generation) approaches to narrow down ambiguous collector names and localities on specimen labels.

## Project name

[Bionomia](https://bionomia.net) is a community that works on GBIF-mediated specimen data, turning "strings into things": the text representation of the people responsible for collecting or identifying a specimen is resolved into persistent person identifiers, allowing their contributions to be assembled and tracked.

The idea that “we need a Bionomia for georeferencing”, expressed informally in community discussions, captures a broader need to apply similar approaches to locality data at scale. This project addresses that need by reconstructing collecting trips from occurrence records, enabling locality descriptions to be interpreted in context rather than in isolation.

We named this project geonomia to reflect its alignment with Bionomia, extending the string-to-thing approach from people to places, and contributing new contextual structure to support both georeferencing and collector attribution.

## Contributing

(TBC)

## Useful links

(TBC)

## Team

- Nicky Nicolson
- Steve Bachman
- Ashleigh Whittaker
