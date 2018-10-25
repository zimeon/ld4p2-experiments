# Exploring BFE data import

These notes from <https://github.com/zimeon/bfe-data>.

## Minimal Work in Monograph profile

### 1. Create test Work

Went to <http://bibframe.org/bfe/index.html> created a new Monograph / Work. Entered work title "The Thing", date of work "2018-10-19". Click preview at bottom of page, copy JSON-LD from panel.

--> <https://zimeon.github.io/bfe-data/01_work_as_exported.jsonld>

Starting [BFE](http://bibframe.org/bibliomata/bfe/development.html) again, Load Work, enter URL `https://zimeon.github.io/bfe-data/01_work_as_exported.jsonld` then click Submit URL. Goes to editor page with title and date shown.

### 2. Tidy the data

Make some arbitrary bnode URIs instead of the ones output, and remove the duplicate type definition, change title to add "- Titied":

```
> diff 01_work_as_exported.jsonld 02_work_tidied.jsonld
13c13
<    "@id": "http://id.loc.gov/resources/works/e289146822784588077004627201841415121964",
---
>    "@id": "_:b0",
20c20
<     "@id": "_:bnode5c8go15KEHTz99pytb6gca"
---
>     "@id": "_:b1"
29,34c29,31
<    "@id": "_:bnode5c8go15KEHTz99pytb6gca",
<    "@type": [
<     "bf:Title",
<     "bf:Title"
<    ],
<    "bf:mainTitle": "The Thing"
---
>    "@id": "_:b1",
>    "@type": "bf:Title",
>    "bf:mainTitle": "The Thing - Tidied"
```

--> <https://zimeon.github.io/bfe-data/02_work_tidied.jsonld>

Same procedure, Load Work from <https://zimeon.github.io/bfe-data/02_work_tidied.jsonld> works fine, shows new title and date.

### 3. Add extra data on the Work

```
> diff 02_work_tidied.jsonld 03_work_extra_triple.json
14a15
>    "http://example.org/predicate": "An Object",
31c32
<    "bf:mainTitle": "The Thing - Tidied"
---
>    "bf:mainTitle": "The Thing - Extra Data"
```

--> <https://zimeon.github.io/bfe-data/03_work_extra_triple.jsonld>

Same procedure, Load Work from <https://zimeon.github.io/bfe-data/03_work_extra_triple.jsonld> works fine, shows new title and date. As expected, does not show new data on form because this isn't set up in a profile. But, does show the extra triple in the RDF when Preview is selected. So, the extra triple is retained.

### 4. Other JSON-LD forms... compacted

Go to [JSON-LD playground](https://json-ld.org/playground/), load previous example as input, select "Compacted" form, copy output and change title to end with "- Compacted".

--> <https://zimeon.github.io/bfe-data/04_work_compacted.jsonld>

Again, is loaded fine, show expected data. Preview has normalized form with extra triple.

### 5. Other JSON-LD forms... expanded

Go to [JSON-LD playground](https://json-ld.org/playground/), load example 3 again as input, select "Expanded" formr, copy output and change title to end with "- Expanded".

--> <https://zimeon.github.io/bfe-data/05_work_expanded.jsonld>

Again, is loaded fine, show expected data. Preview has normalized form with extra triple.

### Conclusions for minimal Work in Monograph profile

From this experiment it seems that BFE does not care about the form of JSON-LD it gets. BFE is also happy to accept and persist extra triples (I tested with just one but assume that carries over for many), but obviously only shows things that a given profile knows about. However, if you go to Preview the extra data is shown in Turtle, JSON-LD and RDF/XML displays.

During these tests I found the upload to be unreliable. Sometimes I’d try to enter a URL and load, and then nothing would happen (was using Chrome on Mac). Other times it worked fine, so I just retried when it appeared to fail.

## Minimal Instance with Monograph profile

### 11. Create test minimal Instance

Went to <http://bibframe.org/bfe/index.html> created a new Monograph / Instance. Entered instance title "An Instance". Click preview at bottom of page, copy JSON-LD from panel.

--> <https://zimeon.github.io/bfe-data/11_instance.jsonld>

2018-10-19 -- Attempting to load this in the "Load IBC" did not work. In both Chrome and Firefox the browser seems to hang for some time after clicking "Submit URL", and eventually becomes response agiain on the same load page. No data gets loaded. @kirkhess resolved an issue with expectations of `instanceOf` to fix this.

Now this loads fine and shows the Instance.

### 12. Tidy without changing structure

Tidying to remove some unused cruft and use generic bnode names as follows:

```
> diff 11_instance.jsonld 12_instance_tidied.jsonld
5,9c5
<   "xsd": "http://www.w3.org/2001/XMLSchema#",
<   "bf": "http://id.loc.gov/ontologies/bibframe/",
<   "bflc": "http://id.loc.gov/ontologies/bflc/",
<   "madsrdf": "http://www.loc.gov/mads/rdf/v1#",
<   "pmo": "http://performedmusicontology.org/ontology/"
---
>   "bf": "http://id.loc.gov/ontologies/bibframe/"
13c9
<    "@id": "http://id.loc.gov/resources/instances/e93211693577620536533546183858602884391",
---
>    "@id": "_:b0",
25c21
<     "@id": "_:bnode7ZhNN2V8jv1XjBGERKYYGb"
---
>     "@id": "_:b1"
44,49c40,42
<    "@id": "_:bnode7ZhNN2V8jv1XjBGERKYYGb",
<    "@type": [
<     "bf:Title",
<     "bf:Title"
<    ],
<    "bf:mainTitle": "An Instance"
---
>    "@id": "_:b1",
>    "@type": "bf:Title",
>    "bf:mainTitle": "An Instance - Tidied"
```

--> <https://zimeon.github.io/bfe-data/12_instance_tidied.jsonld>

Works fine and shows the same data as 11 (modulo title change).

### 13. Simplifying

--> <https://zimeon.github.io/bfe-data/13_instance_simplified.jsonld>

--> <https://zimeon.github.io/bfe-data/13_instance_simplified_expanded.jsonld>

### 14. Over Simplifying

--> <https://zimeon.github.io/bfe-data/14_instance_oversimplified.jsonld>


