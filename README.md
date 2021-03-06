## Transformation Functions Ontology

<div class="section" about="#" typeof="owl:Ontology">

  - Author  
    [<span property="foaf:name">Dorian
    Taylor</span>](https://doriantaylor.com/person/dorian-taylor#me)
  - Created  
    June 4, 2014
  - Updated  
    January 20, 2020
    April 10, 2020
    June 10, 2020
    July 2, 2020
  - Namespace URI  
    <https://vocab.methodandstructure.com/transformation#>
  - Preferred Namespace Prefix  
    tfo

This document describes functions which transform HTTP
<span class="dfn">representations</span>, i.e., the actual literal
payloads of HTTP messages.

Many, if not most Web content manipulation tasks are concerned
exclusively with *representations*: the literal *contents* of HTTP
*messages*, rather than abstract or internal representations. Such
manipulations may include excerpting and rearranging files, compressing
and decompressing, cropping and resizing images, and converting between
data formats. If we separate these operations from other *development*
tasks, we can reuse them.

The purpose of this vocabulary is to provide a mechanism for identifying
<span class="dfn">pure functions</span> that transform opaque data
segments (like HTTP response bodies) and perhaps take additional scalar
parameters (and specify both *named* and *sequential* calling
conventions), and relate a function definition to a concrete
implementation. The vocabulary also provides a mechanism for
[<span class="dfn">memoizing</span>](https://en.wikipedia.org/wiki/Memoization)
the application of a function to a particular data object, i.e.,
relating a particular input (plus any additional parameters) to a
particular output.

Consider the following example:

    @prefix tfo:  <https://vocab.methodandstructure.com/transformation#> .
    @prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#> .
    @prefix owl:  <http://www.w3.org/2002/07/owl#> .
    @prefix xsd:  <http://www.w3.org/2001/XMLSchema#> .
    @prefix skos: <http://www.w3.org/2004/02/skos/core#> .
    @prefix dct:  <http://purl.org/dc/terms/> .
    @prefix prov: <http://www.w3.org/ns/prov#> .
    @prefix ex:   <https://example.club/#> .
    
    ex:range a tfo:Transform ;
      skos:prefLabel "Byte Range"@en ;
      skos:description "Select a subset of bytes from a given segment."@en ;
      tfo:accepts "*/*"^^tfo:content-type ;
      tfo:returns "*/*"^^tfo:content-type ;
      tfo:parameter ex:start, ex:end ;
      tfo:parameter-list ( ex:start ex:end ) ;
      tfo:implementation <jar:functions.jar!org/foobar/textproc/ByteRange.class> .
    
    ex:start a tfo:Parameter ;
      dct:identifier "start"^^xsd:token ;
      rdfs:comment "The beginning offset (omission interpreted as zero)."@en ;
      owl:maxCardinality 1 ;
      rdfs:range xsd:nonNegativeInteger .
    
    ex:end a tfo:Parameter ;
      dct:identifier "end"^^xsd:token ;
      rdfs:comment "The ending offset (omission interpreted as the end of the input)."@en ;
      owl:maxCardinality 1 ;
      rdfs:range xsd:nonNegativeInteger .
    
    <urn:uuid:1d85f168-1710-4bc8-b68b-c7513483c228> a tfo:Application ;
      prov:startedAtTime "2020-01-20T16:46:27.043Z"^^xsd:dateTime ;
      prov:endedAtTime "2020-01-20T16:46:27.051Z"^^xsd:dateTime ;
      tfo:transform ex:range ;
      tfo:input <ni:///sha-256;fzhKzqnfAxmB9ICBwe00Dq3smuo43J2K-58uL_62t6Q> ;
      ex:start 1234 ;
      ex:end 31337 ;
      tfo:output <ni:///sha-256;QqNiOgLJk13nOs8Mn4prYtEIZZfwuiju2y4cT_YCOlI> .

The preceding example defines a transformation function, along with its
parameters, and relates it to a fictional implementation embedded in a
JAR somewhere (the exact way a transform's implementation is resolved is
out of scope for this vocabulary). At the bottom is an imaginary
application of the transformation function, along with values for
supplied parameters. The input and output are designated by digest URIs,
which can be resolved to their corresponding representations.

This vocabulary extends [the Provenance
Ontology](https://www.w3.org/TR/prov-o/), by way of refining the
[`prov:SoftwareAgent`](https://www.w3.org/TR/prov-o/#SoftwareAgent) and
[`prov:Activity`](https://www.w3.org/TR/prov-o/#Activity) classes, and
related properties. For instance, a record can use its inherited PROV
properties to store metrics on the time it took to execute the function
over the input.

</div>

<div class="section">

## Classes

![](https://vocab.methodandstructure.com/transformation-classes)

<div class="section">

### Transforms

A `tfo:Transform` is essentially a function definition, specifying input
and output constraints, and parameters in both named and positional
representations.

<div id="Transform" class="section" about="[tfo:Transform]" typeof="owl:Class">

#### Transform

This class provides a specification for a transformation function.

  - Subclass of:  
    [prov:SoftwareAgent](https://www.w3.org/TR/prov-o/#SoftwareAgent)
  - Properties:  
    [tfo:implementation](https://vocab.methodandstructure.com/transformation#implementation)
    [tfo:parameter](https://vocab.methodandstructure.com/transformation#parameter)
    [tfo:parameter-list](https://vocab.methodandstructure.com/transformation#parameter-list)
    [tfo:accepts](https://vocab.methodandstructure.com/transformation#accepts)
    [tfo:returns](https://vocab.methodandstructure.com/transformation#returns)
    [tfo:by-uri](https://vocab.methodandstructure.com/transformation#by-uri)
    [tfo:not-by-uri](https://vocab.methodandstructure.com/transformation#not-by-uri)

</div>

<div id="MarkupTransform" class="section" about="[tfo:MarkupTransform]" typeof="owl:Class">

#### MarkupTransform

This class represents the set of transformation functions that operate
exclusively over (HTML/XML) markup.

  - Subclass of:  
    [tfo:Transform](https://vocab.methodandstructure.com/transformation#Transform)
  - Properties:  
    [tfo:by-xpath](https://vocab.methodandstructure.com/transformation#by-xpath)
    [tfo:not-by-xpath](https://vocab.methodandstructure.com/transformation#not-by-xpath)

</div>

</div>

<div class="section">

### Parameters

`tfo:Parameter` entities encapsulate name, type, and cardinality of a
given function parameter, while `tfo:ParameterList` entities specify
position.

<div id="Parameter" class="section" about="[tfo:Parameter]" typeof="owl:Class">

#### Parameter

This class provides a specification for a parameter in a given function.

  - Subclass of:  
    [rdf:Property](https://www.w3.org/TR/rdf-schema/#ch_property)
  - Property restrictions:  
    [rdfs:domain](https://www.w3.org/TR/rdf-schema/#ch_domain) ???
    [tfo:Partial](https://vocab.methodandstructure.com/transformation#Partial)
  - Properties:  
    [tfo:default](https://vocab.methodandstructure.com/transformation#default)

</div>

<div id="ParameterList" class="section" about="[tfo:ParameterList]" typeof="owl:Class">

#### ParameterList

This class represents a list with the restriction that its members be
tfo:Parameter nodes.

  - Subclass of:  
    [rdf:List](http://www.w3.org/1999/02/22-rdf-syntax-ns#List)
  - Property restrictions:  
    [rdf:first](https://www.w3.org/TR/rdf-schema/#ch_first) ???
    [tfo:Parameter](https://vocab.methodandstructure.com/transformation#Parameter)
    [rdf:rest](https://www.w3.org/TR/rdf-schema/#ch_rest) ???
    [tfo:ParameterList](https://vocab.methodandstructure.com/transformation#ParameterList)

</div>

</div>

<div class="section">

### Function Application

Recording partial and full function applications is important for
*memoizing* computations.

<div id="Application" class="section" about="[tfo:Application]" typeof="owl:Class">

#### Application

This class represents an application of a transformation function,
connecting a specific input and scalar parameters with its output.

  - Subclass of:  
    [tfo:Partial](https://vocab.methodandstructure.com/transformation#Partial)
  - Properties:  
    [tfo:completes](https://vocab.methodandstructure.com/transformation#completes)
    [tfo:input](https://vocab.methodandstructure.com/transformation#input)
    [tfo:output](https://vocab.methodandstructure.com/transformation#output)

</div>

<div id="Partial" class="section" about="[tfo:Partial]" typeof="owl:Class">

#### Partial

This class represents a *partial* application of a transformation
function, affording the encapsulation and re-use of existing parameters.

  - Subclass of:  
    [prov:Activity](https://www.w3.org/TR/prov-o/#Activity)
  - Properties:  
    [tfo:transform](https://vocab.methodandstructure.com/transformation#transform)

</div>

</div>

</div>

<div class="section">

## Properties

![](https://vocab.methodandstructure.com/transformation-properties)

<div class="section">

### Core

<div id="completes" class="section" about="[tfo:completes]" typeof="owl:ObjectProperty owl:FunctionalProperty">

#### completes

Identifies a `tfo:Partial` function that this `tfo:Application`
completes.

  - Domain:  
    [tfo:Application](https://vocab.methodandstructure.com/transformation#Application)
  - Range:  
    [tfo:Partial](https://vocab.methodandstructure.com/transformation#Partial)

[Back to Top](https://vocab.methodandstructure.com/transformation#)

</div>

<div id="implementation" class="section" about="[tfo:implementation]" typeof="owl:ObjectProperty owl:FunctionalProperty">

#### implementation

URI to the implementation of the function.

The URI is meant to be dereferenced by an internal implementation, e.g.
file:, jar:, or an idiosyncratic scheme like urn:x-python:.

  - Domain:  
    [tfo:Transform](https://vocab.methodandstructure.com/transformation#Transform)
  - Range:  
    [rdfs:Resource](https://www.w3.org/TR/rdf-schema/#ch_resource)

[Back to Top](https://vocab.methodandstructure.com/transformation#)

</div>

<div id="parameter" class="section" about="[tfo:parameter]" typeof="owl:ObjectProperty">

#### parameter

Binds a parameter object to its function.

Parameters can either be supplied to the function as key-value pairs or
sequentially.

  - Domain:  
    [tfo:Transform](https://vocab.methodandstructure.com/transformation#Transform)
  - Range:  
    [tfo:Parameter](https://vocab.methodandstructure.com/transformation#Parameter)

[Back to Top](https://vocab.methodandstructure.com/transformation#)

</div>

<div id="parameter-list" class="section" about="[tfo:parameter-list]" typeof="owl:ObjectProperty owl:FunctionalProperty">

#### parameter-list

Specifies the sequence of parameters when the invocation method of the
function is sequential.

  - Domain:  
    [tfo:Transform](https://vocab.methodandstructure.com/transformation#Transform)
  - Range:  
    [tfo:ParameterList](https://vocab.methodandstructure.com/transformation#ParameterList)

[Back to Top](https://vocab.methodandstructure.com/transformation#)

</div>

<div id="accepts" class="section" about="[tfo:accepts]" typeof="owl:ObjectProperty">

#### accepts

Specifies the list of content-types, in order of preference, that the
function can process.

  - Domain:  
    [tfo:Transform](https://vocab.methodandstructure.com/transformation#Transform)
  - Range:  
    <span about="_:uo1" rel="owl:unionOf" resource="_:q1">
    [tfo:content-type](https://vocab.methodandstructure.com/transformation#content-type)
    <span about="_:q1" rel="rdf:rest" resource="_:q2">???</span>
    [rdf:List](https://www.w3.org/TR/rdf-schema/#ch_list)
    <span about="_:q2" rel="rdf:rest" resource="rdf:nil"></span> </span>

[Back to Top](https://vocab.methodandstructure.com/transformation#)

</div>

<div id="returns" class="section" about="[tfo:returns]" typeof="owl:ObjectProperty">

#### returns

Specifies the list of content-types, in order of preference, that the
function is capable of returning.

  - Domain:  
    [tfo:Transform](https://vocab.methodandstructure.com/transformation#Transform)
  - Range:  
    <span about="_:uo2" rel="owl:unionOf" resource="_:q3">
    [tfo:content-type](https://vocab.methodandstructure.com/transformation#content-type)
    <span about="_:q3" rel="rdf:rest" resource="_:q4">???</span>
    [rdf:List](https://www.w3.org/TR/rdf-schema/#ch_list)
    <span about="_:q4" rel="rdf:rest" resource="rdf:nil"></span> </span>

[Back to Top](https://vocab.methodandstructure.com/transformation#)

</div>

<div id="default" class="section" about="[tfo:default]" typeof="owl:ObjectProperty">

#### default

Specifies one or more default values for a parameter.

  - Domain:  
    [tfo:Parameter](https://vocab.methodandstructure.com/transformation#Parameter)

[Back to Top](https://vocab.methodandstructure.com/transformation#)

</div>

<div id="transform" class="section" about="[tfo:transform]" typeof="owl:ObjectProperty owl:FunctionalProperty">

#### transform

Specifies the transform associated with this particular application

  - Domain:  
    [tfo:Partial](https://vocab.methodandstructure.com/transformation#Partial)
  - Range:  
    [tfo:Transform](https://vocab.methodandstructure.com/transformation#Transform)

[Back to Top](https://vocab.methodandstructure.com/transformation#)

</div>

<div id="input" class="section" about="[tfo:input]" typeof="owl:ObjectProperty owl:FunctionalProperty">

#### input

Specifies the resource that was the input of the transformation
function.

  - Domain:  
    [tfo:Application](https://vocab.methodandstructure.com/transformation#Application)
  - Range:  
    [rdfs:Resource](https://www.w3.org/TR/rdf-schema/#ch_resource)

[Back to Top](https://vocab.methodandstructure.com/transformation#)

</div>

<div id="output" class="section" about="[tfo:output]" typeof="owl:ObjectProperty owl:FunctionalProperty">

#### output

Specifies the resource that was the output of the transformation
function.

  - Domain:  
    [tfo:Application](https://vocab.methodandstructure.com/transformation#Application)
  - Range:  
    [rdfs:Resource](https://www.w3.org/TR/rdf-schema/#ch_resource)

[Back to Top](https://vocab.methodandstructure.com/transformation#)

</div>

</div>

<div class="section">

### Constraints

In certain contexts it is useful to pre-declare constraints on what
constitutes valid input for a transform. We can do this either by URI or
by XPath.

<div id="by-uri" class="section" about="[tfo:by-uri]" typeof="owl:DatatypeProperty">

#### by-uri

Specifies a regular expression for matching against URIs.

  - Domain:  
    [tfo:Transform](https://vocab.methodandstructure.com/transformation#Transform)
  - Range:  
    [tfo:regexp](https://vocab.methodandstructure.com/transformation#regexp)

[Back to Top](https://vocab.methodandstructure.com/transformation#)

</div>

<div id="not-by-uri" class="section" about="[tfo:not-by-uri]" typeof="owl:DatatypeProperty">

#### not-by-uri

Specifies a regular expression for *anti*-matching against URIs.

  - Domain:  
    [tfo:Transform](https://vocab.methodandstructure.com/transformation#Transform)
  - Range:  
    [tfo:regexp](https://vocab.methodandstructure.com/transformation#regexp)

[Back to Top](https://vocab.methodandstructure.com/transformation#)

</div>

<div id="prefix" class="section" about="[tfo:prefix]" typeof="owl:ObjectProperty">

#### prefix

Specifies a SHACL prefix declaration for complementing any associated
XPath expression.

Note that the existing `sh:declare` property has a domain of
`owl:Ontology` so we can't use it for this.

  - Domain:  
    [tfo:MarkupTransform](https://vocab.methodandstructure.com/transformation#MarkupTransform)
  - Range:  
    [sh:PrefixDeclaration](https://www.w3.org/TR/shacl/#sparql-prefixes)

</div>

<div id="by-xpath" class="section" about="[tfo:by-xpath]" typeof="owl:DatatypeProperty">

#### by-xpath

Specifies an XPath expression for matching against markup (HTML/XML)
content.

  - Domain:  
    [tfo:MarkupTransform](https://vocab.methodandstructure.com/transformation#MarkupTransform)
  - Range:  
    [tfo:xpath](https://vocab.methodandstructure.com/transformation#xpath)

[Back to Top](https://vocab.methodandstructure.com/transformation#)

</div>

<div id="not-by-xpath" class="section" about="[tfo:not-by-xpath]" typeof="owl:DatatypeProperty">

#### not-by-xpath

Specifies an XPath expression for *anti*-matching against markup
(HTML/XML) content.

  - Domain:  
    [tfo:MarkupTransform](https://vocab.methodandstructure.com/transformation#MarkupTransform)
  - Range:  
    [tfo:xpath](https://vocab.methodandstructure.com/transformation#xpath)

[Back to Top](https://vocab.methodandstructure.com/transformation#)

</div>

</div>

</div>

<div class="section">

## Datatypes

Currently there is only the datatype for designating [content
types](https://www.iana.org/assignments/media-types/media-types.xhtml),
and those for regular expressions and XPath.

<div id="content-type" class="section" about="[tfo:content-type]" typeof="rdfs:Datatype">

#### content-type

A literal that represents a content-type such as that which is found in
the HTTP `Accept:` or `Content-Type:` header.

  - Restriction of:  
    [xsd:token](https://www.w3.org/TR/xmlschema-2/#token)
    <span rel="owl:withRestrictions" resource="_:rl1">matching</span>
    <span about="_:rl1" rel="rdf:first">``^([!#$%&'\*\+\-.^_`|~0-9-A-Za-z]+)(?:/[!#$%&'\*\+\-.^_`|~0-9-A-Za-z]+)?)$``</span>
    <span about="_:rl1" rel="rdf:rest" resource="rdf:nil"></span>
  - See also:  
    [RFC 7230 ??? Hypertext Transfer Protocol (HTTP/1.1): Message Syntax
    and Routing ?? 3.2.6: Field Value
    Components](https://tools.ietf.org/html/rfc7230#section-3.2.6)
    [RFC 7231 ??? Hypertext Transfer Protocol (HTTP/1.1): Semantics and
    Content ?? 5.3.2:
    `Accept`](https://tools.ietf.org/html/rfc7231#section-5.3.2)

[Back to Top](https://vocab.methodandstructure.com/transformation#)

</div>

<div id="regexp" class="section" about="[tfo:regexp]" typeof="rdfs:Datatype">

#### regexp

A regular expression.

We do not specify a flavour, but in practice we should assume PCRE or
ECMA-262.

  - Restriction of:  
    [xsd:string](https://www.w3.org/TR/xmlschema-2/#string)

</div>

<div id="iregexp" class="section" about="[tfo:iregexp]" typeof="rdfs:Datatype">

#### iregexp

A case-insensitive regular expression.

Note that we do not intend to provide the full complement of flags for
regular expressions like `m`, `s`, or `x`.

  - Subclass of:  
    [tfo:regexp](https://vocab.methodandstructure.com/transformation#regexp)
  - Restriction of:  
    [xsd:string](https://www.w3.org/TR/xmlschema-2/#string)

</div>

<div id="xpath" class="section" about="[tfo:xpath]" typeof="rdfs:Datatype">

#### xpath

An XPath expression.

  - Restriction of:  
    [xsd:string](https://www.w3.org/TR/xmlschema-2/#string)

</div>

</div>

<div>

## References

  - [PROV-O: The Provenance Ontology](https://www.w3.org/TR/prov-o/)
  - [RDF Schema 1.1](https://www.w3.org/TR/rdf-schema/)
  - [XML Schema Part 2: Datatypes Second
    Edition](https://www.w3.org/TR/xmlschema-2/)
  - [Media Types
    (IANA)](https://www.iana.org/assignments/media-types/media-types.xhtml)

</div>
