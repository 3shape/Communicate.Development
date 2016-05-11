# Authentication Service


The Authentication Service is a simple implementation of the OAuth standard

[OAuth standard >][OAuth standard page]

# Environments


## Production 
```
https://auth.3shapecommunicate.com
```

## Staging
This environment is for pre-release feature testing 
New features will be made available in this environment before final release to the production environment 

``` 
https://staging-auth.3shapecommunicate.com
```


# Token requests

## Get token
The process of retriving a token that can be used to authenticate the user with Communicate Services

### Request 

Type
```
Http post
```

Address
```
/oauth/token
```

Header

See the [OAuth standard][OAuth standard page] for more information about the token field 

```
Authorization: Basic <token>
```

Required field
```
username
password
grant_type=password
```

### Response
**Success**

Header
```
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
```

Body
```
access_token
```


**Failed: invalid credentials or user does not exist**

Header
```
HTTP/1.1 401 Unauthorized
Content-Type: application/json; charset=utf-8
```

Body
```
access_token
```

### Example 1
Url
```
POST https://auth.3shapecommunicate.com/oauth/token HTTP/1.1
```

Header
```
Authorization: Basic d2VsX2FwqTEw0ndlYl9zcGkxMF8zZvNyZLQ=
```

Body
```
username=some@email.com&password=123456&grant_type=password
```

## Refresh token
_todo_

## Token info
The process of retriving information about the token

### Request 

Type
```
Http get
```

Address
```
/oauth/token?access_token={access_token}
```

Required field
```
access_token
```

### Response

**success**

Header
```
HTTP/1.1 200 Ok
Content-Type: application/json; charset=utf-8
```

Body
```
User (Email)
Client (OAuth client name)
ClientType (OAuth client type)
LifeTime (Lifetime)
UtcIssued (Issued time)
```


**Failed: Invalid token**

Header
```
HTTP/1.1 500 Internal Server Error
Content-Type: application/json; charset=utf-8
```

Body
```
An error has occurred.
```


[OAuth standard page]: http://oauth.net/2/
