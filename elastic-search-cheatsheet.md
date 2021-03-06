
  

## Check the available indices

    Syntax : curl -XGET 'localhost:9200/_cat/indices?v&pretty'

  

## Check the available nodes

    Syntax : curl -XGET 'localhost:9200/_cat/nodes?v&pretty'

  

## Health Check

    Syntax : curl -XGET 'localhost:9200/_cat/health?v&pretty'

  

## Create new index

    curl -XPUT 'localhost:9200/customers?&pretty'

  

## Add new document to an index

    Syntax : curl -XPUT 'localhost:9200/{indexName}/{type}/{id}?&pretty' -H "Content-Type: application/json" -d '{jsonObject that needs to be stored as an object}'

    Example : curl -XPUT 'localhost:9200/products/mobiles/1?&pretty' -H "Content-Type: application/json" -d '{
       "name":"iphone7",
       "camera":"12MP",
       "storage":"257GB",
       "display":"4.7inch",
       "battery":"1,960 mAh",
       "reviews":[
          "Incredibly happy after using it for one week",
          "Best iphone so far",
          "Very expensive, stick to Android"
       ]
    }'

  

## Get whole document by id

    Syntax : curl -XGET 'localhost:9200/{indexName}/{type}/{id}?&pretty'
    
    Example : curl -XGET 'localhost:9200/products/mobiles/3?&pretty'

  

## Updating the whole document

    Syntax : curl -XPUT 'localhost:9200/{indexName}/{type}/{id}?&pretty' -H "Content-Type: application/json" -d '{jsonObject that needs to be updated as an object}'
    
    Example : curl -XPUT 'localhost:9200/products/mobiles/1?&pretty' -H "Content-Type: application/json" -d '{
       "name":"iphone7",
       "camera":"12MP",
       "storage":"257GB",
       "display":"4.7inch",
       "battery":"1,960 mAh",
       "reviews":[
          "Incredibly happy after using it for one week",
          "Best iphone so far",
          "Very expensive, stick to Android"
       ]
    }'

  

## Partial document update for specific fields

    Syntax : curl -XPOST 'localhost:9200/{indexName}/{type}/{id}/_update?pretty' -d
    
    '{
    
    "doc" : {
    
    "doc":  {{key of the property that needs to be updated} : {value of the property that needs to be updated}}
    
    }'
    
    Example : curl -XPOST 'localhost:9200/products/mobiles/2/_update?pretty' -d
    
    '{
       "doc":{
          "color":"black"
       }
    }'

  

## Update a field using script in the document

    Example : curl -XPOST 'localhost:9200/products/shoes/1/_update?pretty' -d
    
    '{
    
    "script" : "ctx._source.size += 2" ## This will eventually increment the size property value by 2
    
    }'

  

## Add bulk documents using a single operation

    Example : curl -XPOST 'localhost:9200/_bulk?pretty' -d
    
    '{"index" : {"_index" : "products", "_type" : "shoes", "_id" : 3}}
    
    {"name" : "puma", "size": 9, "color": "black"}
    
    {"index" : {"_index" : "products", "_type" : "shoes", "_id" : 4}}
    
    {"name" : "New Balance", "size": 8, "color": "black"}'

  

OR

  

    curl -XPOST 'localhost:9200/products/shoes/_bulk?pretty' -H 'Content-Type: application/json' -d
    
    '{"index" : {"_id" : 3}}
    
    {"name" : "puma", "size": 9, "color": "black"}
    
    {"index" : {"_id" : 4}}
    
    {"name" : "New Balance", "size": 8, "color": "black"}'

  

OR

    curl -XPOST 'localhost:9200/products/shoes/_bulk?pretty' -H 'Content-Type: application/json' -d
    
    '{"index" : {}} ## id will be autogenerated by elastic search
    
    {"name" : "puma", "size": 9, "color": "black"}
    
    {"index" : {}}
    
    {"name" : "New Balance", "size": 8, "color": "black"}'

  

## Multiple operations in a single HTTP Call

    Example : curl -XPOST 'localhost:9200/_bulk?pretty' -H 'Content-Type: application/json' -d
    
    '{"index" : {"_index" : "products", "_type" : "shoes", "_id" : 3}}
    
    {"name" : "puma", "size": 9, "color": "black"}
    
    {"index" : {"_index" : "products", "_type" : "shoes", "_id" : 4}}
    
    {"name" : "New Balance", "size": 8, "color": "black"}
    
    {"delete" : {"_id" : "2"}}
    
    {"create" : {"_id" : "5"}}
    
    {"name" : "Nike Power", "size": 12, "color": "black"}
    
    {"update" : {"_id" : "1"}}
    
    {"doc" : {"color" : "orange"}}'

  
  

## Delete a document in the index

    Syntax : curl -XDELETE 'localhost:9200/{indexName}/{type}/{id}?&pretty'
    
    Example : curl -XDELETE 'localhost:9200/products/mobiles/3?&pretty'

  

## Delete the whole index

    Syntax : curl -XDELETE 'localhost:9200/{indexName}?&pretty'
    
    Example : curl -XDELETE 'localhost:9200/products?&pretty'

  

## Get bulk documents using _mget Api in elastic search

    Example : curl -XGET 'localhost:9200/_mget?pretty' -d '
    {
       "docs":[
          {
             "_index":"products",
             "_type":"laptops",
             "_id":"1"
          },
          {
             "_index":"products",
             "_type":"laptops",
             "_id":"2"
          }
       ]
    }'

  

OR

    Example : curl -XGET 'localhost:9200/products/laptop/_mget?pretty' -d '
    {
       "docs":[
          {
             "_id":"1"
          },
          {
             "_id":"2"
          }
       ]
    }'

  

## Get specific fields of the document by id

    Syntax : curl -XGET 'localhost:9200/{indexName}/{type}/{id}?&pretty&_source={fieldNames}'
    
    Example : curl -XGET 'localhost:9200/products/mobiles/3?&pretty&_source=name,reviews'

  

## Bulk add documents

    Syntax : curl -H "Content-Type: application/x-ndjson" -XPOST 'localhost:9200/{indexName}/{type}/_bulk?pretty&refresh' --data-binary @"{fileName.json}"
    
    Example : curl -H "Content-Type: application/x-ndjson" -XPOST 'localhost:9200/customers/personal/_bulk?pretty&refresh' --data-binary @"customers_full.json"

  

## Sort by age desc while reverting back query result

    Syntax : curl -XGET http:##localhost:9200/{indexName}/_search?q={searchTerm}&pretty
    
    Example : curl -XGET http:##localhost:9200/customers/_search?q=wyoming&pretty

  

## Sort by attribute in specific order while reverting back query result

    Syntax : curl XGET http:##localhost:9200/customers/{indexName}/_search?q={searchTerm}&pretty&sort={attributeName}:{order}&pretty
    
    Example : curl XGET http:##localhost:9200/customers/_search?q=wyoming&pretty&sort=age:desc&pretty

  

## Search by request body json request

    curl -XGET 'localhost:9200/customers/_search?pretty' -H 'Content-Type: application/json' -d '
    {
       "query":{
          "match_all":{
             
          }
       },
       "sort":{
          "age":{
             "order":"desc"
          }
       },
       "size":20
    }'

  



**Search for a term using request body on a specific attribute**

    curl -XGET 'localhost:9200/customers/_search?pretty' -H 'Content-Type: application/json' -d '
    {
      "query": {
        "term": {
          "name": "gates"
        }
      }
    }'

  

## when you want to include certain fields in response and exclude certain fields

    curl -XGET 'localhost:9200/customers/_search?pretty' -H 'Content-Type: application/json' -d '
    {
      "_source": {
        "includes": [
          "st*",
          "*n*"
        ],
        "excludes": [
          "*der"
        ]
      },
      "query": {
        "term": {
          "name": "gates"
        }
      }
    }'

  

## when we need to find a match specific searches

    curl -XGET 'localhost:9200/customers/_search?pretty' -H 'Content-Type: application/json' -d '
    {
      "query": {
        "match": {
          "name": "webster"
        }
      }
    }'

  

## Search with logical operator(even if we don't specify operator field, it will make a or by default)

    curl -XGET 'localhost:9200/customers/_search?pretty' -H 'Content-Type: application/json' -d '
    {
      "query": {
        "match": {
          "name": {
            "query": "frank norris",
            "operator": "or"
          }
        }
      }
    }'

  

## Search for exact field matches

    curl -XGET 'localhost:9200/customers/_search?pretty' -H 'Content-Type: application/json' -d '
    {
      "query": {
        "match_phrase": {
          "street": "tompkins place"
        }
      }
    }'

  

## Search with only prefix

    curl -XGET 'localhost:9200/customers/_search?pretty' -H 'Content-Type: application/json' -d '
    {
      "query": {
        "match_phrase_prefix": {
          "name": "ma"
        }
      }
    }'

  

## Boolean compound query

  

**Must example**

    curl -XGET 'localhost:9200/customers/_search?pretty' -H 'Content-Type: application/json' -d '
    {
      "query": {
        "bool": {
          "must": [
            {
              "match": {
                "street": "ditmas"
              }
            },
            {
              "match": {
                "street": "avenue"
              }
            }
          ]
        }
      }
    }'

  

**Should example**

    curl -XGET 'localhost:9200/customers/_search?pretty' -H 'Content-Type: application/json' -d '
    {
      "query": {
        "bool": {
          "should": [
            {
              "match": {
                "street": "ditmas"
              }
            },
            {
              "match": {
                "street": "avenue"
              }
            }
          ]
        }
      }
    }'

  

**Must not keyword**

    curl -XGET 'localhost:9200/customers/_search?pretty' -H 'Content-Type: application/json' -d '
    {
      "query": {
        "bool": {
          "must_not": [
            {
              "match": {
                "street": "california texas"
              }
            },
            {
              "match": {
                "street": "lane street"
              }
            }
          ]
        }
      }
    }

'

  

## Filter context queries

    curl -XGET 'localhost:9200/customers/_search?pretty' -H 'Content-Type: application/json' -d '
    {
      "query": {
        "bool": {
          "must": {
            "match_all": {}
          },
          "filter": {
            "range": {
              "age": {
                "gte": 20,
                "lte": 30
              }
            }
          }
        }
      }
    }'

  

## Aggregating in elastic search

    curl -XGET 'localhost:9200/customers/_search?pretty' -H 'Content-Type: application/json' -d '
    {
      "size": 1,
      "aggs": {
        "avg_age": {
          "avg": {
            "field": "age"
          }
        }
      }
    }'

  

## search and aggregate

    curl -XGET 'localhost:9200/customers/_search?pretty' -H 'Content-Type: application/json' -d '
    {
      "size": 0,
      "query": {
        "bool": {
          "filter": {
            "match": {
              "state": "minnesota"
            }
          }
        }
      },
      "aggs": {
        "avg_age": {
          "avg": {
            "field": "age"
          }
        }
      }
    }'

  

## search and aggregate with all stats

    curl -XGET 'localhost:9200/customers/_search?pretty' -H 'Content-Type: application/json' -d '
    {
      "size": 0,
      "query": {
        "bool": {
          "filter": {
            "match": {
              "state": "minnesota"
            }
          }
        }
      },
      "aggs": {
        "age_stats": {
          "stats": {
            "field": "age"
          }
        }
      }
    }'

  

## Determining cardinality (finding unique number of values stored in a specific field)

    curl -XGET 'localhost:9200/customers/_search?pretty' -H 'Content-Type: application/json' -d '
    {
      "size": 0,
      "aggs": {
        "age_count": {
          "cardinality": {
            "field": "age"
          }
        }
      }
    }'

  

## Mapping api for fieldData calculation from hash

    curl -XPUT 'localhost:9200/customers/_mapping/personal?pretty' -H 'Content-Type: application/json' -d'
    {
      "properties": {
        "gender": {
          "type": "text",
          "include_type_name": true
        }
      }
    }'

  

## Bucketing age aggregation

    curl -XGET 'localhost:9200/customers/_search?pretty' -H 'Content-Type: application/json' -d '
    {
      "size": 0,
      "aggs": {
        "age_bucket": {
          "terms": {
            "field": "age"
          }
        }
      }
    }'

  

## Bucketing query with ranges

    curl -XGET 'localhost:9200/customers/_search?pretty' -H 'Content-Type: application/json' -d '
    {"size":0,"aggs":{"age_bucket":{"ranges":{"field":"age","ranges":[{"to":30},{"from":30,"to":40},{"from":40,"to":55},{"from":55}]}}}}'

