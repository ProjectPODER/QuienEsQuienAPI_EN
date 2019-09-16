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
Returns an [OCDS recordPackage] (https://standard.open-contracting.org/latest/en/schema/record_package/). Includes a records list, each with its release (for each source) and its compiledRelease. The latter one is used for filters.

### Filters
#### ocid
Unique identifier for the contracting process (ocid). Data type: string.
#### Title: compiledRelease.contracts.title
The title of the contract. Data type: string or regular expression.
## Provider: compiledRelease.awards.suppliers.name
## Dependency: compiledRelease.parties.memberOf.name
#### Start date: compiledRelease.contracts.period.startDate
Start date of the contracting processes contract. Data type: date (0000-00-00T00:00:00Z). Default: empty.
#### End date: compiledRelease.contracts.period.endDate
End date of contracting processes contract. Data type: date (0000-00-00T00:00:00Z). Default: empty.

#### compiledRelease.total_amount
Nominal amount of a contracting process (sum of all the adjudications of this contract). Data type: float (without thousand separator and point as decimal separator). Default: empty.

#### procurement_method
The procedure under which the contracting process was made (direct award, tender, etc). Data type: string. Possible values: open, selective, limited, direct. Default: empty.

#### currency (not implemented)
The currency used to specify the amounts of the contracting processes. Data type: string.

## /csv

This endpoint generates CSV versions of the former ones. The same query takes place after the /csv.

Example: if we have a contracts search sorted by amount `https://api.quienesquien.wiki/v2/contracts?sort=-compiledRelease.total_amount`, and we want the result in CSV, we can add /csv/ after v2 and before contracts, like this: `https://api.quienesquien.wiki/v2/csv/contracts?sort=-compiledRelease.total_amount`

The same applies to any other endpoint.

For each entity type, different tables are generated.

### Contracts

The CSV table has the following columns:
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
The CSV table has the following columns:
```
'id',
'name',
'contract_amount_supplier',
'contract_count_supplier'
```

### Institutions:
The CSV table has the following columns:
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
The CSV table has the following columns:
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
Returns information about how many entities there are for each source and type in QuienEsQuien.wiki.

It has two objects: one of `sources`, that has for each source the quantity of elements of each entity type; and another one of  `collections`, that has the quantity of elements of each entity type.

Note: this endpoint is still under construction.
