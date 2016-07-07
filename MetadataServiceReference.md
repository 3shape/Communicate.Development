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

A list of case related actions that can be performed on the service 

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

This section describes accessing the different files on a case

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

A design is described with the following fields
```
    {
        Id (Guid) - Id of the attachment
        ScanTimestamp (DateTime)
        Type (string) - Describing the type of design 
        Href (Uri) - Link to download the file
        Hash (Sha1)
        FileType (string) - The file extention
    }
```
# Sample workflow
![alt tag](OAuth2Flow.png)
### Fiddler
* Generate code using following url
https://auth.3shapecommunicate.com/oauth/authorise?client_id=myId&redirect_uri=https%3A%2F%2Fwww.mysite.com%2F&response_type=code
* In fiddler post a request with following url, header and request body to get the token
Url - https://auth.3shapecommunicate.com/oauth/token
Header
Authorization: Basic T3JtY286ZDY3ZTg4MGViNmVmNDhmOThkMGNmZTBlOGYwNjNjMzU=
Content-Type: application/x-www-form-urlencoded; charset=utf-8
Request body
code=generated_code&redirect_uri=http%3A%2F%2Fmysite.com%2F%3FtmpVar%3D0&grant_type=authorization_code
* Get request to access the list of cases using token
url - https://as1.3shapecommunicate.com/api/updatedcases?from=2015-01-01%2012:00:00Z&to=2016-04-19%2012:00:00Z
Header
Authorization: Bearer token
Content-Type: application/x-www-form-urlencoded; charset=utf-8
* Get request to access the case using case_id and token
url-https://as1.3shapecommunicate.com/api/cases/(case_id)


### Approach #1: Get Cases using doctor’s credentials

**Assumption:** The doctor has already completed the account pairing process. 

**Step 1: Obtain doctor’s token.**
* Url - https://auth.3shapecommunicate.com/oauth/token

**Step 2: Obtain the Region Uri for the doctor’s Communicate user.**
* Url - https://users.3shapecommunicate.com/api/users/me 
* Make sure you use the doctor’s token in the request header
* The response will contain the doctor’s RegionUri. Remember this value, as it will be needed later in the workflow. An example of the response to the “me” endpoint is shown below.
![alt tag](ApiBrowserMe.png)

**Step 3: Get request to access the list of cases using the doctor’s token.**
* Url - https://<Doctor’s RegionUri>/api/cases?page=0&from=2016-01-01%2012:00:00Z&to=2016-04-19%2012:00:00Z
* Notice the Url to get cases depends on the doctor’s RegionUri.
* Header
    Authorization: Bearer token
    Content-Type: application/x-www-form-urlencoded; charset=utf-8
* This method returns cases 10 at a time. To browse to the next ten, use “page=1” instead of “page=0”. And so on. An example is shown below.
![alt tag](ApiBrowserCasesPage0.png)

**Step 4: Filter out cases that do not belong to client**
* 4.1) To do this, you first need the Communicate User Id for the Client account registered in the doctor’s region (Client should have 3 accounts registered), as follows:
Obtain a token for the client account in the doctor’s region, as usual:
https://auth.3shapecommunicate.com/oauth/token
Then call the “me” endpoint to get your User Id:
https://users.3shapecommunicate.com/api/users/me
![alt tag](ApiBrowserMe2.png)

* 4.2) Next, loop through all the doctor’s cases from Step 3. For each case, make sure (absolutely sure!) that it was sent to the client, by checking that at least one of the elements in the “Actors” list has the user Id found in 4.1. This is shown below.
![alt tag](ApiBrowserCasesPage0_2.png)

**Step 5: Download STL files**
* 5.1) Obtain an client token for the client user in the same region as the doctor, exactly as in step 4.1. (Note you can always just refresh your token instead of requesting a new one).
* 5.2) Using your client token, download the STL files from the Attachments list, by following the ”Href” link on every attachment of type ”stl”.
It is very important to use the client token for this. The doctor’s token will not work because our system does not allow the doctor to download STL files. This requires special permission that we have granted your accounts.
