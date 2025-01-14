# createAlias()

This method creates a alias for a collection.

## Invocation

```javascript
new milvusClient(MILUVS_ADDRESS).collectionManager.createAlias(CreateAliasReq);
```

## Parameters

### CreateAliasReq(object)

| Parameter       | Description                                                                            | Type   | Required |
| --------------- | -------------------------------------------------------------------------------------- | ------ | -------- |
| collection_name | Target collection name                                                                 | String | True     |
| Alias           | Alias name                                                                             | String | True     |
| timeout         | An optional duration of time in millisecond to allow for the RPC. Default is undefined | Number | False    |

## Example

```javascript
new milvusClient(MILUVS_ADDRESS).collectionManager.createAlias({
  collection_name: "my_collection",
  alias: "my_alias",
});
```

## Return

```javascript
// create collection return
{ error_code: 'Success', reason: '' }
```
