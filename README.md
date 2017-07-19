# Task-ElasticSearch-Data-structure
<details>
<summary>TASK Description</summary>

Fill ElasticSearch with documents which represent different users.
*Perform search queries to select:

- all users
- male users only
- male users with name XXX
- male users which were born after date xx-xx-xx and with name XXX
- *Calculate total salary of all users using ElasticSearch aggregations.

Links:

Search API https://www.elastic.co/guide/en/elasticsearch/reference/current/search.html

QUERY DSL https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl.html

Aggregations https://www.elastic.co/guide/en/elasticsearch/reference/current/search-aggregations.html

</details>

#### Run ElasticSearch & kibana

I propose to follow the next order of actions
(To avoid errors)

<details>
<summary>Start ElasticSearch</summary>
  
```
 bin\elasticsearch
```
</details>

<details>
<summary>Start Kibana (optional)</summary>   
I recommend use kibana for show result queries.

Kibana start command:
  
```
  bin\kibana
```

Default kibana start work on next location: 
http://localhost:5601

</details>
 

### Test data

***Bulk query for kibana Dev Tools***

<details>
<summary>Test data for task </summary>

``` 

  POST _bulk
  { "create" : { "_index" : "store", "_type" : "user", "_id" : "1" } }
  {"name":"Sasha","gender":"female","birth_date":"1998.11.09","salary":3564 }
  { "create" : { "_index" : "store", "_type" : "user", "_id" : "2" } }
  {"name":"Sasha","gender":"female","birth_date":"2005.04.03","salary":2541 }
  { "create" : { "_index" : "store", "_type" : "user", "_id" : "3" } }
  {"name":"Sasha","gender":"female","birth_date":"1978.03.07","salary":3500 }
  { "create" : { "_index" : "store", "_type" : "user", "_id" : "4" } }
  {"name":"Sasha","gender":"male","birth_date":"2000.04.08","salary":7800 }
  { "create" : { "_index" : "store", "_type" : "user", "_id" : "5" } }
  {"name":"Lera","gender":"female","birth_date":"1990.03.01","salary":1500 }
  { "create" : { "_index" : "store", "_type" : "user", "_id" : "6" } }
  {"name":"Lera","gender":"female","birth_date":"1993.06.11","salary":1100 }
  { "create" : { "_index" : "store", "_type" : "user", "_id" : "7" } }
  {"name":"Vitaliy","gender":"male","birth_date":"1991.06.06","salary":8300 }
  { "create" : { "_index" : "store", "_type" : "user", "_id" : "8" } }
  {"name":"Igor","gender":"male","birth_date":"1963.01.01","salary":1254 }
  { "create" : { "_index" : "store", "_type" : "user", "_id" : "9" } }
  {"name":"Anton","gender":"male","birth_date":"1995.08.01","salary":17900 }
  { "create" : { "_index" : "store", "_type" : "user", "_id" : "10" } }
  {"name":"Ivan","gender":"male","birth_date":"2002.05.02","salary":9650 }
  { "create" : { "_index" : "store", "_type" : "user", "_id" : "11" } }
  {"name":"Olga","gender":"female","birth_date":"1976.06.02","salary":3540 }
  { "create" : { "_index" : "store", "_type" : "user", "_id" : "12" } }
  {"name":"Irina","gender":"female","birth_date":"1988.03.12","salary":2300 }
  { "create" : { "_index" : "store", "_type" : "user", "_id" : "13" } }
  {"name":"Irina","gender":"female","birth_date":"1997.11.11","salary":2300 }
  { "create" : { "_index" : "store", "_type" : "user", "_id" : "14" } }
  {"name":"Alex","gender":"male","birth_date":"1994.10.01","salary":3200 }
  { "create" : { "_index" : "store", "_type" : "user", "_id" : "15" } }
  {"name":"Maxim","gender":"male","birth_date":"1999.09.08","salary":7200 }
  { "create" : { "_index" : "store", "_type" : "user", "_id" : "16" } }
  {"name":"Elena","gender":"female","birth_date":"1989.09.08","salary":7200 }
  { "create" : { "_index" : "store", "_type" : "user", "_id" : "17" } }
  {"name":"Maxim","gender":"male","birth_date":"1999.09.08","salary":1254 }
  { "create" : { "_index" : "store", "_type" : "user", "_id" : "18" } }
  {"name":"Elena","gender":"female","birth_date":"2000.09.08","salary":7200 }
  { "create" : { "_index" : "store", "_type" : "user", "_id" : "19" } }
  {"name":"Sveta","gender":"female","birth_date":"1990.01.03","salary":6000 }
  { "create" : { "_index" : "store", "_type" : "user", "_id" : "20" } }
  {"name":"Slava","gender":"male","birth_date":"1999.08.07","salary":1300 }


```
</details>


## Solution
 
 ***All queries was develop and test in Kibana - Dev Tools***!
 
<details>
<summary>all users</summary> 

```  
    
   GET /store/user/_search
   
``` 
</details>
 
<details>
<summary>male users only</summary> 

```  

  GET /store/user/_search
  {
    "query": {
      "term": {
        "gender": "male"
      }
    }
  }
  
``` 
</details>

<details>
<summary>male users with name XXX</summary> 

```  
  
  GET /store/user/_search
  {
    "query": {
      "bool": {
        "must": [
          {
            "match": {
              "gender": "male"
            }
          },
          {
            "match": {
              "name": "Sasha"
            }
          }
        ]
      }
    }
  }
  
``` 
</details>

<details>
<summary>male users which were born after date xx-xx-xx and with name XXX</summary> 

```  
  
  GET /store/user/_search
  {
    "query": {
      "bool": {
        "must": [
          {
            "match": {
              "gender": "female"
            }
          },
          {
            "match": {
              "name": "Sasha"
            }
          },
          {
            "range": {
              "birth_date": {
                "gte": "1991.06.16"
              }
            }
          }
        ]
      }
    }
  }
  
``` 
</details>

<details>
<summary>*Calculate total salary of all users using ElasticSearch aggregations</summary> 

```  
  
  GET /store/_search?size=0
  {
    "aggs": {
      "total_salary": {
        "sum": {
          "field": "salary"
        }
      }
    }
  }
  
``` 
</details>



------

<details>
<summary>For delete all documents in type "user", I use next cURL </summary> 

```  
curl -XPOST localhost:9200/store/user/_delete_by_query?conflicts=proceed&pretty -d'{"query": {"match_all": {}}}'  
``` 
</details>

 