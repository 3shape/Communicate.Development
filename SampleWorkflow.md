# Sample workflow
![alt tag](OAuth2Flow2.png)
### Fiddler
Generate code using following url
```
https://auth.3shapecommunicate.com/oauth/authorise?client_id=myId&redirect_uri=https%3A%2F%2Fwww.mysite.com%2F&response_type=code
```
In fiddler post a request with following url, header and request body to get the token
Url
```
https://auth.3shapecommunicate.com/oauth/token
```
Header
```
Authorization: Basic T3JtY286ZDY3ZTg4MGViNmVmNDhmOThkMGNmZTBlOGYwNjNjMzU=
Content-Type: application/x-www-form-urlencoded; charset=utf-8
```
Request body
```
code=generated_code&redirect_uri=http%3A%2F%2Fmysite.com%2F%3FtmpVar%3D0&grant_type=authorization_code
```
Get request to access the list of cases using token
```
https://as1.3shapecommunicate.com/api/updatedcases?from=2015-01-01%2012:00:00Z&to=2016-04-19%2012:00:00Z
```

Header
```
Authorization: Bearer token
Content-Type: application/x-www-form-urlencoded; charset=utf-8
```
Get request to access the case using case_id and token
```
url-https://as1.3shapecommunicate.com/api/cases/(case_id)
```

### Approach #1: Get Cases using doctor’s credentials

**Assumption:** The doctor has already completed the account pairing process. 

**Step 1: Obtain doctor’s token.**

Url
```
https://auth.3shapecommunicate.com/oauth/token
```

**Step 2: Obtain the Region Uri for the doctor’s Communicate user.**

Url
```
https://users.3shapecommunicate.com/api/users/me 
```
Make sure you use the doctor’s token in the request header
The response will contain the doctor’s RegionUri. 
Remember this value, as it will be needed later in the workflow. 
An example of the response to the “me” endpoint is shown below.

![alt tag](ApiBrowserMe.png)

**Step 3: Get request to access the list of cases using the doctor’s token.**

Url
```
https://<Doctor’s RegionUri>/api/cases?page=0&from=2016-01-01%2012:00:00Z&to=2016-04-19%2012:00:00Z
```
Notice the Url to get cases depends on the doctor’s RegionUri.

Header
```
Authorization: Bearer token
Content-Type: application/x-www-form-urlencoded; charset=utf-8
```
This method returns cases 10 at a time. To browse to the next ten, use “page=1” instead of “page=0”. And so on. An example is shown below.

![alt tag](ApiBrowserCasesPage0.png)

**Step 4: Filter out cases that do not belong to client**

To do this, you first need the Communicate User Id for the Client account registered in the doctor’s region (Client should have 3 accounts registered), as follows:
Obtain a token for the client account in the doctor’s region, as usual:
```
https://auth.3shapecommunicate.com/oauth/token
```
Then call the “me” endpoint to get your User Id:
```
https://users.3shapecommunicate.com/api/users/me
```

![alt tag](ApiBrowserMe2.png)

Next, loop through all the doctor’s cases from Step 3. For each case, make sure (absolutely sure!) that it was sent to the client, by checking that at least one of the elements in the “Actors” list has the user Id found above. This is shown below.

![alt tag](ApiBrowserCasesPage0_2.png)

**Step 5: Download STL files**
* 5.1) Obtain an client token for the client user in the same region as the doctor, exactly as in step 4.1. (Note you can always just refresh your token instead of requesting a new one).
* 5.2) Using your client token, download the STL files from the Attachments list, by following the ”Href” link on every attachment of type ”stl”.

It is very important to use the client token for this. The doctor’s token will not work because our system does not allow the doctor to download STL files. This requires special permission that we have granted your accounts.
