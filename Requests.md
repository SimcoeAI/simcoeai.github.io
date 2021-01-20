# Typescript Library of Gateway Requests

## BaseRequest

All requests inherit from the **BaseRequest** type:

```
export abstract class BaseRequest {
    public guid: Guid;
    public typeName: string;
    public domain: string;
    public companyId: number;
    public userId: string;

    constructor() {
        this.domain = 'CognitiveApp';
        this.guid = Guid.create().toJSON().value;
    }
}
```

All properties of the abstract class are required and cannot be sent to gateway as empty or with an invalid value. 
Two fields are populated by the abstract type's constructor and their value should not be overwritten:
- domain
- guid

Requests that implement the BaseRequest are required to populate the following field:
- typeName: a valid gateway request name

The following properties are to be populated before submitting to the gateway, from the access token:3

- companyId: company ID available in the JWT token claims
- userId: user ID available in the JWT token claims

## User Management

### List of Users

To get a list of all users in your Simcoe AI account, use the following request: 

```
export class UsersListRequest extends BaseRequest {
    constructor() {
        super();
        this.typeName = "UserListRequest";
    }
}
```

Sample request:

```
{
  "guid": "7d0290d4-d182-c3c1-aac8-a1b59f7a5929", 
  "version": 1,   
  "domain" : "CognitiveApp", 
  "companyId" : 1234,
  "userId": "28f26030-17ed-4fcf-a71c-e30bca7ed966",
  "typeName": "UserListRequest"
}
```

Sample response:

```
{
   "guid":"7d0290d4-d182-c3c1-aac8-a1b59f7a5929",
   "version":0,
   "result":{
      "results":[
         {
            "id":"0030270f-3c9e-4e61-ab56-560d7dc4f3bc",
            "firstName":"Karen",
            "lastName":"Smith",
            "emailAddress":"karen.smith@somecompany.com"
         },
         {
            "id":"49719770-b8d1-4dec-9127-9e93224b3b5e",
            "firstName":"Joel",
            "lastName":"Doe",
            "emailAddress":"joel.doe@somecompany.com"
         }
      ],
      "code":1,
      "details":[
         
      ],
      "technicalDetails":[
         
      ]
   }
}
```

### Invite a User

To invite a user to join your Simcoe AI company, use the following request:

```
export class SendInvitationRequest extends BaseRequest {
    public toEmailAddress: string;
    public toName: string;

    constructor() {
        super();
        this.typeName = 'UserInvitationRequest';
    }
}
```

Both fields are required:

- toEmailAddress: the email address of the recipient of this invitation
- toName: the name of the recipient which is used in the invitation email's subject and body

**Sample Request**

```
{
   "guid": "fdf974ac-90f9-4c1a-3370-aca2c971c4a8", 
   "version": 1,   
   "domain" : "CognitiveApp", 
   "companyId" : 2034,
   "userId": "28f26030-17ed-4fcf-a71c-e30bca7ed966",
   "typeName": "UserInvitationRequest",
   "toEmailAddress: "jane.doe@converge.com",
   "toName": "Jane"
}
```

**Sample Response**

A successful response looks like the following with a code of 1: 

```
{
   "guid":"fdf974ac-90f9-4c1a-3370-aca2c971c4a8",
   "version":0,
   "result":{
      "results":true,
      "code":1,
      "details":[
         "Email has been sent for invitation 'abc123'"
      ],
      "technicalDetails":[
         
      ]
   }
}
```

### Remove a User

To remove a user from your Simcoe AI company, use the following request:  

```
export class UserDeletionRequest extends BaseRequest {
    constructor(
        public deletingUserId: string
    ) {
        super();
        this.typeName = "UserDeletionRequest";
    }
}
```

Following field is required:

- deletingUserId: ID of the user to delete

**Sample Request**
```
{
   "guid": "fdf974ac-90f9-4c1a-3370-aca2c971c4a8", 
   "version": 1,   
   "domain" : "CognitiveApp", 
   "companyId" : 2034,
   "userId": "28f26030-17ed-4fcf-a71c-e30bca7ed966",
   "typeName": "UserDeletionRequest",
   "deletingUserId": "7053099e-2fc0-4dc2-b4c4-bfad8e03a629"
}
```

**Sample Response**

A code 1 (Success) in response indicates success:

```
{
   "guid":"fdf974ac-90f9-4c1a-3370-aca2c971c4a8",
   "version":0,
   "result":{
      "results":null,
      "code":1,
      "details":[
         
      ],
      "technicalDetails":[
         
      ]
   }
}
```

## Lake Management

### Create Lake

To create a lake of resumes, use the following request:

```
export class CreatePoolRequest extends BaseRequest {
    public poolName: string;
    public userName: string;

    constructor() {
        super();
        this.typeName = 'PoolProvisioningRequest';
    }
}
```

Properties to populate are:
- poolName: Name of the resume lake to create
- userName: Any string. This is a deprecated property and will be removed after  issue #103 is complete

**Sample Request**

```
{
   "guid": "a506cf38-fc73-aae9-5cdf-464357af4ef7", 
   "version": 1,   
   "domain" : "CognitiveApp", 
   "companyId" : 2034,
   "userId": "28f26030-17ed-4fcf-a71c-e30bca7ed966",
   "typeName": "PoolProvisioningRequest",
   "poolName": "Information Technology",
   "userName": "Test"
}
```

**Sample Response**

The "results" field returns the ID of the pool.

```
{
   "guid":"a506cf38-fc73-aae9-5cdf-464357af4ef7",
   "version":0,
   "result":{
      "results":"7F6B254D9AC32F27E6A748A66EBDB1B9",
      "code":1,
      "details":[
         
      ],
      "technicalDetails":[
         
      ]
   }
}
```