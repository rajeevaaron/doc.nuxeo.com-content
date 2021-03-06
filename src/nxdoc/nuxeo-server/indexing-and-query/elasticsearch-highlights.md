---
title: Elasticsearch Highlights
description: A description for how Elasticsearch can be used for highlights.
review:
    date: '2017-05-22'
    status: ok
    comment: ''
labels:
    - elasticsearch
tree_item_index: 650

---

When using the Elasticsearch Page Provider, you can use the [Elasticsearch highlight feature](https://www.elastic.co/guide/en/elasticsearch/reference/2.4/search-request-highlighting.html) to highlight some words in your query results.

### Elasticsearch Configuration

The fields that can be highlighted have to be fulltext analyzed by Elasticsearch, so the mapping has to be modified accordingly. For example, if you want to highlight words in the content of a file, contained in the `ecm:binaryText` field, the mapping should be modified as such:

```
"ecm:binarytext" : {
        "type" : "string",
        "include_in_all" : true,
        "analyzer" : "fulltext"
},

```

### Using Highlights

To trigger highlighting, the highlight [enricher]({{page page='content-enrichers'}}) needs to be used:

- For example through the search endpoint with a page provider, and the list of fields that should be highlighted:

```
http://localhost:8080/nuxeo/api/v1/search/pp/myPageProvider/execute?ecm_fulltext=content&highlight=dc:description,ecm:binarytext&enrichers.document=highlight
```

In the previous example, when calling the `myPageProvider` page provider, the `content` word will be highlighted in the fields `dc:description` or `ecm:binarytext`

- Or with the `Repository.PageProvider` operation with the list of fields that should be highlighted as a parameter. For example with the `nuxeo-operation` Nuxeo element:

```
<nuxeo-operation auto id="highlightOeration" op="Repository.PageProvider"
  params='{"providerName":"myPageProvider", "highlights":"ecm:binarytext", "namedParameters":{"ecm_fulltext":"[[searchTerm]]"}}'
  response={{response}} enrichers="highlight">
```

The results will be available in the `contextParameters` of the response:

```
"entity-type": "document",
...
"contextParameters": {
        "highlight": {
          "ecm:binarytext": [
            "here is my content please <em>highlight</em> me"
          ]
        }
      }
```
