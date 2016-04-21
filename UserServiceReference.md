User Service
======================

The User Service service is the central location for access to and management of all shared user information. 


Environments
======================
List of available environment and their use

## Production 
```
https://users.3shapecommunicate.com
```

## Staging
This environment is for pre-release feature testing 
New features will be made available in this environment before final release to the production environment 

``` 
https://staging-users.3shapecommunicate.com
```

User requests
======================
A list of user specific actions that can be performed on the service 

Create User 
--------------

### Request type
```
Http post
```

### Address
```
 /api/users
```
### Required field
The following fields are required for creating a user.

```
 AddressLine(Max50)
 City(Max50)
 Country(ISO 3166 A2)
 Email(Max255)
 Name(Max255)
 Password(Max128)
 PhoneNumber(Max32)
 PostalCode(Max50)
```

### Reponse


### Example 1

Request header
```
POST https://users.3shapecommunicate.com/api/users HTTP/1.1
Content-Type: application/json; charset=utf-8
Host: users.3shapecommunicate.com
Expect: 100-continue
```

Request body
```
{
    "Password":"123QWEasd",
    "Id":"00000000-0000-0000-0000-000000000000",
    "Email":"dan@dentist.com",
    "IsApproved":false,
    "Name":"Dan the Dentist",
    "FirstName":"Dan",
    "LastName":"the Dentist",
    "PhoneNumber":"1800-dentist",
    "MobilePhoneNumber":null,
    "ClientVersion":0,
    "AddressLine":"345 Perry St",
    "PostalCode":"10014",
    "Country":"us",
    "City":"New York",
    "RegionUri":null,
    "Region":null,
    "Roles":["Clinic"],
    "ConnectedUsers":null,
    "Connections":null,
    "Dongles":[],
    "HasLogo":false,
    "Logo":null,
    "ContactName":"Dan the Dentist",
    "ContactEmail":"dan@dentist.com",
    "ContactPhoneNumber":"1800-dentist",
    "ContactConsentGiven":false,
    "Website": "http://danthedentist.com",
    "NotificationSettings":null
}
```

### Reponse
Communicate user object


Get User 
--------------
There are multiple ways of retriving information about a User 
**Note:If requesting the object of a user who is not you, the content of the user object will be filtered to match your relationship with the user**

### Request type
```
Http get
```

### Address
This endpoint will return the user object for the user that matches the token the request was made with
```
/api/users/me
```

This endpoint will return the user object that matches the provided id
```
/api/users/{id}
```

This endpoint will return the user object that matches the provided email
```
/api/users/{email}
```

### Reponse
Header
```
http status 200
Content-Type: application/json; charset=utf-8
```

Body
```
Communicate user object
```


### Example 1
Request header
```
GET https://users.3shapecommunicate.com/api/users/me HTTP/1.1
Authorization: Bearer <token>
Host: users.3shapecommunicate.com
```

### Example 2
Request header
```
GET https://users.3shapecommunicate.com/api/users/531918a6-2879-48af-9434-a57600ac4123 HTTP/1.1
Authorization: Bearer <token>
Host: users.3shapecommunicate.com
```

### Example 2
Request header
```
GET https://users.3shapecommunicate.com/api/users/dan@dentist.com HTTP/1.1
Authorization: Bearer <token>
Host: users.3shapecommunicate.com
```

Connection requests
======================
Connections is the way which Communicate users use to exchange data. If a approved connection is not available between two users they are not able to collaborate on a case.

Request Connection
--------------
The process of initiating a connection, this requires access to both users Communicate User Id


### Request type
```
Http post
```

### Address
This endpoint will request a connection from UserId the user the authorization token belongs to - to the remote user with the provided id (RemoteUserId)  

```
/api/users/{userId}/requestconnection/{remoteUserId}
```

### Reponse
Header
```
http status 200
Content-Type: application/json; charset=utf-8
```

Body
```
Communicate user object
```

### Example 1
Request header
```
POST http://users.3shapecommunicate.com/api/users/95e8dfe9-9e32-48d0-adad-9fe200f1447c/requestconnection/5d80ea7f-feb9-41a3-ad8c-a0a8010f0477 HTTP/1.1
Authorization: Bearer <Token>
Host: users.3shapecommunicate.com
```

Approve Connection
--------------
Performing this request approves a connection between two parties. This request must be performed by the user (with the token of) that was specified as the remote user

### Request type
```
Http post
```

### Address
This endpoint will approve the connection between two parties

```
/api/users/{userId}/connection/{connectionId}/approve 
```

### Reponse
Header
```
http status 200
Content-Type: application/json; charset=utf-8
```

### Example 1
Request header
```
POST http://users.3shapecommunicate.com/api/users/5d80ea7f-feb9-41a3-ad8c-a0a8010f0477/connection/94457d78-ad6d-4f90-8eaf-a59100e438ea/approve HTTP/1.1
Authorization: Bearer <Token>
Host: users.3shapecommunicate.com
```