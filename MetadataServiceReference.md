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
Http post
```

Address
```
 /api/case/{id}
```

Required field
```
_There are no required fields_
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
