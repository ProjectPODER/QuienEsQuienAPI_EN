# Query endpoints
## /companies
Returns a JSON objects list Type [Popolo Organization](http://www.popoloproject.com/specs/organization.html).

Returns a businesses or civil associations list.
If a reference expansion is specified, memberships and a summary of the organization’s contracts will be listed.

### Filters
#### id
Unique identifier for the business or partnership. Data type: string.
#### name
Partial or full name of the business or partnership. Searches for main and alternative names. Data type: string or regular expression.
#### contract_count.supplier
Number of contracts the business or partnership takes partº in. Data type: integer. Default: empty.

Note: supports min and max syntax. For min, use: `contract_count.supplier=>10` for max, use: `contract_count.supplier=<100`. For min and max, it can be used twice: `contract_count.supplier=>10&contract_count.supplier=>10`.
#### contract_amount.supplier
Amount of the contracts provided by this business. It’s important to note that the amounts are entered as absolute values, disregarding exchange rates. Data type: integer. Default: empty.

Note: supports min and max syntax. For min, use: `contract_count.supplier=>10` for max, use `contract_count.supplier=<100`. For min and max, it can be used twice: `contract_count.supplier=>10&contract_count.supplier=>10`.


#### country (not implemented)
Name or ISO 3166-1 alpha code for the country it belongs to. Data type: string.
#### source (not implemented)
Name of the source where the info was imported from. Data type: string.


## /institutions
Returns a JSON type objects list [Popolo Organization] (http://www.popoloproject.com/specs/organization.html).

Returns a public institutions list.
If a reference expansion is specified, memberships and a summary of the organization’s contracts will be listed.

### Filters
#### id
Unique identifier for the institution. Data type: string.
#### name
Institution partial or full name. Searches for main and alternative names. Data type: string or regular expression.

#### contract_count.supplier and contract_count.buyer
Number of contracts the institution takes part in. Many appear in contracts as buyer and provider; therefore they can be filtered and sorted by both criteria type: integer. Default: empty.

Note: supports min and max syntax. For min, use: `contract_count.supplier=>10` for max: `contract_count.supplier=<100`. For min and max, it can be used twice: `contract_count.supplier=>10&contract_count.supplier=>10`.
#### contract_amount.supplier and contract_amount.buyer
Amount of the contracts provided by this institution. Many appear in contracts as buyer and provider; therefore they can be filtered and sorted by both criteria. It’s important to note that the amounts are entered as absolute values, disregarding exchange rates. Data type: integer. Default: empty.

Note: supports min and max syntax. For min, use: `contract_count.supplier=>10` for max: `contract_count.supplier=<100`. For min and max, it can be used twice: `contract_count.supplier=>10&contract_count.supplier=>10`.


## /persons
Returns a JSON type objects list [Popolo Person] (http://www.popoloproject.com/specs/person.html).

If a reference expansion is specified, memberships and a summary of the person’s contracts will be listed.

### Filters
#### id
Unique identifier for the person. data type: string.
#### name
Partial or full name of the person. Searches for main and alternative names. data type: string or regular expression.

#### contract_count.supplier and contract_count.buyer
Number of contracts the person takes part in. Many appear in contracts as buyer (purchasing agent) and provider; therefore they can be filtered and sorted by both criteria type: integer. Default: empty.

Note: supports min and max syntax. For min, use: `contract_count.supplier=>10` for max, use: `contract_count.supplier=<100`. For min and max, it can be used twice: `contract_count.supplier=>10&contract_count.supplier=>10`.
#### contract_amount.supplier and contract_amount.buyer
Amount of the contracts provided by this person. Many appear in contracts as buyer (purchasing agent) and provider; therefore they can be filtered and sorted by both criteria. It’s important to note that the amounts are entered as absolute values, disregarding exchange rates. Data type: integer. Default: empty.

Note: supports min and max syntax. For min, use: `contract_count.supplier=>10` for max, use: `contract_count.supplier=<100`. For min and max, it can be used twice: `contract_count.supplier=>10&contract_count.supplier=>10`.



#### gender (not implemented)
The sex of the person. data type: string. Default: all. Possible values: male, female, other. Note: the name ‘gender’ is used to avoid issues with vulgarity filters on automatized systems.
#### country (not implemented)
Name or ISO 3166-1 alpha code for the country it belongs to. Data type: string.
#### source (not implemented)
Name of the source where the info was imported from. Data type: string.




  { htmlFieldName: "type-a", apiFieldNames:["compiledRelease.tender.procurementMethodMxCnet"], fieldLabel:"Tipo de procedimiento", type:"string", collections: ["contracts"] },
  { htmlFieldName: "size", apiFieldNames:["limit"], fieldLabel:"Resultados por página", type:"integer", hidden: true, collections: ["all"] },
  { htmlFieldName: "page", apiFieldNames:["offset"], fieldLabel:"Página", type:"integer", hidden: true, collections: ["all"] },
]


## /contracts
Devuelve un [OCDS recordPackage](https://standard.open-contracting.org/latest/en/schema/record_package/). Que incluye un listado de records, cada uno con sus release (de cada fuente) y su compiledRelease, este último es el que se utiliza para los filtros.

### Filtros
#### ocid
El identificador único del proceso de contratación (ocid). Tipo de dato: string.
#### Título: compiledRelease.contracts.title
El título del contrato. Tipo de dato: string o regular expression.
## Proveedor: compiledRelease.awards.suppliers.name
## Dependencia: compiledRelease.parties.memberOf.name
#### Fecha de inicio: compiledRelease.contracts.period.startDate
Fecha de inicio del contrato de los procesos de contratación. Tipo de dato: date (0000-00-00T00:00:00Z). Default: vacío.
#### Fecha de fin: compiledRelease.contracts.period.endDate
Fecha de fin del contrato de los procesos de contratación. Tipo de dato: date (0000-00-00T00:00:00Z). Default: vacío.

#### compiledRelease.total_amount
El importe nominal del proceso de contratación (suma de todos las adjudicaciones de este proceso). Tipo de dato: float (sin separador de miles y con '.' como separador de decimales). Default: vacío.

#### procurement_method
El procedimiento bajo el cual se realizó el proceso de contratación (adjudicación directa, licitación, etc.). Tipo de dato: string. Valores posibles: open, selective, limited, direct. Default: vacío.

#### currency (no implementado)
La moneda utilizada para especificar los importes de los procesos de contratación. Tipo de dato: string.

## /csv

Este endpoint genera versiones en CSV de los anteriores. La misma consulta se pone luego del /csv.

Ejemplo: Si tenemos una búsqueda de contratos ordenada por el importe `https://api.quienesquien.wiki/v2/contracts?sort=-compiledRelease.total_amount` y queremos el resultado en CSV podemos agregar CSV luego del v2 y antes del contracts, así: `https://api.quienesquien.wiki/v2/csv/contracts?sort=-compiledRelease.total_amount`

Lo mismo resulta válido para los otros endpoints.

Para cada tipo de entidad se generan diferentes tablas.

### Contracts

La tabla de CSV tiene las siguientes columnas:
```
"OCID", records.ocid (Repeated for each contract in a compiledRelase)
"Contract title", records.compiledRelease.contracts.title
"Suppliers name", records.compiledRelease.awards.suppliers.name
"Buyer name", records.compiledRelease.party.name
"Buyer parent", records.compiledRelease.party.memberOf.name
"Total amount", records.compiledRelase.total_amount (sum of all contracts in this compiledRelease)
"Procurement method",records.compiledRelease.tender.procurementMethod
"Start date", records.compiledRelease.contracts.period.startDate
"End date", records.compiledRelease.contracts.period.endDate
"Contract amount", records.compiledRelease.contracts.value.amount
"Contract currency", records.compiledRelease.contracts.value.currency
"Source", records.compiledRelease.source
```

### Persons
La tabla de CSV tiene las siguientes columnas:
```
'id',
'name',
'contract_amount_supplier',
'contract_count_supplier'
```

### Institutions:
La tabla de CSV tiene las siguientes columnas:
```
'id',
'name',
'classification',
'subclassification',
'contract_amount_supplier',
'contract_count_supplier',
'contract_amount_buyer',
'contract_count_buyer'
```

### Companies
La tabla de CSV tiene las siguientes columnas:
```
'id',
'name',
'classification',
'subclassification',
'contract_amount_supplier',
'contract_count_supplier',
'contract_amount_buyer',
'contract_count_buyer'
```


## /sources
Devuelve un información sobre cantidades de entidad por fuente y por tipo de entidad en QuienEsQuien.wiki.

Tiene dos objetos, uno de fuentes `sources` que tiene por cada fuente la cantidad de elementos de cada tipo de entidad. Y otro de colecciones `collections` que tiene la cantidad elementos de cada tipo de entidad.

Nota: Este endpoint está en construcción.
