---
uid: AssetsSearchAPI
---

# Assets search API
The assets search API allows you to search for your assets with search criteria.

***
## `Get Assets by Search Criteria` 
Returns an array of assets that meet the search criteria.

### Searchable Properties
Assets can be searched on the following asset properties:

- Asset Top-level Fields
  - Id, Name, Description, AssetTypeId, AssetTypeName

- Metadata Fields
  - Name, Description. Value

- Stream Reference Fields
  - StreamReferences - Stream Reference Name
  - StreamProperties - Sds Stream Property Ids, not including indices

Search criteria can be chained together using an **AND**. See examples below. 

## `Search Assets` 
Searches and returns assets matching the search criteria.

### Request 
```text 
GET api/v1-preview/Tenants/{tenantId}/Namespaces/{namespaceId}/Assets?skip={skip}&count={count}&orderby={orderby}&query={queryString}&filter={filterString}&pageSize={pageSize}&maxPages={maxPages}&continuationToken={continuationToken}
```

### Parameters  
`string tenantId`  
The tenant identifier

`string namespaceId`  
The namespace identifier

`string queryString`  
The asset query string. Search strings are not case-sensitive.

`string filterString`  
String used to filter the asset search results. Filter strings are not case sensitive. The strings on which you can filter results are limited to the asset AssetTypeName property and the asset metadata properties (using the syntax filter[*property_name*]=*property_value*).

[optional] `int skip`   
An optional parameter representing the zero-based offset of the first asset to retrieve. If not specified, a default value of 0 is used.

[optional] `int count`   
An optional parameter, between 1 and 1000 (inclusive), that represents the maximum number of assets to retrieve. If not specified, the default is 100.

[optional] `[id|name] [asc|desc|] orderby`  
An optional parameter which returns assets ordered either by the asset `Id` or the asset `name`. Specify asc or desc to return the results in ascending or descending order. If not specified, the default is ascending order.

[optional] `int pageSize`   
Must be used with `maxPages` to configure the pages of the search results. This parameter defines the maximum number of assets that are returned on each page. 

[optional] `int maxPages`   
Must be used with `pageSize` to configure the pages of the search results. This parameter defines the maximum number of pages. Search results that exceed maxPages are not returned. 

[optional] `string continuationToken`   
Retrieves a specific page of the search results. 

### Response 
Returns an array of assets matching the search query and the total number of assets returned specified as Total-Count in the HTTP response header. 

| Status Code | Body Type | Description |
|--|--|--|
| 200 OK | Asset list | A list of assets matching the search criteria. An empty array is returned if there are no matching assets. |
| 204 OK | empty | No assets match your search criteria. |
| 400 Bad Request | error | The request is not valid. See the response body for additional details. |
| 403 Forbidden | error | You are not authorized to view the requested asset. |


## `Search Matched Fields Asset` 
Searches all assets and returns a list of asset Ids and their matched fields. Use this API to identify the fields in the asset that match your query string.

### Request 
```text 
GET api/v1-preview/Tenants/{tenantId}/Namespaces/{namespaceId}/search/Assets?skip={skip}&count={count}&orderby={orderby}&query={queryString}&filter={filterString}&pageSize={pageSize}&maxPages={maxPages}&continuationToken={continuationToken}
```

### Parameters  
`string tenantId`  
The tenant identifier

`string namespaceId`  
The namespace identifier

`string queryString`  
The asset query string. Search strings are not case-sensitive.

`string filterString`  
String used to filter the asset search results. Filter strings are not case sensitive. The strings on which you can filter results are limited to the asset AssetTypeName property and the asset metadata properties (using the syntax filter[*property_name*]=*property_value*).

[optional] `int skip`   
An optional parameter representing the zero-based offset of the first asset to retrieve. If not specified, a default value of 0 is used.

[optional] `int count`   
An optional parameter, between 1 and 1000 (inclusive), representing the maximum number of retrieved assets. If not specified, the default is 100.

[optional] `[id|name] [asc|desc|] orderby`  
An optional parameter which returns assets ordered either by the asset `Id` or the asset `name`. Specify asc or desc to return the results in ascending or descending order. If not specified, the default is ascending order.

[optional] `int pageSize`   
Must be used with `maxPages` to configure the pages of the search results. This parameter defines the maximum number of assets that are returned on each page. 

[optional] `int maxPages`   
Must be used with `pageSize` to configure the pages of the search results. This parameter defines the maximum number of pages. Search results that exceed maxPages are not returned.

[optional] `string continuationToken`   
Retrieves a specific page of the search results. 

### Response 
A list of asset Ids and their matched fields.

| Status Code | Body Type | Description |
|--|--|--|
| 200 OK | Asset list | A list of assets matching the search criteria. An empty array is returned if there are no matching assets. |
| 204 OK | empty | No assets match your search criteria. |
| 400 Bad Request | error | The request is not valid. See the response body for additional details. |
| 403 Forbidden | error | You are not authorized to view the requested asset. |

#### Example response body
Below is a response when query string is "Name:*Tracer*".

`Count` represents the number of matched assets in the given query.

Also returned is a list of `Results`. Each result contains  
- The `MatchProperties` - a list of matched property objects. Each `MatchProperties` object contains the matched fields and their values.
- `Score` - number that indicates the relevancy of the match.
- `Id` - Id of the matched asset.
- `TypeId` - asset type Id of the asset. This is null if the asset does not reference an asset type.
- `Name` - asset name.
- `Description` - asset description.

```json 
HTTP 200 OK 
Content-Type: application/json
{
    "Count": 2,
    "Results": [
        {
            "MatchedProperties": [
                {
                    "Field": "Name",
                    "Value": "Asset Tracer ced7ee16-984d-480f-8338-3055f7f39d8b"
                }
            ],
            "Score": 1,
            "Id": "AssetId2b5f41ae-0929-4977-bfbd-1e046d8a66f4",
            "TypeId": "AssetTracerType",
            "Name": "Asset Tracer ced7ee16-984d-480f-8338-3055f7f39d8b",
            "Description": "First tracer device"
        },
        {
            "MatchedProperties": [
                {
                    "Field": "Name",
                    "Value": "Asset Tracer d6b984dd-b6da-4225-a2e0-59f781d065a4"
                }
            ],
            "Score": 1,
            "Id": "AssetId3dbfd185-7c62-49ed-b875-7953cba07fc3",
            "TypeId": "AssetTracerType",
            "Name": "Asset Tracer d6b984dd-b6da-4225-a2e0-59f781d065a4",
            "Description": "Another tracer device"
        }
    ]
}
```

### Examples of asset query strings

| Query String                   | Description                                                  |
| ------------------------------ | ------------------------------------------------------------ |
| Id:Id1                         | Returns the asset with `Id` equal to **Id1**.                  |
| Id:Id1 Name desc               | Returns the asset with `Id` equal to **Id1** return results in descending order by Name. |
| Id:Id*                         | Returns all assets with `Id` matching **id*** wildcard. |
| Name:Name1                     | Returns all asset with a friendly name equal to **Name1**. |
| Id:Id AND Name:Name1           | Returns all assets with `Id` matching the **id** and with a friendly name equal to **Name1**. |
| Description:floor1*            | Returns all assets with a description that starts with **floor1**. |
| Metadata/Name:Building*        | Returns all assets with at least one metadata name whose description contains the string **Building**. |
| Metadata/Description:heater*   | Returns all assets with at least one metadata whose description starts with **heater**. |
| Metadata/Value:123             | Returns all assets with at least one metadata whose Value property equals **123**. |
| Id:X* AND Metadata/Name:B*     | Returns all assets with `Id` starting with **X** and containing at least one metadata value with a name that starts with a **B**. |
| AssetTypeId:HeaterTypeId       | Returns all assets with `AssetTypeId` matching `HeaterTypeId` |
| AssetTypeName:HeaterTypeName   | Returns all assets whose `Name` field of the asset type matches **HeaterTypeName** |
| StreamProperties:Pressure      | Returns all assets that have one or more stream references with the stream property ID **Pressure**. Note: This search only searches non-key Sds stream properties. |
| StreamReferences:Name1         | Returns all assets whose stream references contain a stream reference name that matches **Name1**. |

### Special characters in search queries

Add the backslash escape character ( \ ) before any special characters in search queries. The following special characters require an escape character:   " |  /  *  \  ( )  : 

The following are examples of using the escape character in query strings.

| Example Field Value                    | Query String                               |
| -------------------------------------- | ------------------------------------------ |
| Austin\Dallas\Fort Worth               | Austin\\\Dallas\\\Fort Worth               |
| 1:100                                  | 1\\:100                                    |

### Examples of asset filter strings

Filter strings are not case sensitive. Numeric types must be passed as strings according to English locale. For example, a double of 1.1 must be sent as "1.1".

| Query String                                 | Description                                                  |
| -------------------------------------------- | ------------------------------------------------------------ |
| filter[location]=Earth                       | Returns all assets that contain a metadata name = **location** and value is **Earth**. |
| filter[location]=Earth&filter[device]=tracer | Returns all assets that contain a metadata name = **location** and value is **Earth** and also contains a metadata with name = **device** and value is **tracer**. |


## `Asset Results by Pages` 

Asset search results can be returned in pages through `pageSize` ,`maxPages`, and `continuationToken` query parameters.

To return search results in pages, first make a call using the `pageSize` and `maxPages` parameters. For example:

`GET search/assets?pageSize=10&maxPages=100` returns search results with a maximum of 10 assets per page and a maximum of 100 pages. 

The HTTP header of this response of this query will include a "Link" section to which will define either first, next, previous, or last pages. These links can be used directly to follow the pages. For example, the links section may contain:

```json 
<https://{clusterName}/api/v1-preview/tenants/{tenantId}/namespaces/{NamespaceId}/Assets?continuationToken={continuationToken}>; rel="next", 
<https://{clusterName}/api/v1-preview/tenants/{tenantId}/namespaces/{NamespaceId}/Assets?continuationToken={continuationToken}>; rel="previous",
<https://{clusterName}/api/v1-preview/tenants/{tenantId}/namespaces/{NamespaceId}/Assets?continuationToken={continuationToken}>; rel="first",
<https://{clusterName}/api/v1-preview/tenants/{tenantId}/namespaces/{NamespaceId}/Assets?continuationToken={continuationToken}>; rel="last"
```

These links can be followed to get the next, previous, first, and last page.  If no pages are requested, the links to the next, previous, first, and last pages time out in 2 minutes.

## `Asset Faceted Search` 

Asset faceted search allows for searching using asset facets. Asset facets are not case sensitive.  Only asset metadata can be used in asset faceted searches.

### Request 

```text 
GET api/v1-preview/Tenants/{tenantId}/Namespaces/{namespaceId}/search/Assets/facets?name={category}&count={count}
```

### Parameters  

`string tenantId`  
The tenant identifier

`string namespaceId`  
The namespace identifier

`string category`  
The name of the asset metadata for which you want to retrieve the facet values.

[optional] `int count`   
An optional parameter, between 1 and 1000 (inclusive), representing the maximum number of retrieved assets. If not specified, the default is 100.

### Response 

A list of asset Ids and their matched fields.

| Status Code     | Body Type        | Description                                                  |
| --------------- | ---------------- | ------------------------------------------------------------ |
| 200 OK          | Asset facet list | A list of asset facet values that match the search criteria. An empty array is returned if there are no matching asset metadata with the supplied category. |
| 204 OK          | empty            | No assets match your search criteria.                        |
| 400 Bad Request | error            | The request is not valid. See the response body for additional details. |

### Example 

If you have the following assets in your system:

| Assets in System                                             |
| ------------------------------------------------------------ |
| "Id": "CaliforniaAsset",    "Metadata": [      { "Name": "Manufacturer", "Value": "GE", "SdsTypeCode": "String" },      { "Name": "Location", "Value": "California", "SdsTypeCode": "String" },    ] |
| "Id": "CaliforniaAsset1",    "Metadata": [      { "Name": "Manufacturer", "Value": "Rockwell", "SdsTypeCode": "String" },      { "Name": "Location", "Value": "California", "SdsTypeCode": "String" },    ] |
| "Id": "PhillyAsset",    "Metadata": [      { "Name": "Manufacturer", "Value": "GE", "SdsTypeCode": "String" },      { "Name": "Location", "Value": "Philly", "SdsTypeCode": "String" },    ] |


Performing a `GET search/assets/facets?Name=Location` returns the following response. 


```json 
HTTP 200 OK 
Content-Type: application/json
[
    {
        "Name": "Location",
        "FacetValues": [
            {
                "Value": "California",
                "DocumentCount": 2
            },
            {
                "Value": "Philly",
                "DocumentCount": 1
            }
        ]
    }
]
```
