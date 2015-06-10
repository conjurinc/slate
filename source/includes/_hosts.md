# Hosts

A host in Conjur is any non-human actor in your system. Servers, containers, scripts and so on.

## Create

Create a new host.

The `id` for the host will be randomly generated if not specified.
You will own the host unless you specify an `ownerid`.

```shell
# Create a host identity 'ec2/i-1a2b3c4d' for an AWS instance

$ curl --cacert ~/conjur-demo.pem -H $token -X POST \
https://conjur/api/hosts \
--data-urlencode id=ec2/i-1a2b3c4d \
--data-urlencode ownerid=demo:group:developers

{
  "id": "ec2/i-1a2b3c4d",
  "userid": "demo",
  "created_at": "2015-04-09T18:06:02Z",
  "ownerid": "demo:group:developers",
  "roleid": "demo:host:ec2/i-1a2b3c4d",
  "resource_identifier": "demo:host:ec2/i-1a2b3c4d",
  "api_key": "18pmzcn23g55vq8j34na32mnx283f5t9y5270pvkm2myq3qs2y0v73n"
}
```

### HTTP Request

`POST api/hosts`

### Query Parameters

Name | Description
---- | -----------
id  | Name of the host, optional.
ownerid  | Owner of this host, optional.

### Response

Code | Description
---- | -----------
201  | Host creation was successful.
409  | A host with that name already exists.

[read more](https://developer.conjur.net/reference/services/directory/host/create.html)
