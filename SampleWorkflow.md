# Sample workflow
![alt tag](OAuth2Flow2.png)

## WF1 - Pairing workflow 
Generate code using following url
```
https://auth.3shapecommunicate.com/oauth/authorise?client_id=myId&redirect_uri=https%3A%2F%2Fwww.mysite.com%2F&response_type=code
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

**Get request to access the list of cases using token**

Follow the step(s) described in section [M2.4 >] [M2.4]

**Get request to access the case using case_id and token**

Follow the step(s) described in section [M2.1 >] [M2.1]

##  WF2 - Get Cases using doctor’s credentials

**Assumption:** The doctor has already completed the account pairing process (WF1).

**Step 1: Obtain doctor’s token.**

Follow the step(s) described in section [A2.1 >] [A2.1]

**Step 2: Obtain the Region Uri for the doctor’s Communicate user.**

Follow the step(s) described in section [U2.2 >] [U2.2]

Make sure you use the doctor’s token in the request header
The response will contain the doctor’s RegionUri. 
Remember this value, as it will be needed later in the workflow. 
An example of the response to the “me” endpoint is shown below.

![alt tag](ApiBrowserMe.png)

**Step 3: Get request to access the list of cases using the doctor’s token.**

Follow the step(s) described in section [M2.3 >] [M2.3]

Notice the Url to get cases depends on the doctor’s RegionUri.

This method returns cases 10 at a time. To browse to the next ten, use “page=1” instead of “page=0”. And so on. An example is shown below.

![alt tag](ApiBrowserCasesPage0.png)

**Step 4: Filter out cases that do not belong to client**

To do this, you first need the Communicate User Id for the Client account registered in the doctor’s region (Client should have 3 accounts registered), as follows:
Obtain a token for the client account in the doctor’s region, as usual:

Follow the step(s) described in section [A2.1 >] [A2.1]

Then call the “me” endpoint to get your User Id:

Follow the step(s) described in section [U2.2 >] [U2.2]

![alt tag](ApiBrowserMe2.png)

Next, loop through all the doctor’s cases from Step 3. For each case, make sure (absolutely sure!) that it was sent to the client, by checking that at least one of the elements in the “Actors” list has the user Id found above. This is shown below.

![alt tag](ApiBrowserCasesPage0_2.png)

**Step 5: Download STL files**

* 5.1) Obtain an client token for the client user in the same region as the doctor, exactly as in step 4.1. (Note you can always just refresh your token instead of requesting a new one).

* 5.2) Using your client token, download the STL files from the Attachments list, by following the ”Href” link on every attachment of type ”stl”.

It is very important to use the client token for this. The doctor’s token will not work because our system does not allow the doctor to download STL files. 
**This requires special permission that will be granted on a case by case basis.**

[A2.1]: http://3shapeas.github.io/Communicate.Development/AuthenticationServiceReference.html#a2-token-requests-a21-get-token
[U2.2]: http://3shapeas.github.io/Communicate.Development/UserServiceReference.html#u2-user-requests-u22-get-user
[M2.3]: http://3shapeas.github.io/Communicate.Development/MetadataServiceReference.html#m2-case-requests-m23-get-cases
[M2.4]: http://3shapeas.github.io/Communicate.Development/MetadataServiceReference.html#m2-case-requests-m24-get-updated-cases
[M2.1]: http://3shapeas.github.io/Communicate.Development/MetadataServiceReference.html#m2-case-requests-m21-get-case