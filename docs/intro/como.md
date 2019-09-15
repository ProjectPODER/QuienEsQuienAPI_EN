# Reference
The API is available at https://api.quienesquien.wiki/v2/

## Filters
Querying each endpoint without parameters will return all the entries, limited by default to 25 entries. In order to narrow the results, queries can be filtered using any desired criteria. Results can also be sorted and references can be expanded.

There are some generic filters that apply to every endpoint, and others that apply to each point.

Generic filters

## limit
Number of records included on each result page. Default: 25 records. Type: 1000 >= integer > 0.

## offset
Number of records to be skipped. Default: 0 records (home page). Type: integer >= 0. If the offset is set to a greater number than the number of results, the query will return empty.

## embed
Flag that indicates if any reference to another collection is included in any document returned. This includes memberships, flags and summaries for each entity that has it. Default: false. Type: boolean.

## omit
Comma-separated list of fields to be excluded from results. Type: array.

## updated_since (not implemented)
Only show records updated after specified date. Type: date. Default: 0000-00-00T00:00:00Z.

## include_custom_fields (not implemented)
Use this flag to add custom fields to the documents returned in the query Default: none. Possible values: all, none, fields list. Type: array. Si algún campo solicitado no existe, no incluye información adicional en la respuesta.

In QQW databases, there is several additional data that can be used, alongside to the standard ones. TODO: hacer el listado de los fields disponibles para cada data type.
