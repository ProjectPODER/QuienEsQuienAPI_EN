# Error messages

If the API can’t return any results, it returns an empty set but the status will still be success.

The API will return an error message if the query couldn’t be processed.

There is a 5-second limit for the database queries. Queries made on non-indexed fields usually exceed this limit and result in an error message.

## Invalid parameter
```json
{

    "message": "Validation errors",
    "errors": [
        {
            "code": "INVALID_REQUEST_PARAMETER",
            "errors": [
                {
                    "code": "PATTERN",
                    "params": [
                        "^[23456789ABCDEFGHJKLMNPQRSTWXYZabcdefghijkmnopqrstuvwxyz]{17}$",
                        "a=a"
                    ],
                    "message": "String does not match pattern ^[23456789ABCDEFGHJKLMNPQRSTWXYZabcdefghijkmnopqrstuvwxyz]{17}$: a=a",
                    "path": [ ],
                    "description": "Internal ID of person"
                }
            ],
            "in": "path",
            "message": "Invalid parameter (_id): Value failed JSON Schema validation",
            "name": "_id",
            "path": [
                "paths",
                "/persons/{_id}",
                "get",
                "parameters",
                "0"
            ]
        }
    ]

}
```

# Empty Result

If the filters’ set omits all records, the status will still be success, but with no results.

For example:

```
{

    "status": "success",
    "size": 0,
    "limit": 1,
    "offset": 0,
    "pages": 0,
    "count": 0,
    "data": [ ]

}
```

# Process timeout

The timeout for API queries to the database is 6 seconds. This means that if a query is made for an inexistent field or a non-indexed record or any other reason takes more than 6 seconds, these queries will return an error message.

For example:
```
{

    "status": "error",
    "size": 0,
    "limit": 1,
    "offset": 0,
    "pages": null,
    "count": "error: MongoError: Exec error resulting in state DEAD :: caused by :: errmsg: \"operation exceeded time limit\"",
    "data": "error: MongoError: Executor error during find command :: caused by :: errmsg: \"operation exceeded time limit\""

}
```
