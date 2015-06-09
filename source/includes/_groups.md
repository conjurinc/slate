# Groups

A group in Conjur is a collection of users. Permissions are best applied at the group level to allow
easier management.

## Create

Create a new group.

The `id` for the group will be randomly generated if not specified.
You will own the group unless you specify an `ownerid`.

```shell
# Create a group 'contractors' owned by the 'security_admin' group
$ curl --cacert ~/conjur-demo.pem -H $token -X POST \
"https://conjur/api/groups?id=contractors&ownerid=demo:group:security_admin"

{
  "id": "contractors",
  "userid": "demo:user:demo",
  "ownerid": "demo:group:security_admin",
  "roleid": "demo:group:contractors",
  "resource_identifier": "demo:group:contractors"
}
```

### HTTP Request

`POST api/groups`

### Query Parameters

Name | Description
---- | -----------
id  | Name of the group, optional.
ownerid  | Owner of this group, optional.
gidnumber  | GID number to be associated with the group, optional, default none.

### Response

Code | Description
---- | -----------
201  | Group creation was successful.
401  | A group with that name already exists.

[read more](https://developer.conjur.net/reference/services/directory/group/create.html)