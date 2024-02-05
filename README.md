<div about="#" typeof="owl:Ontology">

Author  
<a href="https://doriantaylor.com/person/dorian-taylor#me"
rel="dct:creator" typeof="foaf:Person"><span property="foaf:name">Dorian
Taylor</span></a>

Created  
June 4, 2014

Updated  
January 20, 2020

April 10, 2020

June 10, 2020

July 2, 2020

August 27, 2023

September 6, 2023

October 9, 2023

January 30, 2024

Namespace URI  
[`https://vocab.methodandstructure.com/transformation#`](https://vocab.methodandstructure.com/transformation#)

Preferred Namespace Prefix  
`tfo`

Imported Vocabularies  
<a href="https://www.w3.org/TR/xmlschema11-2/" about="#"
rel="owl:imports" resource="http://www.w3.org/2001/XMLSchema#">W3C XML
Schema Definition Language (XSD) 1.1 Part 2: Datatypes</a>

This document describes functions which transform HTTP <span
class="dfn">representations</span>, i.e., the actual literal payloads of
HTTP messages.

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
implementation. The vocabulary also provides a mechanism for [<span
class="dfn">memoizing</span>](https://en.wikipedia.org/wiki/Memoization)
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

    ex:range a tfo:Function ;
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

    <urn:uuid:1d85f168-1710-4bc8-b68b-c7513483c228> a tfo:Invocation ;
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

This vocabulary extends the
<a href="https://www.w3.org/TR/vocab-dcat-2/" about="#"
rel="owl:imports" resource="dcat:">Data Catalog Vocabulary</a> by making
`tfo:Function` a subclass of
[`dcat:DataService`](https://www.w3.org/TR/vocab-dcat-2/#Class:Data_Service)
and
<a href="https://www.w3.org/TR/prov-o/" about="#" resource="prov:">the
Provenance Ontology</a>, by way of refining the
[`prov:Activity`](https://www.w3.org/TR/prov-o/#Activity) class and
related properties. For instance, a record can use its inherited PROV
properties to store metrics on the time it took to execute the function
over the input.

</div>

<div class="section">

## Classes

![](https://vocab.methodandstructure.com/transformation-classes)

<div class="section">

### Bundles

The Transformation Functions Ontology includes a basic bundling class
that affords the reuse of parameters, e.g. for configuration.

<div id="Bundle" class="section" about="[tfo:Bundle]"
typeof="owl:Class">

#### `Bundle`

This class provides a basic mechanism to yoke a set of transformation
functions together.

Subclass of:  
<a href="https://www.w3.org/TR/vocab-dcat-2/#Class:Catalog"
rel="rdfs:subClassOf"
resource="dcat:Catalog"><code>dcat:Catalog</code></a>

<a href="https://www.w3.org/TR/prov-o/#SoftwareAgent"
rel="rdfs:subClassOf"
resource="prov:SoftwareAgent"><code>prov:SoftwareAgent</code></a>

Properties:  
[`tfo:implementation`](https://vocab.methodandstructure.com/transformation#implementation)

</div>

</div>

<div class="section">

### Transforms

A `tfo:Function` is essentially a function definition, specifying input
and output constraints, and parameters in both named and positional
representations.

<div id="Function" class="section" about="[tfo:Function]"
typeof="owl:Class">

#### `Function`

This class provides a specification for a transformation function.

Subclass of:  
<a href="https://www.w3.org/TR/vocab-dcat-2/#Class:Data_Service"
rel="rdfs:subClassOf"
resource="dcat:DataService"><code>dcat:DataService</code></a>

<a href="https://www.w3.org/TR/prov-o/#Entity" rel="rdfs:subClassOf"
resource="prov:Entity"><code>prov:Entity</code></a>

Properties:  
[*`tfo:parameter`*](https://vocab.methodandstructure.com/transformation#parameter)

[*`tfo:parameter-list`*](https://vocab.methodandstructure.com/transformation#parameter-list)

[*`tfo:accepts`*](https://vocab.methodandstructure.com/transformation#accepts)

[*`tfo:returns`*](https://vocab.methodandstructure.com/transformation#returns)

[`tfo:prefers`](https://vocab.methodandstructure.com/transformation#prefers)

[`tfo:precedes`](https://vocab.methodandstructure.com/transformation#precedes)

[`tfo:follows`](https://vocab.methodandstructure.com/transformation#follows)

[`tfo:triggers`](https://vocab.methodandstructure.com/transformation#triggers)

[`tfo:by-uri`](https://vocab.methodandstructure.com/transformation#by-uri)

[`tfo:not-by-uri`](https://vocab.methodandstructure.com/transformation#not-by-uri)

</div>

<div id="MarkupTransform" class="section" about="[tfo:MarkupTransform]"
typeof="owl:Class">

#### ~~`MarkupTransform`~~

This class represents the set of transformation functions that operate
exclusively over (HTML/XML) markup.

This class has been deprecated since it only made sense in 2020 when
this vocabulary was being used in a different context.

Subclass of:  
<a href="https://vocab.methodandstructure.com/transformation#Function"
rel="rdfs:subClassOf"><code>tfo:Function</code></a>

Properties:  
[~~`tfo:by-xpath`~~](https://vocab.methodandstructure.com/transformation#by-xpath)

[~~`tfo:not-by-xpath`~~](https://vocab.methodandstructure.com/transformation#not-by-xpath)

</div>

</div>

<div class="section">

### Parameters

`tfo:Parameter` entities encapsulate name, type, and cardinality of a
given function parameter, while `tfo:ParameterList` entities specify
position.

<div id="Parameter" class="section" about="[tfo:Parameter]"
typeof="owl:Class">

#### `Parameter`

This class provides a specification for a parameter in a given function.

Subclass of:  
<a href="https://www.w3.org/TR/rdf-schema/#ch_property"
rel="rdfs:subClassOf"
resource="rdf:Property"><code>rdf:Property</code></a>

Property restrictions:  
<a href="https://www.w3.org/TR/rdf-schema/#ch_domain"
rel="owl:onProperty" resource="rdfs:domain"><code>rdfs:domain</code></a>
∈ <span rel="owl:allValuesFrom" resource="_:E0U5-E-2m20e9akyFS7CJK">
<span rel="owl:unionOf"
resource="_:ERYaN15IEBIX5V9Qjn1gSJ">(<a href="https://vocab.methodandstructure.com/transformation#Partial"
rel="rdf:first"><code>tfo:Partial</code></a> <span
about="_:ERYaN15IEBIX5V9Qjn1gSJ" rel="rdf:rest"
resource="_:Elw4xkTlnNqDG4rt5jhIUJ">∪</span>
<a href="https://vocab.methodandstructure.com/transformation#Bundle"
about="_:Elw4xkTlnNqDG4rt5jhIUJ"
rel="rdf:first"><code>tfo:Bundle</code></a><span
about="_:Elw4xkTlnNqDG4rt5jhIUJ" rel="rdf:rest"
resource="rdf:nil">)</span> </span> </span>

Properties:  
<a href="https://vocab.methodandstructure.com/transformation#default"
rev="rdfs:domain"><code>tfo:default</code></a>

</div>

<div id="ParameterList" class="section" about="[tfo:ParameterList]"
typeof="owl:Class">

#### `ParameterList`

This class represents a list with the restriction that its members be
tfo:Parameter nodes.

Subclass of:  
<a href="https://www.w3.org/TR/rdf-schema/#ch_list"
rel="rdfs:subClassOf" resource="rdf:List"><code>rdf:List</code></a>

Property restrictions:  
<a href="https://www.w3.org/TR/rdf-schema/#ch_first"
rel="owl:onProperty" resource="rdf:first"><code>rdf:first</code></a> ∈
<a href="https://vocab.methodandstructure.com/transformation#Parameter"
rel="owl:allValuesFrom"><code>tfo:Parameter</code></a>

<a href="https://www.w3.org/TR/rdf-schema/#ch_rest" rel="owl:onProperty"
resource="rdf:rest"><code>rdf:rest</code></a> ∈ <a
href="https://vocab.methodandstructure.com/transformation#ParameterList"
rel="owl:allValuesFrom"><code>tfo:ParameterList</code></a>

</div>

</div>

<div class="section">

### Queues

Queues are how we bundle sequences of `tfo:Function` (or `tfo:Partial`)
for execution. “Queue” is something of a misnomer, as the sequence of
transforms need not be explicitly asserted, but rather negotiated on the
fly.

<div id="Queue" class="section" about="[tfo:Queue]" typeof="owl:Class">

#### `Queue`

A queue is a collection of `tfo:Function` (and/or `tfo:Partial`)
elements, organized either by explicit sequence (via `tfo:member-list`),
or by dynamic sorting at runtime.

An inference rule that cannot be described in RDFS or OWL is that
`?q tfo:member-list ( ?a ?b ) .` implies `?q tfo:member ?a, ?b .`

Given that `tfo:Invocation` is a subclass of `tfo:Partial`, there is
nothing *in principle* preventing the former from being introduced into
a queue. If this happens, we ignore any `tfo:input` or `tfo:output`
statemments associated with the application of the function.

Subclass of:  
<a href="https://www.w3.org/TR/prov-o/#Activity" rel="rdfs:subClassOf"
resource="prov:Activity"><code>prov:Activity</code></a>

Properties:  
<a href="https://vocab.methodandstructure.com/transformation#member"
rev="rdfs:domain"><code>tfo:member</code></a>

<a
href="https://vocab.methodandstructure.com/transformation#member-list"
rev="rdfs:domain"><code>tfo:member-list</code></a>

<a href="https://vocab.methodandstructure.com/transformation#first"
rev="rdfs:domain"><code>tfo:first</code></a>

<a href="https://vocab.methodandstructure.com/transformation#last"
rev="rdfs:domain"><code>tfo:last</code></a>

<a href="https://vocab.methodandstructure.com/transformation#next"
rev="rdfs:domain"><code>tfo:next</code></a>

</div>

<div id="StrictQueue" class="section" about="[tfo:StrictQueue]"
typeof="owl:Class">

#### `StrictQueue`

A *strict* queue is one for which *all* its elements *must* be executed,
unlike an ordinary queue which only has to *attempt* to run its
contents.

Subclass of:  
<a href="https://vocab.methodandstructure.com/transformation#Queue"
rel="rdfs:subClassOf"><code>tfo:Queue</code></a>

</div>

<div id="AddressableQueue" class="section"
about="[tfo:AddressableQueue]" typeof="owl:Class">

#### `AddressableQueue`

An *addressable* queue is a strict queue intended to hold transforms
that have been invoked through the URL.

Subclass of:  
<a
href="https://vocab.methodandstructure.com/transformation#StrictQueue"
rel="rdfs:subClassOf"><code>tfo:StrictQueue</code></a>

</div>

<div id="FunctionList" class="section" about="[tfo:FunctionList]"
typeof="owl:Class">

#### `FunctionList`

A function list is a list that only holds `tfo:Function` or
`tfo:Partial` entities.

Subclass of:  
<a href="https://www.w3.org/TR/rdf-schema/#ch_list"
rel="rdfs:subClassOf" resource="rdf:List"><code>rdf:List</code></a>

Property restrictions:  
<a href="https://www.w3.org/TR/rdf-schema/#ch_first"
rel="owl:onProperty" resource="rdf:first"><code>rdf:first</code></a> ∈
<span rel="owl:allValuesFrom"><span about="_:uo40" rel="owl:unionOf"
resource="_:q40">
<a href="https://vocab.methodandstructure.com/transformation#Function"
about="_:q40" rel="rdf:first"><code>tfo:Function</code></a> <span
about="_:q40" rel="rdf:rest" resource="_:q41">∪</span>
<a href="https://vocab.methodandstructure.com/transformation#Partial"
about="_:q41" rel="rdf:first"><code>tfo:Partial</code></a> <span
about="_:q41" rel="rdf:rest" resource="rdf:nil"></span></span> </span>

<a href="https://www.w3.org/TR/rdf-schema/#ch_rest" rel="owl:onProperty"
resource="rdf:rest"><code>rdf:rest</code></a> ∈ <a
href="https://vocab.methodandstructure.com/transformation#FunctionList"
rel="owl:allValuesFrom"><code>tfo:FunctionList</code></a>

</div>

<div id="Insertion" class="section" about="[tfo:Insertion]"
typeof="owl:Class">

#### `Insertion`

An insertion is a pre-packaged event that manipulates a per-request
instance of a transformation queue. When the result of the transform
being run matches the condition, the contents of the `tfo:Insertion` are
inserted into the target queue.

Subclass of:  
<a href="https://vocab.methodandstructure.com/transformation#Queue"
rel="rdfs:subClassOf"><code>tfo:Queue</code></a>

Properties:  
<a href="https://vocab.methodandstructure.com/transformation#target"
rev="rdfs:domain"><code>tfo:condition</code></a>

<a href="https://vocab.methodandstructure.com/transformation#target"
rev="rdfs:domain"><code>tfo:target</code></a>

</div>

</div>

<div class="section">

### Function Invocation

Recording partial and full function invocations is important for
*memoizing* computations.

<div id="Invocation" class="section" about="[tfo:Invocation]"
typeof="owl:Class">

#### `Invocation`

This class represents an invocation of a transformation function,
connecting a specific input and scalar parameters with its output.

This class supersedes `tfo:Application`, which has been deprecated due
to being too easily confused with a software application versus the
application of a function.

Subclass of:  
<a href="https://vocab.methodandstructure.com/transformation#Partial"
rel="rdfs:subClassOf"><code>tfo:Partial</code></a>

Equivalent to:  
`tfo:Application`

Properties:  
<a href="https://vocab.methodandstructure.com/transformation#completes"
rev="rdfs:domain"><code>tfo:completes</code></a>

<a href="https://vocab.methodandstructure.com/transformation#input"
rev="rdfs:domain"><code>tfo:input</code></a>

<a href="https://vocab.methodandstructure.com/transformation#output"
rev="rdfs:domain"><code>tfo:output</code></a>

</div>

<div id="Partial" class="section" about="[tfo:Partial]"
typeof="owl:Class">

#### `Partial`

This class represents a *partial* application of a transformation
function, affording the encapsulation and re-use of existing parameters.

Subclass of:  
<a href="https://www.w3.org/TR/prov-o/#Activity" rel="rdfs:subClassOf"
resource="prov:Activity"><code>prov:Activity</code></a>

Properties:  
<a href="https://vocab.methodandstructure.com/transformation#transform"
rev="rdfs:domain"><code>tfo:transform</code></a>

</div>

</div>

</div>

<div class="section">

## Properties

![](https://vocab.methodandstructure.com/transformation-properties)

<div class="section">

### Specifying Transforms

These properties have to do with specifying `tfo:Function` entities and
their parameters.

<div id="accepts" class="section" about="[tfo:accepts]"
typeof="owl:ObjectProperty">

#### `accepts`

Specifies the list of content-types, in order of preference, that the
function can process.

Domain:  
<a href="https://www.w3.org/TR/vocab-dcat-2/#Class:Data_Service"
rel="rdfs:domain"
resource="dcat:DataService"><code>dcat:DataService</code></a>

Range:  
<span about="_:uo1" rel="owl:unionOf" resource="_:q1"> <a
href="https://vocab.methodandstructure.com/transformation#content-type"
about="_:q1" rel="rdf:first"><code>tfo:content-type</code></a> <span
about="_:q1" rel="rdf:rest" resource="_:q2">∪</span>
<a href="https://www.w3.org/TR/rdf-schema/#ch_list" about="_:q2"
rel="rdf:first" resource="rdf:List"><code>rdf:List</code></a> <span
about="_:q2" rel="rdf:rest" resource="rdf:nil"></span> </span>

<a href="https://vocab.methodandstructure.com/transformation#"
rel="rdfs:isDefinedBy">Back to Top</a>

</div>

<div id="returns" class="section" about="[tfo:returns]"
typeof="owl:ObjectProperty">

#### `returns`

Specifies the list of content-types, in order of preference, that the
function is capable of returning.

Domain:  
<a href="https://www.w3.org/TR/vocab-dcat-2/#Class:Data_Service"
rel="rdfs:domain"
resource="dcat:DataService"><code>dcat:DataService</code></a>

Range:  
<span about="_:uo2" rel="owl:unionOf" resource="_:q3"> <a
href="https://vocab.methodandstructure.com/transformation#content-type"
about="_:q3" rel="rdf:first"><code>tfo:content-type</code></a> <span
about="_:q3" rel="rdf:rest" resource="_:q4">∪</span>
<a href="https://www.w3.org/TR/rdf-schema/#ch_list" about="_:q4"
rel="rdf:first" resource="rdf:List"><code>rdf:List</code></a> <span
about="_:q4" rel="rdf:rest" resource="rdf:nil"></span> </span>

<a href="https://vocab.methodandstructure.com/transformation#"
rel="rdfs:isDefinedBy">Back to Top</a>

</div>

<div id="prefers" class="section" about="[tfo:prefers]"
typeof="owl:DatatypeProperty owl:FunctionalProperty">

#### `prefers`

Specifies the `tfo:content-type` the transform *prefers* to emit in lieu
of a preference specified by the request. May be a list.

Subproperty of:  
<a href="https://vocab.methodandstructure.com/transformation#returns"
rel="rdfs:subPropertyOf"><code>tfo:returns</code></a>

Domain:  
<a href="https://vocab.methodandstructure.com/transformation#Function"
rel="rdfs:domain"><code>tfo:Function</code></a>

Range:  
<span about="_:uo3" rel="owl:unionOf" resource="_:q5"> <a
href="https://vocab.methodandstructure.com/transformation#content-type"
about="_:q5" rel="rdf:first"><code>tfo:content-type</code></a> <span
about="_:q5" rel="rdf:rest" resource="_:q6">∪</span>
<a href="https://www.w3.org/TR/rdf-schema/#ch_list" about="_:q6"
rel="rdf:first" resource="rdf:List"><code>rdf:List</code></a> <span
about="_:q6" rel="rdf:rest" resource="rdf:nil"></span> </span>

<a href="https://vocab.methodandstructure.com/transformation#"
rel="rdfs:isDefinedBy">Back to Top</a>

</div>

<div id="implementation" class="section" about="[tfo:implementation]"
typeof="owl:ObjectProperty owl:FunctionalProperty">

#### `implementation`

URI to the implementation of the function.

The URI is meant to be dereferenced by an internal implementation, e.g.
`file:`, `jar:`, or an idiosyncratic scheme like `urn:x-python:`.

Domain:  
<a href="https://vocab.methodandstructure.com/transformation#Bundle"
rel="rdfs:domain"><code>tfo:Bundle</code></a>

Range:  
<a href="https://www.w3.org/TR/rdf-schema/#ch_resource" rel="rdfs:range"
resource="rdfs:Resource"><code>rdfs:Resource</code></a>

<a href="https://vocab.methodandstructure.com/transformation#"
rel="rdfs:isDefinedBy">Back to Top</a>

</div>

<div id="parameter" class="section" about="[tfo:parameter]"
typeof="owl:ObjectProperty">

#### `parameter`

Binds a parameter object to its function.

Parameters can either be supplied to the function as key-value pairs or
sequentially.

Domain:  
<a href="https://www.w3.org/TR/vocab-dcat-2/#Class:Data_Service"
rel="rdfs:domain"
resource="dcat:DataService"><code>dcat:DataService</code></a>

Range:  
<a href="https://vocab.methodandstructure.com/transformation#Parameter"
rel="rdfs:range"><code>tfo:Parameter</code></a>

<a href="https://vocab.methodandstructure.com/transformation#"
rel="rdfs:isDefinedBy">Back to Top</a>

</div>

<div id="parameter-list" class="section" about="[tfo:parameter-list]"
typeof="owl:ObjectProperty owl:FunctionalProperty">

#### `parameter-list`

Specifies the sequence of parameters when the invocation method of the
function is sequential.

Domain:  
<a href="https://www.w3.org/TR/vocab-dcat-2/#Class:Data_Service"
rel="rdfs:domain"
resource="dcat:DataService"><code>dcat:DataService</code></a>

Range:  
<a
href="https://vocab.methodandstructure.com/transformation#ParameterList"
rel="rdfs:range"><code>tfo:ParameterList</code></a>

<a href="https://vocab.methodandstructure.com/transformation#"
rel="rdfs:isDefinedBy">Back to Top</a>

</div>

<div id="default" class="section" about="[tfo:default]"
typeof="owl:ObjectProperty">

#### `default`

Specifies one or more default values for a parameter.

Domain:  
<a href="https://vocab.methodandstructure.com/transformation#Parameter"
rel="rdfs:domain"><code>tfo:Parameter</code></a>

<a href="https://vocab.methodandstructure.com/transformation#"
rel="rdfs:isDefinedBy">Back to Top</a>

</div>

</div>

<div class="section">

### Specifying Queues

These properties all have to do with providing hints to topological
sorts of transforms within queues, or otherwise explicit sequences.

<div id="member" class="section" about="[tfo:member]"
typeof="owl:ObjectProperty">

#### `member`

Denotes a member of a queue.

Domain:  
<a href="https://vocab.methodandstructure.com/transformation#Queue"
rel="rdfs:domain"><code>tfo:Queue</code></a>

Range:  
<span about="_:uo4" rel="owl:unionOf" resource="_:q7">
<a href="https://vocab.methodandstructure.com/transformation#Function"
about="_:q7" rel="rdf:first"><code>tfo:Function</code></a> <span
about="_:q7" rel="rdf:rest" resource="_:q8">∪</span>
<a href="https://vocab.methodandstructure.com/transformation#Partial"
about="_:q8" rel="rdf:first"><code>tfo:Partial</code></a> <span
about="_:q8" rel="rdf:rest" resource="rdf:nil"></span> </span>

<a href="https://vocab.methodandstructure.com/transformation#"
rel="rdfs:isDefinedBy">Back to Top</a>

</div>

<div id="member-list" class="section" about="[tfo:member-list]"
typeof="owl:ObjectProperty">

#### `member-list`

Denotes an explicit member list for a queue.

Domain:  
<a href="https://vocab.methodandstructure.com/transformation#Queue"
rel="rdfs:domain"><code>tfo:Queue</code></a>

Range:  
<a
href="https://vocab.methodandstructure.com/transformation#FunctionList"
rel="rdfs:range"><code>tfo:FunctionList</code></a>

<a href="https://vocab.methodandstructure.com/transformation#"
rel="rdfs:isDefinedBy">Back to Top</a>

</div>

<div id="precedes" class="section" about="[tfo:precedes]"
typeof="owl:ObjectProperty">

#### `precedes`

Specifies one or more `tfo:Function` that the subject must *precede* in
a queue.

Domain:  
<a href="https://vocab.methodandstructure.com/transformation#Function"
rel="rdfs:domain"><code>tfo:Function</code></a>

Range:  
<a href="https://vocab.methodandstructure.com/transformation#Function"
rel="rdfs:range"><code>tfo:Function</code></a>

Inverse of:  
<a href="https://vocab.methodandstructure.com/transformation#follows"
rel="owl:inverseOf"><code>tfo:follows</code></a>

<a href="https://vocab.methodandstructure.com/transformation#"
rel="rdfs:isDefinedBy">Back to Top</a>

</div>

<div id="follows" class="section" about="[tfo:follows]"
typeof="owl:ObjectProperty">

#### `follows`

Specifies one or more `tfo:Function` that the subject must *follow* in a
queue.

Domain:  
<a href="https://vocab.methodandstructure.com/transformation#Function"
rel="rdfs:domain"><code>tfo:Function</code></a>

Range:  
<a href="https://vocab.methodandstructure.com/transformation#Function"
rel="rdfs:range"><code>tfo:Function</code></a>

Inverse of:  
<a href="https://vocab.methodandstructure.com/transformation#precedes"
rel="owl:inverseOf"><code>tfo:precedes</code></a>

<a href="https://vocab.methodandstructure.com/transformation#"
rel="rdfs:isDefinedBy">Back to Top</a>

</div>

<div id="next" class="section" about="[tfo:next]"
typeof="owl:ObjectProperty owl:FunctionalProperty">

#### `next`

Specifies the next queue to run after this one.

Domain:  
<a href="https://vocab.methodandstructure.com/transformation#Queue"
rel="rdfs:domain"><code>tfo:Queue</code></a>

Range:  
<a href="https://vocab.methodandstructure.com/transformation#Queue"
rel="rdfs:range"><code>tfo:Queue</code></a>

<a href="https://vocab.methodandstructure.com/transformation#"
rel="rdfs:isDefinedBy">Back to Top</a>

</div>

<div id="first" class="section" about="[tfo:first]"
typeof="owl:ObjectProperty owl:FunctionalProperty">

#### `first`

Explicitly specifies a transform that must go *first*.

Sub-property of:  
<a href="https://vocab.methodandstructure.com/transformation#member"
rel="rdfs:subPropertyOf"><code>tfo:member</code></a>

Domain:  
<a href="https://vocab.methodandstructure.com/transformation#Queue"
rel="rdfs:domain"><code>tfo:Queue</code></a>

Range:  
<span about="_:uo41" rel="owl:unionOf" resource="_:q42">
<a href="https://vocab.methodandstructure.com/transformation#Function"
about="_:q42" rel="rdf:first"><code>tfo:Function</code></a> <span
about="_:q42" rel="rdf:rest" resource="_:q43">∪</span>
<a href="https://vocab.methodandstructure.com/transformation#Partial"
about="_:q43" rel="rdf:first"><code>tfo:Partial</code></a> <span
about="_:q43" rel="rdf:rest" resource="rdf:nil"></span> </span>

<a href="https://vocab.methodandstructure.com/transformation#"
rel="rdfs:isDefinedBy">Back to Top</a>

</div>

<div id="last" class="section" about="[tfo:last]"
typeof="owl:ObjectProperty owl:FunctionalProperty">

#### `last`

Explicitly specifies a transform that must go *last*.

Sub-property of:  
<a href="https://vocab.methodandstructure.com/transformation#member"
rel="rdfs:subPropertyOf"><code>tfo:member</code></a>

Domain:  
<a href="https://vocab.methodandstructure.com/transformation#Queue"
rel="rdfs:domain"><code>tfo:Queue</code></a>

Range:  
<span about="_:uo42" rel="owl:unionOf" resource="_:q44">
<a href="https://vocab.methodandstructure.com/transformation#Function"
about="_:q44" rel="rdf:first"><code>tfo:Function</code></a> <span
about="_:q44" rel="rdf:rest" resource="_:q45">∪</span>
<a href="https://vocab.methodandstructure.com/transformation#Partial"
about="_:q45" rel="rdf:first"><code>tfo:Partial</code></a> <span
about="_:q45" rel="rdf:rest" resource="rdf:nil"></span> </span>

<a href="https://vocab.methodandstructure.com/transformation#"
rel="rdfs:isDefinedBy">Back to Top</a>

</div>

</div>

<div class="section">

### Manipulating Queues at Runtime

These properties relate trnasforms to queue insertion events.

<div id="triggers" class="section" about="[tfo:triggers]"
typeof="owl:ObjectProperty">

#### `triggers`

A `tfo:Function` can trigger an `tfo:Insertion` event on a certain
condition (e.g., successful completion).

Domain:  
<a href="https://vocab.methodandstructure.com/transformation#Function"
rel="rdfs:domain"><code>tfo:Function</code></a>

Range:  
<a href="https://vocab.methodandstructure.com/transformation#Insertion"
rel="rdfs:range"><code>tfo:Insertion</code></a>

<a href="https://vocab.methodandstructure.com/transformation#"
rel="rdfs:isDefinedBy">Back to Top</a>

</div>

<div id="target" class="section" about="[tfo:target]"
typeof="owl:ObjectProperty">

#### `target`

A target queue for the insertion.

Domain:  
<a href="https://vocab.methodandstructure.com/transformation#Insertion"
rel="rdfs:domain"><code>tfo:Insertion</code></a>

Range:  
<a href="https://vocab.methodandstructure.com/transformation#Queue"
rel="rdfs:range"><code>tfo:Queue</code></a>

<a href="https://vocab.methodandstructure.com/transformation#"
rel="rdfs:isDefinedBy">Back to Top</a>

</div>

<div id="condition" class="section" about="[tfo:condition]"
typeof="owl:ObjectProperty">

#### `condition`

An HTTP status code for which the insertion is triggered.

Domain:  
<a href="https://vocab.methodandstructure.com/transformation#Insertion"
rel="rdfs:domain"><code>tfo:Insertion</code></a>

Range:  
<a href="https://www.w3.org/TR/HTTP-in-RDF10/#StatusCodeClass"
rel="rdfs:range"
resource="httpv:StatusCode"><code>http:StatusCode</code></a>

<a href="https://vocab.methodandstructure.com/transformation#"
rel="rdfs:isDefinedBy">Back to Top</a>

</div>

</div>

<div class="section">

### Applying Transformations

This group of properties is responsible for relating `tfo:Function` to
the actual data objects they transform.

<div id="completes" class="section" about="[tfo:completes]"
typeof="owl:ObjectProperty owl:FunctionalProperty">

#### `completes`

Identifies a `tfo:Partial` function that this `tfo:Invocation`
completes.

Domain:  
<a href="https://vocab.methodandstructure.com/transformation#Invocation"
rel="rdfs:domain"><code>tfo:Invocation</code></a>

Range:  
<a href="https://vocab.methodandstructure.com/transformation#Partial"
rel="rdfs:range"><code>tfo:Partial</code></a>

<a href="https://vocab.methodandstructure.com/transformation#"
rel="rdfs:isDefinedBy">Back to Top</a>

</div>

<div id="transform" class="section" about="[tfo:transform]"
typeof="owl:ObjectProperty owl:FunctionalProperty">

#### `transform`

Specifies the transform associated with this particular invocation.

Subproperty of:  
<a href="https://www.w3.org/TR/prov-o/#used" rel="rdfs:PropertyOf"
resource="prov:used"><code>prov:used</code></a>

Domain:  
<a href="https://vocab.methodandstructure.com/transformation#Partial"
rel="rdfs:domain"><code>tfo:Partial</code></a>

Range:  
<a href="https://vocab.methodandstructure.com/transformation#Function"
rel="rdfs:range"><code>tfo:Function</code></a>

<a href="https://vocab.methodandstructure.com/transformation#"
rel="rdfs:isDefinedBy">Back to Top</a>

</div>

<div id="input" class="section" about="[tfo:input]"
typeof="owl:ObjectProperty owl:FunctionalProperty">

#### `input`

Specifies the resource that was the input of the transformation
function.

Subproperty of:  
<a href="https://www.w3.org/TR/prov-o/#used" rel="rdfs:PropertyOf"
resource="prov:used"><code>prov:used</code></a>

Domain:  
<a href="https://vocab.methodandstructure.com/transformation#Invocation"
rel="rdfs:domain"><code>tfo:Invocation</code></a>

Range:  
<a href="https://www.w3.org/TR/rdf-schema/#ch_resource" rel="rdfs:range"
resource="rdfs:Resource"><code>rdfs:Resource</code></a>

<a href="https://vocab.methodandstructure.com/transformation#"
rel="rdfs:isDefinedBy">Back to Top</a>

</div>

<div id="output" class="section" about="[tfo:output]"
typeof="owl:ObjectProperty owl:FunctionalProperty">

#### `output`

Specifies the resource that was the output of the transformation
function.

Domain:  
<a href="https://vocab.methodandstructure.com/transformation#Invocation"
rel="rdfs:domain"><code>tfo:Invocation</code></a>

Range:  
<a href="https://www.w3.org/TR/rdf-schema/#ch_resource" rel="rdfs:range"
resource="rdfs:Resource"><code>rdfs:Resource</code></a>

<a href="https://vocab.methodandstructure.com/transformation#"
rel="rdfs:isDefinedBy">Back to Top</a>

</div>

</div>

<div class="section">

### Constraints

In certain contexts it is useful to pre-declare constraints on what
constitutes valid input for a transform. We can do this either by URI or
by XPath.

<div id="by-uri" class="section" about="[tfo:by-uri]"
typeof="owl:DatatypeProperty">

#### `by-uri`

Specifies a regular expression for matching against URIs.

Domain:  
<a href="https://vocab.methodandstructure.com/transformation#Function"
rel="rdfs:domain"><code>tfo:Function</code></a>

Range:  
<a href="https://vocab.methodandstructure.com/transformation#regexp"
rel="rdfs:range"><code>tfo:regexp</code></a>

<a href="https://vocab.methodandstructure.com/transformation#"
rel="rdfs:isDefinedBy">Back to Top</a>

</div>

<div id="not-by-uri" class="section" about="[tfo:not-by-uri]"
typeof="owl:DatatypeProperty">

#### `not-by-uri`

Specifies a regular expression for *anti*-matching against URIs.

Domain:  
<a href="https://vocab.methodandstructure.com/transformation#Function"
rel="rdfs:domain"><code>tfo:Function</code></a>

Range:  
<a href="https://vocab.methodandstructure.com/transformation#regexp"
rel="rdfs:range"><code>tfo:regexp</code></a>

<a href="https://vocab.methodandstructure.com/transformation#"
rel="rdfs:isDefinedBy">Back to Top</a>

</div>

<div id="prefix" class="section" about="[tfo:prefix]"
typeof="owl:ObjectProperty">

#### `prefix`

Specifies a SHACL prefix declaration for complementing any associated
XPath expression.

Note that the existing `sh:declare` property has a domain of
`owl:Ontology` so we can't use it for this.

Domain:  
<a
href="https://vocab.methodandstructure.com/transformation#MarkupTransform"
rel="rdfs:domain"><code>tfo:MarkupTransform</code></a>

Range:  
<a href="https://www.w3.org/TR/shacl/#sparql-prefixes" rel="rdfs:range"
resource="sh:PrefixDeclaration"><code>sh:PrefixDeclaration</code></a>

<a href="https://vocab.methodandstructure.com/transformation#"
rel="rdfs:isDefinedBy">Back to Top</a>

</div>

<div id="by-xpath" class="section" about="[tfo:by-xpath]"
typeof="owl:DatatypeProperty">

#### ~~`by-xpath`~~

Specifies an XPath expression for matching against markup (HTML/XML)
content.

Domain:  
<a
href="https://vocab.methodandstructure.com/transformation#MarkupTransform"
rel="rdfs:domain"><del><code>tfo:MarkupTransform</code></del></a>

Range:  
<a href="https://vocab.methodandstructure.com/transformation#xpath"
rel="rdfs:range"><del><code>tfo:xpath</code></del></a>

<a href="https://vocab.methodandstructure.com/transformation#"
rel="rdfs:isDefinedBy">Back to Top</a>

</div>

<div id="not-by-xpath" class="section" about="[tfo:not-by-xpath]"
typeof="owl:DatatypeProperty">

#### ~~`not-by-xpath`~~

Specifies an XPath expression for *anti*-matching against markup
(HTML/XML) content.

Domain:  
<a
href="https://vocab.methodandstructure.com/transformation#MarkupTransform"
rel="rdfs:domain"><del><code>tfo:MarkupTransform</code></del></a>

Range:  
<a href="https://vocab.methodandstructure.com/transformation#xpath"
rel="rdfs:range"><del><code>tfo:xpath</code></del></a>

<a href="https://vocab.methodandstructure.com/transformation#"
rel="rdfs:isDefinedBy">Back to Top</a>

</div>

</div>

</div>

<div class="section">

## Datatypes

Currently there is only the datatype for designating [content
types](https://www.iana.org/assignments/media-types/media-types.xhtml),
and those for regular expressions and XPath.

<div id="content-type" class="section" about="[tfo:content-type]"
typeof="rdfs:Datatype">

### `content-type`

A literal that represents a content-type such as that which is found in
the HTTP `Accept:` or `Content-Type:` header.

Restriction of:  
<a href="https://www.w3.org/TR/xmlschema-2/#token" rel="owl:onDatatype"
resource="xsd:token"><code>xsd:token</code></a> <span
rel="owl:withRestrictions" resource="_:rl1">matching</span> <span
about="_:rl1"
rel="rdf:first">`` ^([!#$%&'\*\+\-.^_`|~0-9-A-Za-z]+)(?:/[!#$%&'\*\+\-.^_`|~0-9-A-Za-z]+)?)$ ``</span>
<span about="_:rl1" rel="rdf:rest" resource="rdf:nil"></span>

See also:  
<a href="https://tools.ietf.org/html/rfc7230#section-3.2.6"
rel="rdfs:seeAlso">RFC 7230 — Hypertext Transfer Protocol (HTTP/1.1):
Message Syntax and Routing § 3.2.6: Field Value Components</a>

<a href="https://tools.ietf.org/html/rfc7231#section-5.3.2"
rel="rdfs:seeAlso">RFC 7231 — Hypertext Transfer Protocol (HTTP/1.1):
Semantics and Content § 5.3.2: <code>Accept</code></a>

<a href="https://vocab.methodandstructure.com/transformation#"
rel="rdfs:isDefinedBy">Back to Top</a>

</div>

<div id="regexp" class="section" about="[tfo:regexp]"
typeof="rdfs:Datatype">

### `regexp`

A regular expression.

We do not specify a flavour, but in practice we should assume PCRE or
ECMA-262.

Restriction of:  
<a href="https://www.w3.org/TR/xmlschema-2/#string" rel="owl:onDatatype"
resource="xsd:string"><code>xsd:string</code></a>

<a href="https://vocab.methodandstructure.com/transformation#"
rel="rdfs:isDefinedBy">Back to Top</a>

</div>

<div id="iregexp" class="section" about="[tfo:iregexp]"
typeof="rdfs:Datatype">

### `iregexp`

A case-insensitive regular expression.

Note that we do not intend to provide the full complement of flags for
regular expressions like `m`, `s`, or `x`.

Subclass of:  
<a href="https://vocab.methodandstructure.com/transformation#regexp"
rel="rdfs:subClassOf"><code>tfo:regexp</code></a>

Restriction of:  
<a href="https://www.w3.org/TR/xmlschema-2/#string" rel="owl:onDatatype"
resource="xsd:string"><code>xsd:string</code></a>

<a href="https://vocab.methodandstructure.com/transformation#"
rel="rdfs:isDefinedBy">Back to Top</a>

</div>

<div id="xpath" class="section" about="[tfo:xpath]"
typeof="rdfs:Datatype">

### ~~`xpath`~~

An XPath expression.

Restriction of:  
<a href="https://www.w3.org/TR/xmlschema-2/#string" rel="owl:onDatatype"
resource="xsd:string"><code>xsd:string</code></a>

<a href="https://vocab.methodandstructure.com/transformation#"
rel="rdfs:isDefinedBy">Back to Top</a>

</div>

</div>

<div class="section">

## References

-   <a href="https://www.w3.org/TR/prov-o/" rel="dct:references">PROV-O: The
    Provenance Ontology</a>
-   <a href="https://www.w3.org/TR/rdf-schema/" rel="dct:references">RDF
    Schema 1.1</a>
-   <a href="https://www.w3.org/TR/xmlschema-2/" rel="dct:references">XML
    Schema Part 2: Datatypes Second Edition</a>
-   <a href="https://www.iana.org/assignments/media-types/media-types.xhtml"
    rel="dct:references">Media Types (IANA)</a>

</div>
