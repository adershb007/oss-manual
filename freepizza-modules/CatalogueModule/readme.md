# Catalogue Module for React

| #    | Version | Change Log    |
| ---- | ------- | ------------- |
| 1    | 1.0     | Initial Draft |

This document covers software specifications and design contraints that needs to be followed while developing the module

------

### Abstract

A catalogue module is proposed to be developed for showcasing an organisations productions on their web or mobile apps. This module should provide, homepage, search page and a product / catalogue details page for marketing purposes.

The catalogue could be *products* an ecommerce organisation offers, or it could be *movies / tv shows* for companies like *Netflix*, For FreePizza, each catalogue represents individual module or use-case.

#### Customisability Evaluation

> N/A

#### Design Constraints

- The design should follow our general UI / UX guidelines for developing a FreePizza module
- Additionally, the user must feel engaged and entertained when using this module

------

### Architecture

FreePizza modules are developed to be used with Ark Framework

#### Views

1. Home.View
   - Functionalities: 
     - Load content on page load (if not loaded yet)
     - Language / Currency Change Listeners
2. CatalogueDetails.View
   - Functionality:
     - Load content on page load (if not loaded)
     - Handle 404 appropriately
     - Language / Currency Change Listeners
   - Representational Design Only
3. SearchResults.View
   - Dummy Filter Options (Extended)
   - Tag based Filter (Multiple)
   - Collection Based Filter (Multiple)
   - Infinite Scrolling
   - Language / Currency Change Listeners

#### User Flow

> N/A

#### @types

````typescript
type Collection = {
  _id: string
  _handler: string
  title: string
  description?: string
  count: number
  items: Array<CatalogueItem>
}

type CatalogueItem = {
  _id: string
  _handler: string
  title: string
  description: string
  tags: Array<string>
  collections: Array<Collection>
}

type FilterOptions = {
  keyword: string
  tag: string[]
  maxItemsPerFetch: number // Defaults to 20
  hasMoreResults: boolean
}

type State = {
  isLoading: boolean
  hasInitialized: boolean
  filter: FilterOption
  
  // Home page
  featuredItems: Array<CatalogueItem>
  
  // Filter Page
  results: Array<CatalogueItem>
  
  // Details Page
  context: CatalogueItem
}
````



#### Controller Specification

````typescript
type Controller = {
  populateHome: (force?: boolean) => void // Home.View
  populateCatalogueItemByHandler: (
  	handler: string, 
  	force?: boolean
  ) => void // CatalogueDetails.View
  populateResults: (filterOpt: FilterOption, force?: boolean) => void
}
````



#### Service Specification (Declaration Map)

```typescript
type Service = {
  fetchHome: () => {
    featuredItems: Array<CatalogueItem>
  }
  fetchCatalogueItemByHandler: (handler: string) => CatalogueItem
  fetchResult: (filterOpt: FilterOption) => void
}
```



#### Analytics Requirement

> N/A