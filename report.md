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

The approach ta
range of data sources: monitoring sites, flow and stage measurements, REC, geographical info

data model - drawing on existing approaches where possible

ETL processes to transform data to appropriate form and load into system

graph database

access to that data via API - in particular via SPARQL endpoint

user interface to explore the detailed structure and availability of data


## Components



* data sources
* data import processes
* database
* user interface
* APIs
* visualisation application

## Application layer (other apps)

The principle of the approach taken is that a consistent collection of data, available in machine-readable form through an API, can support a range of different applications: each selecting relevant data and presenting it in a way that suits a particular purpose or target audience.

In addition to the main map-based visualisation described in the previous section, other experimental applications have been built on top of the proof of concept data system, including:

* ...list examples

# Data samples

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

there is a need to go back and correct older data in some circumstances

potential to link environmental data to demographic, socio-economic and various other kinds of data.  If other orgs also took a similar approach, the 'network effect' of being able to combine and compare data from different sources would be amplified

check Susan's lessons-learned log

## Value and benefits




# Conclusions

# Appendices

## References

## Glossary and acronyms
