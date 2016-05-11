#Metadata Service
The Communicate Metadata service is a REST service that delivers functionality regarding the metadata of cases that have been submitted to the Communicate platform


#Environments

## Production regional servers
America
```
https://ammetadata.3shapecommunicate.com
```
Asia 
```
https://ammetadata.3shapecommunicate.com
```
Europe
```
https://ammetadata.3shapecommunicate.com
```

## Staging regional servers
This environment is for pre-release feature testing 
New features will be made available in this environment before final release to the production environment 

America
```
https://staging-ammetadata.3shapecommunicate.com
```
Europe
```
https://staging-eumetadata.3shapecommunicate.com
```


#Case requests

A list of user specific actions that can be performed on the service 

#Get Case 
Getting a case with a known Id is performed by making a get request to /api/cases/{id}. The key element to the request are is the Id. This is a GUID.

_note: write something with case versions_

## Request

Type 
```
Http get
```

Address
```
 /api/cases/{id}
```

Required field
```
There are no required fields
```

## Reponse

### Success
Header
```
http status 200
Content-Type: application/json; charset=utf-8
```

Body
```
Communicate case object
```

### Failed: Case does not exist
Header
```
http status 400
Content-Type: application/json; charset=utf-8
```

Body
```
No case with Id {id} could be found
```

### Failed: Unable to access case
This happens if a case is accessed with a user that is not an actor on the case
Header
```
http status 400
Content-Type: application/json; charset=utf-8
```

Body
```
You don't have permission to access case {id}
```

## Example 1
Url
```
GET https://eumetadata.3shapecommunicate.com/api/cases/531918a6-2879-48af-9434-a57600ac4123 HTTP/1.1
```
Header
```
Authorization: Bearer <token>
```


#Get Cases 
Getting a paged list of case.

* The page size is 10 and a counter describing the amount of cases
* List of cases is always returned in decending order (date)
* Cases are returned with in a timespan

## Request

Type 
```
Http get
```

Address

Get the first page of cases
```
 /api/cases?page=0
```

Get the first page of cases from a specific FromDate 
```
 /api/cases?page=0&from={FromDate}
```
Get the first page of cases in a timespan from the specified FromDate to the specified ToDate 
```
 /api/cases?page=0&from={FromDate}&to={ToDate}
```

Required field
```
page
```

## Reponse

### Success
Header
```
http status 200
Content-Type: application/json; charset=utf-8
```

Body
```
Cases[10]
Count(int)
```

## Example 1

### Request
Url
```
GET https://eumetadata.3shapecommunicate.com/api/cases?page=0 HTTP/1.1
```
Header
```
Authorization: Bearer <token>
```

### Response
```
{Cases : [
    { ... },
    { ... },
    { ... },
    { ... },
    { ... },
    { ... },
    { ... },
    { ... },
    { ... },
    { ... }
],
Count :  11
} 
```

## Example 2

### Request
Url
```
GET https://eumetadata.3shapecommunicate.com/api/cases?page=0&from=2000-01-01 HTTP/1.1
```
Header
```
Authorization: Bearer <token>
```

### Response
```
{Cases : [
    { ... },
    { ... },
    { ... },
    { ... },
    { ... },
    { ... },
    { ... },
    { ... },
    { ... }
],
Count :  9
} 
```

## Example 3

### Request
Url
```
GET https://eumetadata.3shapecommunicate.com/api/cases?page=0&from=2016-01-01T12:00:00Z&to=2016-01-02T18:00:00Z HTTP/1.1
```
Header
```
Authorization: Bearer <token>
```

### Response
```
{Cases : [
    { ... },
    { ... },
    { ... },
    { ... },
    { ... },
    { ... },
    { ... }
],
Count :  7
} 
```
