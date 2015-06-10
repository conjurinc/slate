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
409  | A group with that name already exists.

[read more](https://developer.conjur.net/reference/services/directory/group/create.html)

## Add Members

Add a group member.

The group member is typically a user or a group, although Conjur will permit any role to belong to a group.

Members of a group inherit all the permissions of the group.

You can add and revoke a member's admin priviledges in the group by toggling `admin_option`.

```shell
# Add user 'tony' to 'v1/ops' group

$ curl --cacert ~/conjur-demo.pem -H "$token" -X PUT \
https://conjur/api/authz/demo/roles/group/v1/ops?members \
--data-urlencode member=user:tony

Membership granted

# Add user 'donna' to 'v1/ops' group with admin option

$ curl --cacert ~/conjur-demo.pem -H "$token" -X PUT \
https://conjur/api/authz/demo/roles/group/v1/ops?members \
--data-urlencode member=user:donna \
--data admin_option=true

Adminship granted

# Revoke donna's admin on 'v1/ops' group

$ curl --cacert ~/conjur-demo.pem -H "$token" -X PUT \
https://conjur/api/authz/demo/roles/group/v1/ops?members \
--data-urlencode member=user:donna \
--data admin_option=false

Adminship revoked
```

### HTTP Request

`PUT api/authz/demo/roles/group/<group_id>?members&member=user:<member_id>`

### Query Parameters

Name | Description
---- | -----------
group_id | Name of the group, can include '/'
member_id  | Name of the user to add to the group

### Body Parameters

Name | Description
---- | -----------
admin_option | Allows the member to add and remove other group members

### Response

Code | Description
---- | -----------
200  | Group member added successfully.


## Remove Members

Removes a group member.

Once removed from a group, a role loses any permissions that it had inherited from the group.

```shell
# Remove user 'donna' from the 'v1/ops' group
$ curl --cacert ~/conjur-demo.pem -H "$token" -X DELETE \
"https://conjur/api/authz/demo/roles/group/v1/ops?members&member=user:donna"
200
```

### HTTP Request

`DELETE api/authz/demo/roles/group/<group_id>?members&member=user:<member_id>`

### Query Parameters

Name | Description
---- | -----------
group_id | Name of the group, can include '/'
member_id  | Name of the user to add to the group

### Response

Code | Description
---- | -----------
200  | Group member removed successfully.