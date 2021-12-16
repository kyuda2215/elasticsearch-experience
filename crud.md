# Data CRUD
In SQL or any ACID based DBMS, we learn the term CRUD, stands for Create, Read, Update, Delete. CRUD can be done in Elasticsearch as well.

## Create (and Update)
The way to Create and Update an entry in Elasticsearch is, basically, same. You only need to hit `PUT <index>/_doc/<id>` to create/update data into the `index` with specific `id`. If there is no data in that `id`, it will create one. If there is a data in that `id`, it will update that data. However, the data **must follow the mapping that has been defined**. If there is no mapping for that field, it will generate mapping for that field automatically on default case. The data payload need to be structured in JSON Format. Set the body type into `application/json` as well.

Example to Update/Create data on index `song` for `id` 1
```
PUT song/_doc/1
{
	"artist": "Good Artist",
	"title": "Good Song",
	"year": 2000,
	"play_count": 10000
}
```

You can also put data without the id of the data. This will give you a random index though.
```
POST song/_doc/
{
	"artist": "Good Artist",
	"title": "Good Song",
	"year": 2000,
	"play_count": 10000
}
```
To update data in Elasticsearch, **you must do it with the whole payload**. It means you have to **put every field in the data and the field that you want to change the value**. For example, if we want to update `artist` in the data above, you must also put `title`, `year`, and `play_count` in the payload, just like the example below. **Otherwise, the data will be lost!**
```
PUT song/_doc/1
{
	"artist": "Better Artist",
	"title": "Good Song",
	"year": 2000,
	"play_count": 10000
}
```
Be noted, each update gives you a new version of the data. This version is stored in `_version` field in the return response.

### Partial Update
However, if you wish to only update without including the whole data payload, you can use Partial Update. To do this, hit `POST <index>/_update/<id>` with the fields you want to update inside `"doc"` structure, like this example below
```
POST <index>/_update/<id>
{
    "doc":{
        "artist": "Best Artist"
    }
}
```
Be noted, partial update also gives you a new version of the data. This version is stored in `_version` field in the return response.

## Read
To read or get the data from specific index, you can hit `GET <index>/_doc/<id>`
```
GET song/_doc/1
```
If you only want specific fields, add `_source_includes=<field1>,<field2>,...` as query parameter. Separate each field with coma
```
GET song/_doc/1?_source_includes=artist,title
```

## Delete
To delete data from specific index, you can hit `DELETE <index>/_doc/<id>`
```
DELETE song/_doc/2
```

References:
- https://www.elastic.co/guide/en/elasticsearch/reference/current/docs-get.html
- https://www.elastic.co/guide/en/elasticsearch/reference/current/docs-delete.html
- https://www.elastic.co/guide/en/elasticsearch/reference/current/docs-update.html
- https://www.elastic.co/guide/en/elasticsearch/reference/current/docs-index_.html