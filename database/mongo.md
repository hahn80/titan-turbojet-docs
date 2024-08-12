# Mongo Query Arrow

This `mongodb` service can be used to query any collection to arrow file.

Remember to call the right service:
```JAVA
String service = "mongodb";
```

## For single input file:

Here is a sample json params:

```JSON
{
  "client_config": {
    "host": "localhost",
    "port": 27017
  },
  "output": "/tmp/tmpzbfqktrr",
  "batch_size": 1000,
  "database_name": "test",
  "collection_name": "customers",
  "query": {},
}

```

***Chú thích:***

- *client_config*: the config MAP to connect to mongo db (HashMap).
- *output*: the output result after query (String).
- *database_name*: name of the database (String)
- *collection_name*: name of the collection in a database (String)
- *query*: query information (HashMap). For example {"firstname": "Smith"} or just empty Map.
