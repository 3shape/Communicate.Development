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


# Case requests

A list of user specific actions that can be performed on the service 

## Get Case 
Getting a case with a known Id is performed by making a get request to /api/cases/{id}. The key element to the request are is the Id. This is a GUID.

Using Get Case will always return the latest version of the Case.

### Request

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

### Reponses

**Success**

Header
```
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
```

Body
```
Communicate case object
```

**Failed: Case does not exist**

Header
```
HTTP/1.1 400 Bad Request
Content-Type: application/json; charset=utf-8
```

Body
```
No case with Id {id} could be found
```

**Failed: Unable to access case**

This happens if a case is accessed with a user that is not an actor on the case
Header
```
HTTP/1.1 400 Bad Request
Content-Type: application/json; charset=utf-8
```

Body
```
You don't have permission to access case {id}
```

### Example 1
Url
```
GET https://eumetadata.3shapecommunicate.com/api/cases/531918a6-2879-48af-9434-a57600ac4123 HTTP/1.1
```
Header
```
Authorization: Bearer <token>
```

## Get Case version
Getting a case with a known Id and a specific version is performed by making a get request to /api/cases/{id}/version/{version}. 

Every time a case is updated a full version of that case is saved and will be available with this api.

### Request

Type 
```
Http get
```

Address
```
 /api/cases/{id}/version/{version}
```

Required field
```
id
version
```

### Reponses

**Success**

Header
```
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
```

Body
```
Communicate case object
```

**Failed: Case does not exist**

Header
```
HTTP/1.1 400 Bad Request
Content-Type: application/json; charset=utf-8
```

Body
```
No case with Id {id} could be found
```

**Failed: Unable to access case**

This happens if a case is accessed with a user that is not an actor on the case
Header
```
HTTP/1.1 400 Bad Request
Content-Type: application/json; charset=utf-8
```

Body
```
You don't have permission to access case {id}
```

### Example 1
Url
```
GET https://eumetadata.3shapecommunicate.com/api/cases/531918a6-2879-48af-9434-a57600ac4123/version/1 HTTP/1.1
```
Header
```
Authorization: Bearer <token>
```


## Get Cases 
Getting a paged list of case.

* The page size is 10 and a counter describing the amount of cases
* List of cases is always returned in decending order (date)
* Cases are returned with in a timespan (yyyy-mm-ddThh:mm:ssZ) 

### Request

Type 
```
Http get
```

_Address_

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

### Reponse

**Success**

Header
```
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
```

Body
```
Cases[10]
Count(int)
```

### Example 1

**Request**

Url
```
GET https://eumetadata.3shapecommunicate.com/api/cases?page=0 HTTP/1.1
```
Header
```
Authorization: Bearer <token>
```

**Response**

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

### Example 2

**Request**

Url
```
GET https://eumetadata.3shapecommunicate.com/api/cases?page=0&from=2000-01-01 HTTP/1.1
```
Header
```
Authorization: Bearer <token>
```

**Response**

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

### Example 3

**Request**

Url
```
GET https://eumetadata.3shapecommunicate.com/api/cases?page=0&from=2016-01-01T12:00:00Z&to=2016-01-02T18:00:00Z HTTP/1.1
```
Header
```
Authorization: Bearer <token>
```

**Response**

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

# Files

This section describes accessing the diffrent files on a case

## Attachment 

All files associated with a case can always be located in the Attachment section of a case.

### Description

Attachments are located at the root of the Case object
```
    {
        Id : Id of the case
        .
        .
        .
        Attachments[] 
    }
```

An attachment is described with the following fields
```
    {
        Id (Guid) - Id of the attachment
        Name (string) 
        Hash (Sha1)
        FileType (string) - The file extention
        Created (DateTime) - The time the file was created
        Updated (DateTime) - The last time the file was updated
        Href (Uri) - Link to download the file
    }
```

### Request

Type 
```
Http get
```

Address
```
 /api/cases/{id}/attachments/{attachmentId}
```

Required field
```
There are no required fields
```

### Reponses

**Success**

Header
```
HTTP/1.1 200 OK
Content-Type: application/octet-stream
filename="<NameOfTheFile>"
```

Body
```
Stream of data
```

**Failed: Don't have permissions to access the file**

Header
```
HTTP/1.1 400 Bad Request
```

Body
```
You don't have permission to access this resource.
```

### Example 1

**Request**

Url
```
GET https://eumetadata.3shapecommunicate.com/api/cases/531918a6-2879-48af-9434-a57600ac4123/attachments/417752978da43bb0fa3d03f4fef597904334993a HTTP/1.1
```
Header
```
Authorization: Bearer <token>
```

**Response**

Header
```
HTTP/1.1 200 OK
Content-Type: application/octet-stream
filename="417752978da43bb0fa3d03f4fef597904334993a"
```

Body
```
stream of data
```
 
## Scans

This is a specialized section of the case, files listed in this section have additional metadata associated with them.

The Scan files are retrived in the same way described in the Attachment section.

### Description

Scans are located at the root of the Case object
```
    {
        Id : Id of the case
        .
        .
        .
        Scans[] 
    }
```

A scan is described with the following fields
```
    {
        Id (Guid) - Id of the attachment
        ScanTimestamp (DateTime)
        Type (string) - Describing the type of scan 
        Href (Uri) - Link to download the file
        Hash (Sha1)
        FileType (string) - The file extention
    }
```

## Designs

This is a specialized section of the case, files listed in this section have additional metadata associated with them.

The Designs files are retrived in the same way described in the Attachment section.

### Description

Designs are located at the root of the Case object
```
    {
        Id : Id of the case
        .
        .
        .
        Designs[] 
    }
```

A scan is described with the following fields
```
    {
        Id (Guid) - Id of the attachment
        ScanTimestamp (DateTime)
        Type (string) - Describing the type of scan 
        Href (Uri) - Link to download the file
        Hash (Sha1)
        FileType (string) - The file extention
    }
```