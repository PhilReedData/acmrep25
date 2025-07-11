## Overview
> 
> ### Objectives
> - Construct an RO-Crate by hand using JSON
> - Describe each part of the Research Object
> - Learn basic JSON-LD to create FAIR metadata
> - Connect different parts of the Research Object using identifiers



# 1. Introduction

This tutorial assumes you have already viewed [Introduction to RO-Crate](https://docs.google.com/presentation/d/1d-3PnEytZhhjSCKwit1wdcdS6cV7ALWG/edit?slide=id.p1)


## Tutorial walk-through

In this tutorial, meant to be read along with the [RO-Crate specification](https://www.researchobject.org/ro-crate/specification/1.2/),
we'll walk through the initial steps for creating a basic RO-Crate.
You are invited to replicate the below steps on your local computer.


> ### Abbreviations
> - FAIR: Findable, Accessible, Interoperable, Reusable; a set of principles for publishing research data and metadata.
> - FDO: FAIR Digital Object; a set of recommendations to improve findability, accessibility, interoperability, and reproducibility for any digital object.
> - JSON: JavaScript Object Notation, a generic structured text-based data format.
> - JSON-LD: JSON Linked Data, a way to express Linked Data (RDF) using regular JSON.
> - RO-Crate: Research Object Crate; a way to package research data with structured FAIR metadata.
> - PID: Persistent Identifier; a long-lasting reference to a digital object.
> - URI: Uniform Resource Identifier; a string of characters that identifies a resource.
> 
> ### Key Points
> - RO-Crate provides a structure to make FAIR data packages
> - schema.org in JSON-LD provides a controlled vocabulary for FAIR metadata
> - Each entity of the crate is described separately
> - Cross-references between entities create a graph
> - The RO-Crate specification recommends which types and keys to use
---
# 2. Turning a folder into an RO-Crate
> ### Objectives
> - Creating a skeleton RO-Crate Metadata File
> - Use the JSON-LD pre-amble to enable Linked Data
> ### Questions
> - How can I start a new RO-Crate?


## Turning a folder into an RO-Crate

To convert a folder into an RO-Crate, the fundamental step involves placing an RO-Crate Metadata File, specifically named ro-crate-metadata.json, within that folder. This file serves as the central descriptor for all content within the folder (and its subfolders), which is then considered the RO-Crate Root.

First create a new folder `crate1/`
and add a single file `data.csv` to represent our dataset:

```
"Date","Minimum temperature (°C)","Maximum temperature (°C)","Rainfall (mm)"
2022-02-01,16.0,28.4,0.6
2022-02-02,16.3,17.2,12.4
```


Next, to turn this folder into an RO-Crate,
we need to add the _RO-Crate Metadata File_, which has a fixed filename.
Create the file `ro-crate-metadata.json`
using [Visual Studio Code](https://code.visualstudio.com/) or your favourite editor,
then add the following JSON:

```json
{
  "@context": "https://w3id.org/ro/crate/1.2/context",
  "@graph": [

  ]
}
```

Your folder should now look like this:

![Folder listing of crate1, including data.csv and ro-crate-metadata.json](https://www.researchobject.org/packaging_data_with_ro-crate/fig/crate1-folders.svg)

The presence of the reserved `ro-crate-metadata.json` filename
means that `crate1` (and its children) can now be considered to be an **RO-Crate**.
We call the top-level folder of the crate for the **RO-Crate Root**
and can now refer to its content with relative file paths.

We also need to make some declaration within the JSON file to turn it into a valid _RO-Crate Metadata Document_,
explained in the next session.


## JSON-LD preamble

The preamble of `@context` and `@graph` are JSON-LD structures
that help provide global identifiers to the JSON keys and types
used in the rest of the RO-Crate document.
These will largely map to definitions in the [schema.org](http://schema.org/) vocabulary,
which can be used by RO-Crate extensions to provide additional metadata beyond the RO-Crate specifications.
It is this feature of JSON-LD that helps make RO-Crate extensible for many different purposes
-- this is explored further in the specification's [appendix on JSON-LD](https://www.researchobject.org/ro-crate/specification/1.2/appendix/).
In short, only JSON keys (_properties_) and types defined this way can be used within the RO-Crate Metadata Document.

However, in the general case it should be sufficient to follow the RO-Crate JSON examples directly without deeper JSON-LD understanding.
The RO-Crate Metadata Document contains a flat list of _entities_ as JSON objects in the `@graph` array.
These entities are cross-referenced using `@id` identifiers, rather than being deeply nested.
This is one major difference from JSON structures you may have experienced before.
The `@type` keyword associates an object with a predefined type from the JSON-LD context.
Almost any property can alternatively be used with an `[]` array to provide multiple values.

The rest of this tutorial, and indeed most of the [RO-Crate specification](https://www.researchobject.org/ro-crate/specification/1.2/),specify which entities can be added to the `@graph` array. 
## Key Points
- Adding a RO-Crate Metadata file to a folder turns it into an RO-Crate
- The RO-Crate Root is the top-level folder of the crate
- RO-Crate uses schema.org as base vocabulary
- The JSON-LD context enables optional Linked Data processing
- Descriptions are listed flatly as entities in the @graph array
---
# 3. Making a metadata descriptor

> ### Questions
> - Which RO-Crate version is used?
> - How can the crate self-identify as an RO-Crate?O
> 
> ### Objectives
> - Add the first entity to the JSON-LD @graph
> - Indicate the version of RO-Crate


## RO-Crate Metadata descriptor 

The first JSON-LD _entity_ to add in the `@graph` array has the `@id` value of `ro-crate-metadata.json` to describe the JSON file itself:


```json
{
    "@id": "ro-crate-metadata.json",
    "@type": "CreativeWork",
    "conformsTo": {"@id": "https://w3id.org/ro/crate/1.2"},
    "about": {"@id": "./"}
}
```

This required entity, known as the [RO-Crate Metadata Descriptor](https://www.researchobject.org/ro-crate/specification/1.2/root-data-entity.html#ro-crate-metadata-descriptor),
helps this file self-identify as an RO-Crate Metadata Document,
which is conforming to (`conformsTo`) the RO-Crate specification version 1.2.
Notice that the `conformsTo` URL corresponds to the `@context` URL version-wise,
but they have two different functions.
The context brings the defined terms into the metadata document,
while the conformance declares which RO-Crate conventions of using those terms are being followed.


## RO-Crate versions
This tutorial is written for RO-Crate 1.2,
the RO-Crate website will list the [current specification version](https://www.researchobject.org/ro-crate/specification.html)
-- RO-Crates can generally be upgraded to newer versions following [semantic versioning](https://semver.org/) conventions,
but check the [change log](https://www.researchobject.org/ro-crate/specification/1.2/appendix/changelog.html) for any important changes.
The next development version of the specification, indicated with a `-DRAFT` status,
may still be subject to changes and should only be used with caution.

## Key Points 
- The RO-Crate Metadata Descriptor describes the JSON-LD file itself
- RO-Crate specifications are versioned
- The version of RO-Crate is indicated using the conformsTo property

---
# 4. Declaring the root folder

> ### Questions
> - What is the root folder?
> ### Objectives
> - Create a top-level entity that can list the parts of the crate


## RO-Crate Root

Next we'll add another entity to the `@graph` array,
to describe the [RO-Crate Root](https://www.researchobject.org/ro-crate/specification/1.2/appendix/changelog.html):

```json
{
    "@id": "./",
    "@type": "Dataset",
    "hasPart": [ 

    ]
}
```

> ## Adding entities to the JSON array
> 
> Because we're adding incrementally to the `@graph` array. It is
> important to remember the comma `,` between each entity,
> **except** for the final entity in the JSON array; and likewise for the properties within the JSON object for each entity. This is an
> artefact of the strict [JSON](https://www.json.org/) file format rules
> to simplify parsing. The order of the entities within the `@graph`
> JSON-LD array and the order of the keys within a JSON object is _not
> significant_. The _graph_ content is given by the `@id`
> cross-references.
> 
> You will add a comma here between the `ro-crate-metadata.json` entity,
> and the root data entity.


By convention, in RO-Crate the `@id` value of  `./` means that this entity describes the folder in which the RO-Crate metadata file is located.
This reference from `ro-crate-metadata.json` is therefore semantically marking the `crate1` folder as being the RO-Crate Root.



## RO-Crates can be published on the Web
 
This example is a folder-based RO-Crate stored on disk,
and therefore absolute paths are avoided,
e.g. in case the root folder is moved or archived as a ZIP file. 
 
If the crate is being served from a Web service,
such as a data repository or database where files are not organized in folders,
then the `@id` might be an absolute URI instead of `./`
-- this is one reason why we point to the root entity from the metadata descriptor,
see section [Root Data Entity](https://www.researchobject.org/ro-crate/specification/1.2/root-data-entity.html) for details.

## Key Points
- The RO-Crate Root is the top-level object of the RO-Crate
- The root identifier may be a URL, but commonly just ./ for the current folder

# 5. Describing the root entity

> ### Questions
> - How can I describe the crate?
> - How do I specify the license of the RO-Crate?
> ### Objectives
> - Learn about required metadata for the RO-Crate Root
> - Understand license identifiers using SPDX


## Describing the root entity

When describing the [root entity](https://www.researchobject.org/ro-crate/specification/1.2/root-data-entity.html#direct-properties-of-the-root-data-entity),
the properties generally apply to the whole of the crate.
For instance it is a good idea to give a description of why these resources are gathered in a crate,
as well as giving the crate a name and license for FAIR reuse and citation.


## Add metadata to root entity

Try to add the `name`, `description` and `datePublished` properties,
and for `license` as a cross-reference,
use [SPDX](https://spdx.org/licenses/) license list to find the identifier for Creative Commons Zero
or another license of your choice:

:::::::::::::::  solution
```json
{
  "@id": "./",
  "@type": "Dataset",
  "hasPart": [ ],
  "name": "Example crate",
  "description": "I created this example by following the tutorial",
  "datePublished": "2023-05-22T12:03:00+0100",
  "license": { "@id": "http://spdx.org/licenses/CC0-1.0"}  
}
```

> ## License identifiers
> 
> In the above solution, the identifier for CC0-1.0
> <http://spdx.org/licenses/CC0-1.0> is slightly different from their
> listed web page URI <https://spdx.org/licenses/CC0-1.0.html>
> -- the former is chosen to align with [SPDX JSON-LD identifiers](https://github.com/spdx/license-list-data/tree/main/jsonld),
> which unfortunately are not shown directly on their website as
> _permalinks_.  It is not a requirement in RO-Crate to use permalinks for `@id` of entities like licenses,  it is nevertheless best practice
> to propagate permalinks where known.

## Choosing a license

Choosing a license appropriate for your dataset can be non-trivial,
particularly if third-party data/software and multiple organizations are involved.
See [FAIR Cookbook on licensing](https://faircookbook.elixir-europe.org/content/recipes/reusability/ATI-licensing.html).
It is worth noting that an RO-Crate permits data entities to have a `license` different from the overall Crate license.
It is still recommended to choose an overall Crate license that can legally apply across all the content in the RO-Crate Root.
## Key Points
- Name, description, date published and license are required for the RO-Crate Root"
- RO-Crate allows multiple licenses for different parts

# 6. Adding cross-references

> ### Questions
> - How can I describe an entity further?
> - How can I cross-reference different entities?
> ### Objectives
> - Understand cross-references in flattened JSON-LD
> - Add a data entity reference from the root entity
> - Add another type to the root entity



## About cross-references

In a RO-Crate Metadata Document,
entities are cross-referenced using `@id` reference objects,
rather than using deeply nested JSON objects.
In short, this _flattened JSON-LD_ style (shown below) allows any entity to reference any other entity,
and RO-Crate consumers can directly find all the descriptions of a given entity as a single JSON object. 

![JSON block with id `ro-crate-metadata.json` has some attributes, `conformsTo` RO-Crate 1.2, and `about` referencing id `./`. In second JSON block with id <code>./</code> we see additional attributes such as its name and description.](fig/introduction-figure-1.svg){alt="showing RO-Crate Metadata descriptor's `about` property pointing at the RO-Crate Root entity with matching `@id`"}

## Add cross-reference to data entity

Consider the root Data Entity `./`,
and add such a cross-reference to the file `data.csv` using the _property_ called `hasPart`:

```json
{
   "@id": "./",
   "@type": "Dataset",
   "hasPart": [ 
       {"@id": "data.csv"} 
   ],
   "…": "…"
}
```

The RO-Crate root is always typed `Dataset`,
though `@type` may in some cases have additional types by using a JSON array instead of a single value.
Most entities can have such more specific types,
e.g. chosen from [schema.org type list](https://schema.org/docs/full.html).

## Add an additional type

1. Navigate the schema.org type list to find a subtype of `CreativeWork` that is suitable for a learning resource.
2. Modify the root entity's `@type` to be an array.
3. Add the type name for learning resource at the end of the array.


```json
{
   "@id": "./",
   "@type": ["Dataset", "LearningResource"],
   "hasPart": [ 
     {"@id": "data.csv"} 
   ],
   "…": "…"
}
```

The root has several metadata properties that describe the RO-Crate as a whole,
considering it as a Research Object of collected resources.
The section on [root data entity](https://www.researchobject.org/ro-crate/specification/1.2/root-data-entity.html)
details further the required and recommended properties of the root `./`. 

## Key Points
- The @id uniquely identifies the entity within the RO-Crate
- The @id key is used for cross-referencing
- Multiple types can be listed by using an array

# 7. Data entities

> ### Questions
> - How do I describe the files in my RO-Crate?
> ### Objectives
> - Understand the purpose of data entities
> - Learn required properties for data entities


## Data entities

A main type of resources collected in a Research Object is _data_
-- simplifying, we can consider data as any kind of file that can be opened in other programs.
These are aggregated by the Root Dataset with the `hasPart` property.
In this example we have an array with a single value,
a reference to the entity describing the file `data.csv`. 

> ## Referencing external resources
> 
> RO-Crates can also contain data entities that are folders and Web
> resources, as well as non-File data like online databases
> -- see section on [data entities](https://www.researchobject.org/ro-crate/specification/1.2/data-entities.html).


We should now be able to follow the `@id` reference for the corresponding _data entity_ JSON block for our CSV file,
which we need to add to the `@graph` of the RO-Crate Metadata Document. 


## Add a data entity

1. Add a declaration for the CSV file as new entity with `@type` declared as `File`.  
2. Give the file a human-readable `name` and `description` to detail it as _Rainfall data for Katoomba in NSW Australia, clicenceaptured February 2022_. 
3. To add this is a CSV file,
   declare the `encodingFormat` as the appropriate [IANA media type](https://www.iana.org/assignments/media-types/#text) string. 

```json
{
   "@id": "data.csv",
   "@type": "File",
   "name": "Rainfall Katoomba 2022-02",
   "description": "Rainfall data for Katoomba, NSW Australia February 2022",
   "encodingFormat": "text/csv"
}
```

It is recommended that every entity has a human-readable `name`;
as shown in the above example, this does not need to match the filename/identifier.
The `encodingFormat` indicates the media file type so that consumers of the crate can open `data.csv` in an appropriate program,
and can be particularly important for less common file extensions frequently encountered in outputs from research software and instruments.

For more information on describing files and folders,
including their recommended and required attributes,
see section on [data entities](https://www.researchobject.org/ro-crate/specification/1.2/data-entities.html).


## Override the license

1. Consider if the file content of `data.csv` is not covered by our overall license (CC0), 
   but [Creative Commons BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/)
   (which only permits non-commercial use)
2. To override, add an  `license` cross-reference property on this particular data entity


```json
{
    "@id": "data.csv",
    "@type": "File",
    "name": "Rainfall Katoomba 2022-02",
    "description": "Rainfall data for Katoomba, NSW Australia February 2022",
    "encodingFormat": "text/csv",
    "license": { "@id": "https://creativecommons.org/licenses/by-nc-sa/4.0/" }
}
```
## Key Points
- Data entities are files & folders within the root, as well as external Web references
- Required properties for files are name and encodingFormat
- License can be overridden for particular data entities


# 8. Contextual entities

> ### Questions
> - How can I describe things in the world?
> - How can I give details about licenses?
> ### Objectives
> - Understand the difference between contextual and data entities
> - Add a contextual entity to the RO-Crate



## Contextual entities

Entities that we have added under `hasPart` are considered _data entities_,
while entities only referenced from those are considered _contextual entities_
-- they help explain the crate and its content.

You may notice the subtle difference between a _data entity_ that is conceptually part of the RO-Crate and is file-like (containing bytes),
while a _contextual entity_ is a representation of a real-life organization that can't be downloaded:
following the URL, we would only get its _description_.
The section [contextual entities](https://www.researchobject.org/ro-crate/specification/1.2/contextual-entities.html)
explores several of the entities that can be added to the RO-Crate to provide it with a **context**,
for instance how to link to authors and their affiliations.
Simplifying slightly, a data entity is referenced from `hasPart` in a `Dataset`,
while a contextual entity is referenced using any other defined property.

## Detailing licenses

We have previously declared two different `license` cross-references.
While following the URLs in this case explain the licenses well,
it is also best practice to include a very brief summary of contextual entities in the RO-Crate Metadata Document.
This is more important if the cross-reference do not use a permalink and may change over time.
As a minimum, each referenced entity should have a `@type` and `name` property.
It is also possible to add `url` for more information.

## Add license entities

Add a contextual entity for each of the two licenses,
see the [licensing](https://www.researchobject.org/ro-crate/specification/1.2/contextual-entities.html#licensing-access-control-and-copyright) section for details:

```json
{
   "@id": "https://creativecommons.org/licenses/by-nc-sa/4.0/",
   "@type": "CreativeWork",
   "name": "CC BY-NC-SA 4.0 International",
   "description": "Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International"
}  
{
   "@id": "http://spdx.org/licenses/CC0-1.0",
   "@type": "CreativeWork",
   "name": "CC0-1.0",
   "description": "Creative Commons Zero v1.0 Universal",
   "url": "https://creativecommons.org/publicdomain/zero/1.0/"
}
```


An additional exercise is to try to unify the two entities so that both use spdx identifiers,
remembering to update the corresponding `license` cross-references when changing the `@id`.
However, not all licenses have a direct SPDX identifier.
## Key Points
- Contextual entities are not considered part of the crate
- Cross-references should be expanded as contextual entities
- It is recommended to provide a human-readable name for licenses

# 9. Authorship in crates

> ### Questions
> - How can I list who made the content of the crate?
> - How do I affiliate a person with their place of work?
> ### Objectives
> - Adding and describing a Person contextual entity
> - Adding a Organization contextual entity
> - Understanding what it means to be an author of of the crate
> - Indicating the publisher of the RO-Crate

## Authorship

Moving back to the RO-Crate root `./`, let's specify who are the authors of the crate.

## Add an author and affiliation

1. Add yourself as an [`author`](https://www.researchobject.org/ro-crate/specification/1.2/contextual-entities.html#people)
   of the crate using the type `Person`
2. Include your preferred name. 
3. If you don't have an [ORCID](https://orcid.org/), you may use either the URL of your main home page at your institution,
   or a crate-local identifier like `#alice`.
4. Include your `affiliation` as a string value.

```json
{
  "@id": "./",
  "@type": ["Dataset", "LearningResource"],
  "author": {"@id": "https://orcid.org/0000-0002-1825-0097"},
  "…": "…"
},
{
  "@id": "https://orcid.org/0000-0002-1825-0097",
  "@type": "Person", 
  "name": "Josiah Carberry",
  "affiliation": "Brown University"
}
```

## Who can be authors of an RO-Crate?

When we say someone is an author of a crate,
it means they have contributed something substansively to its content (typically the data).
Agreement on what is considered authorship on a dataset can be tricky;
you may decide some people would be better represented as `contributor`.
One advantage of RO-Crate is that authorship can be declared explicitly also on each data entity,
so it can be clearer where each person have contributed (e.g. a statistician is author of an R script).
This means that generally the authors of the crate can be a broader,
more inclusive list than perhaps traditionally recognized as academic authorship.

## Add an organization

1. "Unroll" your `affiliation` of the person as cross-reference to another contextual entity,
   typed as an `Organization`. 
2. You can use [ROR](https://ror.org/) to find an identifier for most educational/research institutions,
   or you can use the main web page of your organization as its `@id`.

```json
{
  "@id": "https://orcid.org/0000-0002-1825-0097",
  "@type": "Person", 
  "name": "Josiah Carberry",
  "affiliation": {
    "@id": "https://ror.org/05gq02987"
  }
},
{
  "@id": "https://ror.org/05gq02987",
  "@type": "Organization",
  "name": "Brown University",
  "url": "http://www.brown.edu/"
}
```

The reuse of existing identifiers is important for both persons and organization from a FAIR perspective,
as their names may not be globally unique.


## Specify a publisher

1. Now imagine you are going to publish the RO-Crate on your institution's web pages. 
2. Cross-reference the same Organization entity with `publisher` from the RO-Crate Root entitity:


```json
{
   "@id": "./",
   "@type": ["Dataset", "LearningResource"],
   "publisher": {"@id": "https://ror.org/05gq02987"},
   "…": "…"
}
```
## Key Points
- Authors are described as separate entities
- Organization entities can be shared by multiple persons having the same affiliation
- Crate authors made (some) of the crate's content
- Publishers of an RO-Crate are typically organizations

---
# 10.Validating JSON-LD
> ### Questions
> - How can I validate the JSON-LD?
> ### Objectives
> - Learn to avoid common JSON and JSON-LD errors
> - Try the JSON-LD Playground with RO-Crate metadata
> - Visualize the RO-Crate entities

## Validating JSON-LD

As we made this RO-Crate Metadata File by hand,
it's good to check for any JSON errors, such as missing/extra `,` or unclosed `"` quotes.
Try pasting the file content into the [JSON-LD Playground](https://json-ld.org/playground/).
It should show up any errors, for example:

```error
JSON markup - SyntaxError: JSON.parse: expected `','` or `']'` after array element 
at line 29 column 5 of the JSON data
```

Modify the JSON file in your editor to fix any such errors.
You can also use editor commands such as _Format Document_ to ensure you have consistent spacing,
indentation and brackets.

If the document passes without errors in the JSON-LD Playground,
you should see output under _Expanded_ looking something like:

```json
[
  {
    "@id": "ro-crate-metadata.json",
    "@type": [
      "http://schema.org/CreativeWork"
    ],
    "http://schema.org/about": [
      {
        "@id": "./"
      }
    ],
    "http://purl.org/dc/terms/conformsTo": [
      {
        "@id": "https://w3id.org/ro/crate/1.2"
      }
    ]
  },
  {
    "@id": "./",
    "@type": [
      "http://schema.org/Dataset"
    ],
    "…": "…"
  }
]
```

This verbose listing of the JSON-LD shows how the `@context` has correctly expanded the keys,
but is not particularly readable.
Try the _Visualized_ tab to see an interactive rendering of the entities:

![Visualized in the JSON-LD Playground](https://www.researchobject.org/packaging_data_with_ro-crate/fig/jsonld-playground-visualized.png)


> ## Expanding the visualization
> 
> Click on the circles to expand or collapse the details of the graph's
> different nodes.


As the RO-Crate Metadata Document is valid JSON-LD it is also possible to process it
using Linked Data technologies such as triple stores and SPARQL queries.
It is beyond the scope of this tutorial to explain this aspect fully,
but interested readers should consider how to [handle relative URI references](https://www.researchobject.org/ro-crate/1.2/appendix/relative-uris.html).
As an example, try the _Table_ button and notice that the entities with relative identifiers are not included.
This is because when converting to RDF you need absolute URIs which do not readily exist when a crate is stored on disk,
we've not decided where the crate is to be published yet.  

## Key Points
- RO-Crate metadata files are valid JSON-LD
- The JSON-LD Playground can do basic validation and visualization
- Further use of RO-Crate as Linked Data is possible, but may require handling of relative URI references
---
# 11.Converting JSON-LD to triples

> ### Questions
> - How can I generate RDF triples from an RO-Crate?
> ### Objectives
> - Understand converting the JSON-LD to RDF triples
> - Learn how to create unique identifiers for items within an RO-Crate


## Advanced: Converting JSON-LD to triples

To convert the RO-Crate JSON-LD to triples,
e.g. to demonstrate how it might be described at a web resource,
a 'base' URI is needed that will point to that resource,
i.e. resolve as a page or file on the web.

Try to specify a hypothetical base URI by modifing the graph's `@context` within the [JSON-LD Playground](https://json-ld.org/playground/)
(do not modify the `ro-crate-metadata.json` on disk), and revisit the _Table_ rendering.

```json
{
  "@context": [
    "https://w3id.org/ro/crate/1.2/context",
    { "@base": "arcp://uuid,deffa754-c764-4e04-aabf-e600c6200553/" }
  ],
  "…": "…"
}
```

![Triples table in the JSON-LD Playground](https://www.researchobject.org/packaging_data_with_ro-crate/fig/jsonld-playground-table.png)

Above `arcp://uuid,deffa754-c764-4e04-aabf-e600c6200553/` is a randomly generated identifier to represent the RO-Crate root,
and now the JSON-LD Playground can show all the triples from the metadata file.
You can likewise use the _N-Quads_ button to convert the metadata file to the [RDF N-Quads](http://www.w3.org/TR/n-quads/) format.
Most RDF libraries and stores have JSON-LD support, but may need to specify a base URI as we did above,
making a new UUID for each imported RO-Crate.
## Key Points
- The JSON-LD @context maps JSON keys to schema.org vocabulary
- A @base URI is needed to make absolute URIs
- arcp and UUID can be used for RO-Crates that are not exposed on the Web

# 12. Visualizing a crate as HTML preview
## Questions
- How can an RO-Crate be rendered without showing the JSON?
- How can I generate a preview of the RO-Crate?
## Objectives
- Explore a HTML preview of an RO-Crate

## HTML preview

An RO-Crate can be distributed on disk, in a packaged format such as a zip file or disk image,
or placed on a static website. In any of these cases, an RO-Crate can have an accompanying HTML version (`ro-crate-metadata.html`) designed to be human-readable. 

![Example dataset for RO-Crate specification](https://www.researchobject.org/packaging_data_with_ro-crate/fig/ro-crate-preview-example.png)


## Navigate the RO-Crate preview

Try navigating the [preview of the running example](https://www.researchobject.org/packaging_data_with_ro-crate/files/rainfall-1.2.1/ro-crate-preview.html) and find:

1. What is the license of the rainfall CSV?
2. What is the affiliation of the crate's author?
3. What does the Validity Check inspect
4. What is not covered by this check?

1. CC BY-NC-SA 4.0 International
2. Brown University
3. The context, and for root dataset: existance, valid identifier, name, description, license and date published.  
4. The other entities were not checked, e.g. the `affiliation` of the author.


## Advanced: Generating HTML preview

The [LDACA RO-Crate Playground](https://ro-crate.ldaca.edu.au/) is a web-based tool that lets you upload an RO-Crate and view a structured HTML summary of its metadata.

Navigate to the [LDACA RO-Crate Playground](https://ro-crate.ldaca.edu.au/) Click "Start Packaging", then click "Examples." From the examples list, click "Try it" for "sample-crate."
You will see a tab called "HTML Preview" that contains a preview of the RO-Crate metadata.

If you want to generate a preview of your own RO-Crate, you can click "Create New" to create a new RO-Crate, where you can upload your data files and add metadata. Once you have added your data files and metadata, click the "HTML Preview" button at the top of the page.

The tool will generate a preview of your RO-Crate, which you can then download as an HTML file. This preview will show the entities in your RO-Crate, including the root dataset, data files, licenses, authors, and any other contextual entities you have added. You can save the resulting HTML page using your browser’s “Save As” feature, or extract the generated preview for distribution.



## Key Points
- RO-Crate can be rendered into a HTML preview
- RO-Crate previews tend to show each entity separately
- The preview HTML can be added as part of the RO-Crate

# 13. Completed RO-Crate

> ### Questions
> - What should the final RO-Crate look like?
> ### Objectives
> - Compare your RO-Crate with the solution
> - Understand the flattened nature of the JSON-LD

## Complete RO-Crate Metadata Document


The final RO-Crate Metadata Document constructed in this tutorial should look something like:


## `ro-crate-metadata.json`

```json
{
 "@context": "https://w3id.org/ro/crate/1.2/context",
 "@graph": [
   {
     "@id": "ro-crate-metadata.json",
     "@type": "CreativeWork",
     "conformsTo": {"@id": "https://w3id.org/ro/crate/1.2"},
     "about": {"@id": "./"}
   },
   {
     "@id": "./",
     "@type": ["Dataset", "LearningResource"],
     "hasPart": [
       {"@id": "data.csv"}
     ],
     "name": "Example dataset for RO-Crate specification",
     "description": "Official rainfall readings for Katoomba, NSW 2022, Australia",
     "datePublished": "2023-05-22T12:03:00+0100",
     "license": {"@id": "http://spdx.org/licenses/CC0-1.0"},
     "author": { "@id": "https://orcid.org/0000-0002-1825-0097" },
     "publisher": {"@id": "https://ror.org/05gq02987"}
   },
   {
     "@id": "data.csv",
     "@type": "File",
     "name": "Rainfall Katoomba 2022-02",
     "description": "Rainfall data for Katoomba, NSW Australia February 2022",
     "encodingFormat": "text/csv",
     "license": {"@id": "https://creativecommons.org/licenses/by-nc-sa/4.0/"}
   },
   {
     "@id": "https://creativecommons.org/licenses/by-nc-sa/4.0/",
     "@type": "CreativeWork",
     "name": "CC BY-NC-SA 4.0 International",
     "description": "Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International"
   },    
   {
     "@id": "http://spdx.org/licenses/CC0-1.0",
     "@type": "CreativeWork",
     "name": "CC0-1.0",
     "description": "Creative Commons Zero v1.0 Universal",
     "url": "https://creativecommons.org/publicdomain/zero/1.0/"
   },    
   {
     "@id": "https://orcid.org/0000-0002-1825-0097",
     "@type": "Person", 
     "name": "Josiah Carberry",
     "affiliation": {
       "@id": "https://ror.org/05gq02987"
     }
   },
   {
     "@id": "https://ror.org/05gq02987",
     "@type": "Organization",
     "name": "Brown University",
     "url": "http://www.brown.edu/"
   }
 ]
}
```
## Key Points
- A single RO-Crate lists all the entities
- The order of entities in the @graph array is not important


# 14. Next steps

> ## Questions
> - What else can I do with RO-Crate?
> ## Objectives
> - Find additional sections in RO-Crate specification


## Next steps


You have completed making a basic RO-Crate. You may try any of the following:

- **Add Additional Properties**: Explore the RO-Crate specification for a wide range of properties to describe  [data entities](https://www.researchobject.org/ro-crate/specification/1.2/data-entities.html) more comprehensively.

- **More Contextual Entities**: Discover additional [contextual entities](https://www.researchobject.org/ro-crate/specification/1.2/contextual-entities.html)  you can add to your crate, such as projects, grants, or related publications, to enrich the crate's context.

- **Provenance and Software**: Learn how to describe the [provenance](https://www.researchobject.org/ro-crate/specification/1.2/provenance.html)(origin and history) of your data and include information about the [software](https://www.researchobject.org/ro-crate/specification/1.2/workflows.html) and workflows used in your research.

- **RO-Crate Profiles**: Delve into [RO-Crate Profiles](https://www.researchobject.org/ro-crate/profiles.html), which are community-defined content checklists that extend the base specification to meet the specific needs of a domain or data type. Profiles impose conventions and may add domain specific terms/vocabularies, effectively making RO-Crates typed and machine-actionable for specific use cases. Examples include the [Workflow RO-Crate profile](https://about.workflowhub.eu/Workflow-RO-Crate/) used by WorkflowHub and the [Five Safes RO-Crate profile](https://trefx.uk/5s-crate/) for trusted workflow execution in Trusted Research Environments (TREs).

- **FAIR Signposting**: Understand how [FAIR Signposting](https://catalogue.fair-impact.eu/resources/fair-signposting) can be used in combination with RO-Crate to guide machine agents through metadata space. It re-uses existing web standards (HTTP Link headers or HTML `<link>` tags tags) to make machine navigation explicit, helping automated agents locate an object's identifier, data records, and metadata records.

- **Bioschemas**: Explore [Bioschemas](https://bioschemas.org/), a community initiative built on schema.org that defines domain-specific profiles (e.g., Dataset, ComputationalWorkflow, ChemicalSubstance) to add structured metadata to web resources, improving findability and interoperability, particularly in the life sciences.

- **Community and Tools**: Join the open [RO-Crate community](https://www.researchobject.org/ro-crate/community) on GitHub, participate in drop-in sessions, and explore available tools like Crate-O, RO-Crate Playground, and libraries for various programming languages (Python, JavaScript). This active community and the developer-friendly tools are key to RO-Crate's adoption and sustainability.

- **Real-world Use Cases**: Gain inspiration from how RO-Crates are being [used in practice](https://www.researchobject.org/ro-crate/use_cases) across various domains, such as in ELIXIR resources for life sciences, WorkflowHub for computational workflows, for Electronic Lab Notebooks (ELN) provenance, and for packaging earth observation data. RO-Crate enables exchange between services and data repositories, fostering an ecosystem of research infrastructure.