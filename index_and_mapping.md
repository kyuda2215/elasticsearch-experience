# Creating Index

To use Elasticsearch, you need HTTP Client to run commands for Elasticsearch (Postman, Insomnia, cURL). These commands include creating indexes.

How to Create Index:
```
PUT <index_name>
```
> Make sure index name has never been taken before!

In request body, you can put index's settings and mapping for the index. You can also put several more items, look at the references below.

## Setting
Example for index creation with setting
```
PUT example
{
    "settings":{
        "number_of_shards": 3,
        "number_of_replicas": 2
    }
}
```  
## Mapping
Mapping defines the data type in each fields in documents stored in each index. 

Example for index creation with mapping
```
PUT song
{
    "mappings":{
        "properties":{
            "title":{
                "type":"text"
            },
            "artist":{
                "type":"text"
            },
            "year":{
                "type":"integer"
            }
        }
    }
}
```  
The types needed for mapping could be seen here https://www.elastic.co/guide/en/elasticsearch/reference/current/mapping-types.html 

You can also add fields after creating an index just by adding data into the index. Dyamic Mapping will handle the new field and automatically set the data type for the field. However, if you need to specify the data types from the start it's recommended to set up the mapping first when creating index.

You can get mapping for an index by calling `GET <index>/_mapping`
```
GET song/_mapping
```
You can update mappings by calling `PUT <index>/_mapping`
```
PUT song/_mapping
{
	"properties": {
		"artist": {
			"type": "text"
		},
		"title": {
			"type": "text"
		},
		"year": {
			"type": "integer"
		},
		"play_count":{
			"type": "long"
		}
	}
}
```
  
#### Some facts I learned about the data types in Elasticsearch...
- `Text` data type is used when you need **full-text** search for a field. Use `Text` for bodies of e-mails, product description, or strings that you can search up to word by word. `Keyword` data type is used when you need **exact-value** search, especially if you need filtering, sorting, and aggregations based on the value. Use `Keyword` for fields like e-mail address, zip codes, tags, etc.  

References for Index and Mapping:
- https://www.elastic.co/guide/en/elasticsearch/reference/current/indices-create-index.html
- https://www.elastic.co/guide/en/elasticsearch/reference/current/indices-put-mapping.html
- https://www.tutorialspoint.com/elasticsearch/elasticsearch_index_apis.htm
- https://logz.io/blog/elasticsearch-mapping/