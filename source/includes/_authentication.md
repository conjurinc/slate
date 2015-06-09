# Authentication

Conjur authentication is based on auto-expiring tokens, which are issued by Conjur when presented with both:

* A login name
* A corresponding password or API key

The Conjur Token provides authentication for API calls.

For API usage, it is ordinarily passed as an HTTP Authorization "Token" header.

`Authorization: Token token="eyJkYX...Rhb="`

Properties of the token include:

* It is JSON.
* It carries the login name and other data in a payload.
* It is signed by a private authentication key, and verified by a corresponding public key.
* It carries the signature of the public key for easy lookup in a public key database.
* It has a fixed life span of several minutes, after which it is no longer valid.

## Authenticate

Exchange an API key for an authentication token to be used for further API calls.

```shell
$ curl --cacert ~/conjur-demo.pem -X POST \
"https://conjur/api/authn/users/donna/authenticate" \
--data cmwd3x2rmj8934mbmkp2nexc813pz8r1c2m3ertm2d9drja3qdgvn6

{
    "data": "donna",
    "timestamp": "2013-08-28 21:04:24 UTC",
    "signature": "F447CB3fDlfHln0wr8ZpM2nO5y9yL492FrldSrIahxtmmcoE4rK1sNmpuGPPdpTOg4YeisSNRZdC7b90KB-9NDq9Oj57oeDANUVIWcPdeb-3tx0qPC_z7PWQ3Sc1j41zFgdCNxGJdKs3epHuSLivzzHUXVii97zR-mJ2CgCoHFKyKOXnjMkQgF35nf_cgbgr6kqnz3V-bykM_Tz5g8czGZ6HZuOYf3m5tvppODuW5waE5t8t1lQ-bCOSxar_dOoMYd_O3S-HjwtMtG4uZ-SUPlHCpTgs94lI8fT0qGbHrtO9Fvww3ik9kMGemfuaN2joBbPN4gyIGA6eaur3Z6zE8F8KvlGAD7M8NuT4Fh4_1oAJKLovfyzO_uaqxcAT1qsK",
    "key": "f4479babe7006e5c7ec4fc4e8c0d5370"
}
```

### HTTP Request

`POST api/authn/users/:login/authenticate`

### Query Parameters

Parameter | Description
--------- | -----------
login | Login name of the Conjur identity.

### Body Parameters

The API key should be the entire message body.

### Response

> Before the token can be used to make subsequent calls to the API, it must be formatted.
>Take the response from the `authenticate` call and base64-encode it and strip out newlines, like so:

> `$ token=$(echo -n $response | base64 | tr -d '\r\n')`

> The token can now be used for Conjur API access.

Code | Meaning
---- | -----------
200  | The credentials were accepted, and a signed token is returned.
401  | The credentials were not accepted.

<aside class="notice">
If you have the Conjur CLI installed you can get a pre-formatted token with <code>conjur authn authenticate -H</code>.
</aside>

## Login

Exchanges a User's login and password for the api_key.
Once the API key is obtained, it may be used to rapidly obtain authentication tokens,
which are required to use most other Conjur services.

```shell

# the value for the basic auth header is obtained with
$ echo myusername:mypassword | base64

# request
$ curl --cacert ~/.ssh/conjur-conjurops.pem \
-H "Authorization: Basic ZHVzdGluOm7dfStpbHVzMQ==" \
https://conjur/api/authn/users/login

# response
1dsvap135aqvnv3z1bpwdkh92052rf9csv20510ne2gqnssc363g69y
```

### HTTP Request

`GET api/authn/users/login`

### Headers

Name | Description
---- | -----------
Authorization  | The login name and password, formatted as HTTP Basic Authentication.

### Response

Code | Description
---- | -----------
200  | The response body is the API key.
401  | The credentials were not accepted.

Passwords are stored in the Conjur database using bcrypt with a work factor of 12. Therefore, login is a fairly expensive operation.

<aside class="notice">
If you login through the command-line interface, you can print your current logged-in identity with the authn whoami command.
</aside>