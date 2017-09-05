# Introduction
## Purpose of the document
The purpose of this document is to provide technical information that describes the detail of the architecture for the implementation of the Environmental Integrated Data Infrastructure (e-IDI) proof of concept (PoC).

This document does not contain:

- Functional requirements
- Evaluation results

## Background

A lack of standardised protocols and formats for data sharing amongst central and local government agencies,  crown research institutes, private sector and New Zealand public causes large, ongoing costs of time, money and efficiency.  

Almost every agency has differing infrastructure, data formats and channels for data storage and sharing.  The result is that each time data is shared amongst agencies, the data must be collated, cleaned and re-formatted by the receiving party before even basic use or analysis of the data may begin.

The [Natural Resource Sector](http://nrs.mfe.govt.nz/) sought funding to run a proof of concept to use established technologies including web, location intelligence and data linking to connect different data holdings into a single seamless virtual data layer.

The deliverables identified for the proof of concept were:

- *Brokering solution* to transform different data from disparate systems into a harmonised form for integration. 
- *Registry* for discoverability and accessibility.
- *Applications* for anlaysis and querying the data and proving reuse of the brokered data. 
- *Business case*, a write up of lessons learnt, and recommendations for next steps.

## Glossary of terms

Term|Description
---|---
Resource Definition Framework (RDF)| Standard model for data interchange on the Web
Transformation pipeline|  Converts tabular data into either more tabular data or linked data graphs. (http://grafter.org)
eXtensible Markup Language (XML) | Designed to store and transport data
Linked Data | An approach of connecting and publishing structured data from different data sources by using standard Web technologies.
PublishMyData |A complete end-to-end publishing solution by Swirrl (http://www.swirrl.com/)
WaterML 2.0 |Standard information model for the representation of water observations data, with the intent of allowing the exchange of such data sets across information systems [http://www.waterml2.org/]
REC| Dataset from MFE organises information about the physical characteristics of New Zealand's rivers. Individual river sections are mapped according to physical factors such as climate, source of flow for the river water, topography, and geology, and catchment land cover eg, forest, pasture or urban.
Uniform Resource Identifier (URI) | A single global identifier for a resource on the Web.

## Acronyms and terminology

Acronym|Term
:---|:---
RDF|Resource Description Framework
REC|Resource Environment Classification
WaterML2.0| Water Markup Language version 2.0
URI|Uniform Resource Identifier
XML|eXtensible Markup Language

## Related documents

Document|Date|Description
---|---|---
Better Public Service Seed fund Application|  2016|Original application for seeking NZ Treasury funding
High Level Requirements Specification |Feb 2017|Including functional and non-functional requirements for PoC 
e-IDI Summary Report|Aug 2017| Summary of PoC results, lessons learnt and recommendations

# Architecture

The proof of concept was based around application of the Linked Data approach, to implement a standards-based approach to disseminating data via the Web.  The overall solution incorporated a number of components:

* persistent URIs
* standard vocabularies
* metadata
* a software platform to store and present data
* transformation software to prepare and load data to the platform
* a software application to present an easily understandable, interactive view of the data

This section starts by briefly summarising the Linked Data approach and goes on to summarise the role of each of the above components in the overall solution.

## Linked Data Approach

'Linked Data' is the name given to a standards-based approach to representing and disseminating data using the World Wide Web.  The [principles of Linked Data](https://www.w3.org/DesignIssues/LinkedData.html) were first documented by Tim Berners-Lee in 2006.  The four guiding principles of Linked Data are:

* Use URIs as names for things.
* Use HTTP URIs so that people can look up those names.
* When someone looks up a URI, provide useful information, using the standards (RDF and SPARQL)
* Include links to other URIs. so that they can discover more things.

Using the Linked Data approach, all entities of interest, as well as the attributes used to describe them, are assigned a URI - a globally unique web-accessible identifier.  The characteristics of a thing are described as a series of attributes and values.  URIs can appear as the object of a subject-property-object 'triple' as well as the subject, so describing relationships between different things.  The [Resource Description Framework (RDF)](https://www.w3.org/RDF/) is a simple but powerful set of standards for representing data in this way.  [SPARQL](https://www.w3.org/TR/sparql11-query/) is a standardised query language for data represented as RDF.

The benefits of using Linked Data are:
* it is well suited to making machine-readable data available using the web
* it is based on well-established clearly documented open standards, so anyone can use it and it is supported by a variety of open source and proprietary software tools
* it is well suited to data integration: making it easy to combine and compare data from different sources

A common example of data integration using Linked Data is combining data from different organisations about a single place.  With well-defined identifiers for locations or areas, and standard patterns for representing observational or statistical data, then many organisations can publish what they know about a place in an interoperable way.  If you want to investigate the relationships between employment and health, or the environment and different approaches to farming, then it becomes much easier to pull together the information you need.

Linked Data also provides a natural mechanism for documenting data, describing its provenance, licensing terms and other key information generally referred to as 'metadata'. 

Making data available in this form can sometimes require extra effort from a data publisher, in order to describe their data precisely in a structured way: however this effort pays back every time someone uses the data, because a user of the data can access it with less effort and more confidence.

The process of standardisation still needs different people to agree on ways of identifying and describing their data, but Linked Data provides a mechanism for easy re-use of other people's data; and making data interoperable can often build on standards work that has already been done.

## Software Platform Overview

The technical solution implemented in the proof of concept is based around Swirrl's 'PublishMyData' software platform.  This consists of a data store, various tools for loading data and managing the data collection, as well as user interface and API features for viewing and extracting data.

This is used to deliver the web interface to the data at [http://envdatapoc.co.nz](http://envdatapoc.co.nz).

This is supplemented by a visualisation application, custom-built for this project using the RShiny web application framework for the 'R' programming language.  The visualisation shows how data can be extracted from the Linked Data platform and presented in a user-friendly way.  It can be viewed at [https://swirrl.shinyapps.io/NZ_River_Flow/](https://swirrl.shinyapps.io/NZ_River_Flow/).

The source code for the visualisation application is available in this [Github repository](https://github.com/Swirrl/nz_shiny_river_app).   (TO DO: that repository is still private, but could be made public when we are ready to publish the report).

The PublishMyData system runs on a single Linux virtual server, part of the Google Cloud Platform.  The RShiny application is hosted via the Shinyapps.io service and retrieves its data through the public API of the PublishMyData service.

The system holds a range of data: river monitoring sites locations and attributes; river flow and stage measurements at those sites; the River Environment Classification (REC) for all river reaches in New Zealand; and a range of supporting geographical information, such as region boundaries, water management zone boundaries and meshblock boundaries.

These data were all taken from existing publicly available sources and transformed into RDF using Extract-Transform-Load software 'pipelines', implemented using the open source ['Grafter' library](http://grafter.org). 

The sources are available in a range of formats: monitoring site data is provided by each participating regional council as a WFS endpoint, flow and monitoring data are provided as WaterML2 endpoints, the REC is available as a CSV file and the boundaries were provided as ESRI Shapefiles.

In some cases these were run as a one-off process.  The flow and stage data loading pipelines are run repeatedly on a schedule, in order to keep the cache of data up to date.

Once stored in the PublishMyData platform, the data is made available via a [SPARQL endpoint](http://envdatapoc.co.nz/sparql) supporting retrieval of data for use in other contexts.

The data can also be browsed and downloaded via a user interface. 

Authorised authenticated users have access to an administration user interface where they can add, edit or delete parts of the data and metadata collection.


### Software Components

The diagram below illustrates the architecture of the PublishMyData system and hence of the technical solution used in the proof of concept. 

![System architecture](https://github.com/Swirrl/envdatapoc-report/blob/master/system_architecture.png "System architecture diagram")

* data sources
* data import processes
* database
* user interface
* APIs
* visualisation application

(a graph database using the [Stardog](http://www.stardog.com) software),

## Persistent URIs

Part of the Linked Data approach is to assign URIs to entities of interest. These act both as an identifier and a means of retrieving a description of the entity.  

The prime requirement for a URI is that it uniquely identifies the associated entity, but following predictable patterns for designing URIs makes the data easier for users to understand and makes the job of producing and processing data more reliable.

The approach taken in the PoC was to follow the main points of the UK Government guidance 'Designing URI sets for the UK public sector' (https://www.gov.uk/government/publications/designing-uri-sets-for-the-uk-public-sector).

For operational systems, it is highly desirable that the URI identifying an entity does not change, and continues to be available online.  This means that people can use the associated data make references to the URI with confidence that it will continue to be available.  See ['Cool URIs don't change'](https://www.w3.org/Provider/Style/URI).

As this is a Proof of Concept project, intended to be experimental and of finite duration, persistence of the URIs was not one of the objectives, but the general design of URIs used in the project will be reviewed and can be taken forward for a potential future operational system.


## Standard Vocabularies

The process of representing data as RDF involves describing the attributes of the entities of interest and the relationships between those entities.  To make data as interoperable and understandable as possible, it is good practice to re-use existing well documented vocabularies of terms wherever possible.  Building on the standardisation and documentation work of others not only improves re-usability of the data, but also saves time.

The most important standard vocabularies used in the PoC were:

* [Sensor, Observation, Sample and Actuator ontology](https://www.w3.org/TR/vocab-ssn/) (SOSA, part of the Semantic Sensor Network)
* [Dublin Core Metadata Initiative](http://dublincore.org/documents/dcmi-terms/)
* [Data Catolog Vocabulary (DCAT)](https://www.w3.org/TR/vocab-dcat/)
* [Vocabulty of Interlinked Datasets (VoID)](https://www.w3.org/TR/void/)
* [Data Quality Vocabulary](https://www.w3.org/TR/vocab-dqv/)
* [QUDT](http://www.qudt.org/)

Not all attributes and relationships in the data were a good match to pre-existing vocabularies, so a new vocabulary was created for the PoC:

* [e-IDI PoC ontology](http://envdatapoc.co.nz/def/ontology)

In addition to the properties and classes described by the vocabularies above, the PoC also made use of various concept schemes.  Some of these were already defined, in registries such as the [NEMS Definition Register](https://registry.scinfo.org.nz/lab/nems).  In other cases, new concept schemes were created in the PoC, based on pre-existing glossaries and classifications, for example the [NEMS Glossary](http://envdatapoc.co.nz/lab/nems/def/glossary) and the [River Environment Classification](http://envdatapoc.co.nz/def/concept-scheme/rec).


## Metadata ##

Metadata is vital for data discoverability.  The initial [**metadata framework**](metadataFramework.pdf) identified metadata elements with associated standards and vocabulary mappings.   In accordance to the following framework , each council provided metadata information for monitoring sites, water management zones and water level observations, as a spreadsheet for loading into the PublishMyData platform.  Some metadata was extracted from the services, particularly dates and times for the time series data.  

The [**metadata model**](metadata-model.pdf) shows an example based on the final metadata model (for Horizons Regional Council's data) which is applicable to other council's data too.  

For **water level** datasets, metadata was not officially supplied, but the time and dates recordings of flow and stage were extracted for metadata purposes. Other metadata elements were added manually through the PoC administration interface.

The **geographic** data (i.e., REC, meshblocks and regions) references links that provides users metadata information within documentation at the data source.

# Data Samples

In order to test the PoC solution and validate the architecture, data needed to be provided in real time by the original data source.  

Stakeholders participating in the PoC agreed to supply links to their XML  and WFS services.  Table 2 identifies the data supplied for the PoC.

Dataset |Type| Supplier|Format
:---|:---|:-------------|:---
**River Monitoring Sites**|Static|	Horizons RC<br>Hawkes Bay RC<br>Environment Canterbury<br>Waikato RC|WFS
**River Measurements (stage and flow)**	|Dynamic<br>(varies across councils - 5 minutes to daily updates)|Horizons RC<br>Hawkes Bay RC<br>Environment Canterbury<br>Waikato RC|XML/WaterML 2.0<br><br>(via Hilltop &Kisters systems)
**Water Management Zones (WMZ)**|Static|Horizons RC<br>Hawkes Bay RC<br>Environment Canterbury<br>Waikato RC|WFS
**Regions**|Static|Statistics New Zealand|	Geodatabase (ESRI)
**Meshblocks**|Static|Statistics New Zealand|	Geodatabase (ESRI)
**River Environment Classification  (REC)**|Static|Ministry for Environment|	Geodatabase (ESRI)


*Table 2: Data used for the PoC*

*Note: Changes according to frequency identified in transformation pipeline (refer to solution section)

### Data Modelling

The data used for the PoC already existed for use within the LAWA application, which meant that the it was significantly standardized before being transformed into linked data.  There were still variances between the data elements across datasets.  Since, vocabulary terms were still required to be mapped to the data elements the datasets did not necessarily require standardisation before the transformation process.  

To view an example of the data models for monitoring sites across the four councils, refer to the following website ([]())

# Applications

This section identifies three applications that were used within the PoC.  The Horizons Regional Council application was developed external to the PoC environment to consume the data from the PoC. 

## PublishMyData Registry

The PublishMyData linked data registry developed by Swirrl. Refer to section [Software Platform Overview](report/#software-platform-overview)

<div class="video_container2">
<video width="640"  height="380" src="eidi-service.mp4"  controls preload>
<p>Or you can <a  href="eidi-service.mp4">download the video</a> instead.</p>
  </video> 
</div>

----------

## Landcare Lab Registry

This registry contained terms applicable to the LAWA  [LAWA)](http://lawa.org.nz) application.  The registry was set up at Landcare Research experimental laboratory registry site which utilises linked data registries.  For more information regarding this, click [here](https://registry.scinfo.org.nz/ui/about)

<div class="video_container3">
<video width="640"  height="380" src="lab-registry.mp4"  controls preload>
<p>Or you can <a  href="lab-registry.mp4">download the  video</a> instead.</p>
  </video> 
</div>

## Data Visualisation

The visualisation for the PoC solution developed by Swirrl using RShiny. Refer to section [Software Platform Overview](report/#software-platform-overview).

<div class="video_container1">
<video width="640"  height="480" src="eidi-shiny.mp4"  controls preload>
<p>Or you can <a  href="eidi-shiny.mp4">download the  video</a> instead.</p>
  </video>  
</div>



## Other Applications

The principle of the approach taken is that a consistent collection of data, available in machine-readable form through an API, can support a range of different applications: each selecting relevant data and presenting it in a way that suits a particular purpose or target audience.

In addition to the main chart and map-based visualisation described in section 4.1, other experimental applications have been built on top of the proof of concept data system, including one from [**Horizons Regional Council**](http://www.horizons.govt.nz)

The main purpose of this application was to demonstrate the reuse of the SPARQL endpoints for displaying data at an operational level that showed site information.  

Horizons Regional Council built an application using RShiny. The developer used RShiny and had no previous experience with using SPARQL and linked data.  Within a very short timeframe (approximately 20 minutes) an application was developed using one of the gallery templates.  The R library (SPARQL) writing function was applied to access the SPARQL endpoint.  To obtain the flow of a selected site within a region, a standard R Shiny script for filtering and displaying the data. 

See [Appendix B](report/#Appendix-b-sample-sparql-query) for the query function applied.


Figure 2  shows an image of the shiny appliation using the above query.

![RShiny Site Results](hrc-screenshot.JPG)

*Figure 2: Horizons Regional Council Shiny Application*


# Assessment  

## Overview

Acceptance criteria was identified from the high level requirements where two documents were generated to test the PoC. 

1. [Acceptance testing user guide]()
2. [Test case plan]()

**Ten** users with no prior linked data experience were selected to test the PoC.  **Two** other users were identified to test the technical details of the solution. 

## Results Summary



### Data ###

The data feeds do have issues at times, so councils need to have some validation processes to make sure that the data is coming through on regular basis.

# Accomplishments
Table 3 identifies the types of accomplishments the PoC achieved.

Type|Description
---|---
Architecture| The application of a data brokering ecosystem that is extensible and flexible to meet the changing needs of organisations and customers.
Programming accomplishments| More accessible and reusable applications of data assets.
Technical accomplishments| Users can access this technology with simple concepts to add value to their data.
Cultural change| A demonstrable product to prove a simple approach to harmonising and interlinking heterogeneous data sources. 

*Table 3: PoC Accomplishments*

## Considerations for future work

there is a need to go back and correct older data in some circumstances - while that is possible with the current system, the data import pipelines could be configured to make that easier.

potential to link environmental data to demographic, socio-economic and various other kinds of data.  If other orgs also took a similar approach, the 'network effect' of being able to combine and compare data from different sources would be amplified

check Susan's lessons-learned log

the fact that all 4 participating regions were using consistent OGC standards for delivery of the monitoring sites and flow measurements data meant that integrating the different sources was relatively easy.  Nonetheless, slight variations in the implementations of those standards by the different councils (or their software suppliers) meant that the data import processes had to deal with special cases per region.

potential value of geosparql enabled database --> probs with generalisation of meshblock geometries

Business case for the next steps.



## Value and benefits

- Use of standards in the form of vocabularies applied to datasets for interoperability is achievable. 
- A brokering ecosystem is inexpensive, extensible and sustainable.
- Multiple methods of data access (of full, filtered and combined datasets) through multiple downloadable mechanisms (in machine-readable (e.g., endpoints and linked data formats) and traditional formats (e.g., csv).
- Multiple live products to show reusability and visualisations of linked data.
- Incorporate into business as usual publishing workflows.
- Brokering data that is harmonised through use of standards increases data integration and reuse opportunities.


# Conclusions

# Appendices

## A: References

## B Sample SPARQL Query

The query function used for the Horizons Regional Council application is as follows:

    get_flow <- function(source_region,site){
      source_region <- paste(source_region,"-flow-measurements" ,sep = "")
      endpoint <- "http://guest:eidipoc@envdatapoc.co.nz/sparql"
      
      # create query statement
      query <- sprintf("
      PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
      PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
      PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>
      PREFIX sosa: <http://www.w3.org/ns/sosa/>
      PREFIX qudt: <http://qudt.org/1.1/schema/qudt#>
      
      SELECT ?time ?flowrate ?unit
      WHERE {graph <http://envdatapoc.co.nz/graph/%s> {
 	   ?s a sosa:Observation ;
    	sosa:hasFeatureOfInterest <http://envdatapoc.co.nz/id/measurement-site/%s> ;
   		sosa:resultTime ?time ;
    	sosa:hasResult ?result .
    	?result qudt:numericValue ?flowrate ;
    	qudt:unit ?unit .
      }}  ORDER BY ?time
      " ,source_region, site)
    
      # Step 2 - Use SPARQL package to submit query and save results to a data frame
      qd <- SPARQL(endpoint,query)  
      flow <- qd$results
      flow <- na.omit(flow)
      #plot(y = flow$flowrate, x = flow$time, type = "l")  
      return(flow)
    }

