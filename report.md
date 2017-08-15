# Introduction
## Purpose of the document
## Background
## Glossary of terms
## Acronyms and terminology
## Related documents

# Architecture

## Linked data approach

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


## Solution overview


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




## Components

The diagram below illustrates the architecture of the PublishMyData system and hence of the technical solution used in the proof of concept. 

![System architecture](https://github.com/Swirrl/envdatapoc-report/blob/master/system_architecture.png "System architecture diagram")

* data sources
* data import processes
* database
* user interface
* APIs
* visualisation application

(a graph database using the [Stardog](http://www.stardog.com) software),


## Application layer (other apps)

The principle of the approach taken is that a consistent collection of data, available in machine-readable form through an API, can support a range of different applications: each selecting relevant data and presenting it in a way that suits a particular purpose or target audience.

In addition to the main map-based visualisation described in the previous section, other experimental applications have been built on top of the proof of concept data system, including:

* ...list examples

# Data samples

## Approach to data modelling

## River monitoring sites

## Environment

## Census

# Demonstration

## PublishMyData interface

## Visualisation interface for Proof of Concept

# Assessment overview

## Acceptance testing user guide

## Results

# Accomplishments

## Architecture

## Programming accomplishments

## Technical accomplishments

## Considerations for future work

there is a need to go back and correct older data in some circumstances - while that is possible with the current system, the data import pipelines could be configured to make that easier.

potential to link environmental data to demographic, socio-economic and various other kinds of data.  If other orgs also took a similar approach, the 'network effect' of being able to combine and compare data from different sources would be amplified

check Susan's lessons-learned log

the fact that all 4 participating regions were using consistent OGC standards for delivery of the monitoring sites and flow measurements data meant that integrating the different sources was relatively easy.  Nonetheless, slight variations in the implementations of those standards by the different councils (or their software suppliers) meant that the data import processes had to deal with special cases per region.

potential value of geosparql enabled database --> probs with generalisation of meshblock geometries



## Value and benefits




# Conclusions

# Appendices

## References

## Glossary and acronyms
