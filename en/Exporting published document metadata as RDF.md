Since January 7th 2013 (blog.nxp.com), NXP officially entered the open linked data world. That day we published our first set of product data and exposed a SPARQL endpoint. As explained in the article John Walker published that day, the main drive is internal. We got tired of managing different data models across silos and needed one central and reliable source of data to answer our questions and those our users ask us everyday.

Since that day, published documents metadata has also been published. For the most popular types of document, we publish the following metadata (examples for LPC111X data sheet):

* Title (LPC111X)
* Descriptive title (32-bit ARM Cortex-M0 microcontroller; 4 kB flash and 1 kB SRAM)
* Release date (2013-02-20T15:06:57Z)
* Language code (en-US)
* Page count (114)
* File size (1823744 bytes)
* Products linked:
	* LPC1112FHN24
	* LPC1114FHI33
	* LPC1115FBD48
* and 17 others...

And this is the corresponding RDF (in Turtle syntax):

```turtle
@prefix dcterms: <http://purl.org/dc/terms/>.
@prefix skos: <http://www.w3.org/2004/02/skos/core#>.
@prefix nfo: <http://www.semanticdesktop.org/ontologies/nfo/#>.
@prefix schema: <http://schema.org/>.
@prefix foaf: <http://xmlns.com/foaf/0.1/>.
@prefix xs: <http://www.w3.org/2001/XMLSchema#>.
@prefix nxp: <http://purl.org/nxp/schema/v1/>.
@prefix xml: <http://www.w3.org/XML/1998/namespace>.
@prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#>.
@prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>.
<http://www.nxp.com/documents/data_sheet/LPC111X.pdf> dcterms:identifier "1335189131148";
    dcterms:issued "2013-02-20T15:06:57"^^xs:dateTime;
    dcterms:title "LPC111X - 32-bit ARM Cortex-M0 microcontroller; up to 64 kB flash and 8 kB SRAM"@en-US;
    dcterms:type nxp:DataSheet;
    nxp:specificationStatus "Product";
    schema:inLanguage "en-US";
    nfo:fileName "LPC111X.pdf";
    nfo:fileSize 1823744;
    nfo:pageCount 114;
    nfo:permissions "Company Public";
    a nfo:PaginatedTextDocument;
    foaf:isPrimaryTopicOf <http://data.nxp.com/doc/published_files/1335189131148>,
        <http://data.nxp.com/internal/published_files/1335189131148>;
    foaf:topic <http://data.nxp.com/id/basic_types/lpc1110fd20>,
        <http://data.nxp.com/id/basic_types/lpc1111fdh20>,
        <http://data.nxp.com/id/basic_types/lpc1111fhn33>,
        <http://data.nxp.com/id/basic_types/lpc1112bn28>,
        […]
        <http://data.nxp.com/id/groups/grouping394>.
```


To produce this RDF, I wrote an XQuery that queries our XML database, where this metadata is stored, structured with a proprietary XML Schema. This query returns RDF statements in RDF/XML (another syntax for RDF), which are loaded in our RDF repository, Dydra (self hosted).

We are now able to make SPARQL queries across published document metadata, product data, and the marketing tree of categories and groups of products, such as:
Which value propositions for the products of the Microcontrollers branch have been updated since the last translation project?
What is the average number of pages in data sheets per top product category?
Which products have been released in the past month, and don’t have a value proposition?

Since this is all new to us, the challenge now is to be creative, leave the old mindset in the past and use the full potential of this data!
