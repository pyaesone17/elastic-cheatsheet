### Term query vs match query

The term query looks for the exact term in the field’s inverted index — 
it doesn’t know anything about the field’s analyzer. 
This makes it useful for looking up values in keyword fields, 
or in numeric or date fields. 
When querying full text fields, use the match query instead, which understands how the field has been analyzed.

search?"Quick brown"

Term query will search "Quick Brown" ( value matching )
Match query will search ["quick", "brown"] ( text searching )

Query string with wild card

```json
"query_string": {
    "fields": ["first_name", "last_name"],
    "query":  "mgmg*",
},
```

### Elastic Search DSL

Leaf query clauses
term, match, range

Compound query clauses
bool , dis_max 

### Terms Level Query

* term query
Find documents which contain the exact term specified in the field specified.

* terms query
Find documents which contain any of the exact terms specified in the field specified.

* terms_set query
Find documents which match with one or more of the specified terms. The number of terms that must match depend on the specified minimum should match field or script.

* range query
Find documents where the field specified contains values (dates, numbers, or strings) in the range specified.

* exists query
Find documents where the field specified contains any non-null value.

* prefix query
Find documents where the field specified contains terms which begin with the exact prefix specified.

* wildcard query
Find documents where the field specified contains terms which match the pattern specified, where the pattern supports single character wildcards (?) and multi-character wildcards (*)

* regexp query
Find documents where the field specified contains terms which match the regular expression specified.

* fuzzy query
Find documents where the field specified contains terms which are fuzzily similar to the specified term. Fuzziness is measured as a Levenshtein edit distance of 1 or 2.

* type query
Find documents of the specified type.

* ids query
Find documents with the specified type and IDs.

### Full text queries

* match query
The standard query for performing full text queries, including fuzzy matching and phrase or proximity queries.

* match_phrase query
Like the match query but used for matching exact phrases or word proximity matches.

* match_phrase_prefix query
The poor man’s search-as-you-type. Like the match_phrase query, but does a wildcard search on the final word.

* multi_match query
The multi-field version of the match query.

* common terms query
A more specialized query which gives more preference to uncommon words.

* query_string query
Supports the compact Lucene query string syntax, allowing you to specify AND|OR|NOT conditions and multi-field search within a single query string. For expert users only.

* simple_query_string query
A simpler, more robust version of the query_string syntax suitable for exposing directly to users.

### Multi fields on one property

PUT my_index
```json
{
  "mappings": {
    "_doc": {
      "properties": {
        "city": {
          "type": "text",
          "fields": {
            "raw": { 
              "type":  "keyword"
            }
          }
        }
      }
    }
  }
}
```

GET my_index/_search
```json
{
  "query": {
    "match": {
      "city": "york" 
    }
  },
  "sort": {
    "city.raw": "asc" 
  },
  "aggs": {
    "Cities": {
      "terms": {
        "field": "city.raw" 
      }
    }
  }
}
```

### Bool query and condition

Must, Should, Should Not

### Elasticsearch Standard Anaylizer 

POST _analyze
```json
{
  "analyzer": "standard",
  "text": "The 2 QUICK Brown-Foxes jumped over the lazy dog's bone."
}
```

[ the, 2, quick, brown, foxes, jumped, over, the, lazy, dog's, bone ]

```json

"query": {
  "bool": {
    "must": [
      {
        "term": {
          "is_gigster": true
        }
      },
      {
        "term": {
          "skills.category.id": "photography"
        }
      }
    ]
  }
}
```
```json
"filter": [
  {
    "term": {
      "is_gigster": true
    }
  },
  {
    "term": {
      "skills.category.id": "photographys"
    }
  }
],
```
