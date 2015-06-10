# Variables

A variable in Conjur is a container for sensitive data. This can be a password, API token, SSL certificate and so on.

## Create

Create a new variable. This is a 'secret' in Conjur and can be any value.

A variable can be created with or without an initial value.
If you don't give the variable an id, one will be randomly generated.


```shell
# Create a variable 'dev/mongo/password' owned by 'developers' group

$ curl --cacert ~/conjur-demo.pem -H $token -X POST \
https://conjur/api/variables \
--data id=dev/mongo/password \
--data ownerid=demo:group:developers \
--data kind=secret
--data mime_type=text/plain \
--data value=p89b12ep12puib

{
  "id": "dev/mongo/password",
  "userid": "demo",
  "mime_type": "text/plain",
  "kind": "password",
  "ownerid": "demo:group:developers",
  "resource_identifier": "demo:variable:dev/mongo/password",
  "version_count": 1
}
```

### HTTP Request

`POST api/variables`

### Body Parameters

Name | Description
---- | -----------
id  | Name of the variable, optional.
ownerid  | Owner of this variable.
mime_type  | Media type of the variable.
kind  | Purpose of this variable.
value  | Value of this variable.

### Response

Code | Description
---- | -----------
201  | Variable creation was successful.
409  | A variable with that name already exists.

[read more](https://developer.conjur.net/reference/services/directory/host/create.html)

## Value

Retrieve the value of a variable.

By default this returns the latest version of a variable, but you can retrieve any earlier version as well.

Variable ids must be escaped in the url, e.g., '/' -> '%2F'.

```shell
# Retrieve the value of variable 'dev/mongo/password'

$ curl --cacert ~/conjur-demo.pem -H $token \
https://conjur/api/variables/dev%2Fmongo%2Fpassword/value

p89b12ep12puib
```

### HTTP Request

`GET api/variables/:id/value?version=:version`

### Body Parameters

Name | Description
---- | -----------
id  | Name of the variable.
version  | Version of the variable to retrieve, optional.

### Response

Code | Description
---- | -----------
202  | The variable's value.
404  | Variable not found.

[read more](https://developer.conjur.net/reference/services/directory/variable/value.html)

## Value Add

Add a value to a variable.

Variable ids must be escaped in the url, e.g., '/' -> '%2F'.

```shell
# Update the value of variable 'dev/mongo/password'

$ curl --cacert ~/conjur-demo.pem -H $token -X POST \
https://conjur/api/variables/dev%2Fmongo%2Fpassword/values \
--data value=np89daed89p

Value added
```

### HTTP Request

`POST api/variables/:id/values`

### Query Parameters

Name | Description
---- | -----------
id  | Name of the variable.

### Body Parameters

Name | Description
---- | -----------
value  | New value of the variable

### Response

Code | Description
---- | -----------
201  | Value added.
404  | Variable not found.
422  | Value malformed or missing

[read more](https://developer.conjur.net/reference/services/directory/variable/values-add.html)