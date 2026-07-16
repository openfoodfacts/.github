# How to Generate Unique URLs for our Taxonomy Entries

## Context and Problem Statement

Our current taxonomies generate a canonical identifier from the first label of the entry. For example `en: Plant Milks, alternative milks, non-dairy milk drinks` would have a canonical id of `en:plant-milks`.

Should we use this identifier in our ontologies?

## Decision Drivers

* it should be easy to match the ontology entry with our existing taxonomy item
* identifiers must ne unique
* identifiers should ideally be immutable

## Considered Options

* use the existing canonical identifier
* use the existing identifier, replacing the colon with a dash
* use the existing identifier without the country code prefix
* generate a new, immutable, identifier

## Decision Outcome

Chosen option: "use the existing identifier without the country code prefix" subject to checking for duplicates in our existing taxonomies.




## Pros and Cons of the Options

### Use the existing canonical identifier

In this case the full URI for an item might be something like: `https://openfoodfacts.org/data/taxonomies/ingredients#en:milk-powder`

* Good: Works with our existing APIs and facets pages
* Good: Aligns with our existing canonical identifiers
* Neutral: Users may feel they need to escape the colon, which is not necessary
* Bad: Currently our identifiers can change if the preferred label for the first language is updated

### Use the existing identifier, replacing the colon with a dash

In this case the full URI for an item might be something like: `https://openfoodfacts.org/data/taxonomies/ingredients#en-milk-powder`

* Good: No confusion about escaping the colon
* Good: Intuitively similar to our existing identifier
* Bad: Doesn't work with our existing APIs or facets pages
* Bad: Can still change

### Use the existing identifier without the country code prefix

In this case the full URI for an item might be something like: `https://openfoodfacts.org/data/taxonomies/ingredients#milk-powder`

* Good: No confusion about escaping the colon
* Good: Intuitively similar to our existing identifier
* Good: Should work with our APIs and facets pages
* Good: Identifier will not need to change if we later introduce a new, immutable, `xx:` identifier
* Bad: Could introduce duplicates

### Generate a new, immutable, identifier

In this case the full URI for an item might be something like: `https://openfoodfacts.org/data/taxonomies/ingredients#0a7f1bc8-32b4-43cb-b56e-69f9e30a5521`

* Good: No confusion about escaping the colon
* Good: Globally unique and immutable
* Bad: Won't work with our APIs and facets pages
* Bad: Does not correlate with our existing taxonomies
* Bad: Difficult for humans to generate new identifiers

