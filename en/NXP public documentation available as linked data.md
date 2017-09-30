Due to popular demand, we’re working on making some basic (meta)data available about many of the documents and other files published on nxp.com including: data sheets, application notes, user manuals, white papers and videos.

We’ll be publishing this as RDF Linked Data using the following existing web vocabularies:

* foaf: http://xmlns.com/foaf/0.1/
* dcterms: http://purl.org/dc/terms/
* nfo: http://www.semanticdesktop.org/ontologies/nfo/#
* schema: http://schema.org/

We have introduced some new resources of type rdfs:Class to provide a list of ‘approved’ NXP terms that may be used as a value of the dcterms:type property. The full list of these classes is here.

Below is a small sample of the description of a data sheet in Turtle syntax (apologies for lack of indentation):

```turtle
@prefix dcterms: <http://purl.org/dc/terms/>.
@prefix nfo: <http://www.semanticdesktop.org/ontologies/nfo/#>.
@prefix schema: <http://schema.org/>.
@prefix foaf: <http://xmlns.com/foaf/0.1/>.
@prefix nxp: <http://purl.org/nxp/schema/v1/>.
<http://www.nxp.com/documents/data_sheet/PMBFJ108_109_110.pdf> a nfo:PaginatedTextDocument;
dcterms:title "PMBFJ108_109_110 - N-channel junction FETs"@en-US;
dcterms:description "N-channel junction FETs"@en-US;
dcterms:identifier "1256628791101";
dcterms:issued "2011-09-20T12:12:30"^^<http://www.w3.org/2001/XMLSchema.xsd#dateTime>;
dcterms:type nxp:DataSheet;
schema:inLanguage "en-US";
nfo:fileName "PMBFJ108_109_110.pdf";
nfo:fileSize "82944"^^<http://www.w3.org/2001/XMLSchema#integer>;
nfo:pageCount "9"^^<http://www.w3.org/2001/XMLSchema#integer>;
nfo:permissions "Company Public";
foaf:isPrimaryTopicOf <http://data.nxp.com/doc/published_files/1256628791101>;
foaf:topic <http://data.nxp.com/id/basic_types/pmbfj108>, <http://data.nxp.com/id/basic_types/pmbfj109>, <http://data.nxp.com/id/basic_types/pmbfj110>.
```

Looking at a few points in more detail you might notice the URI of the resource is the URI in the www.nxp.com domain, we have chosen to use these existing URIs rather than mint new ones in data.nxp.com sub-domain as it is far simpler to do a simple DESCRIBE query using these than having to first look-up or somehow otherwise resolve the original URI. Also the foaf:isPrimaryTopicOf is used to refer to a document containing the description of the resource (i.e. the above RDF), effectively a document whose content is the metadata about another document. If you navigate to that URI (try it), you should get an HTML representation of the above metadata. Last but not least the foaf:topic relates the data sheet to the product(s) described in the data sheet. Note that these URIs are acting as identifiers for the conceptual product (which obviously cannot be downloaded over the internet) and, as such, redirect to a document containing a representation of the resource.

This metadata has been created from the XML stored in our XML database using XQuery, serializing the result as RDF/XML.

The publication of this metadata gives us the opportunity to shine a light on our value proposition content  entirely published and translated from DITA XML (example for , ZIP), an OASIS standard for technical documentation. The value proposition describes the key features, benefits and applications of a product or a group of products (here, for example, the first paragraph and the features list). This content represents the great majority of the content visible on nxp.com. Migrating to DITA XML has enabled very high reuse of content, multi channel publication and a fluid translation process (the same page as above, in Chinese).
