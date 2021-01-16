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

Requests that implement the BaseRequest are required to populate the following fields:
- typeName: a valid gateway request name
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

Sample response:

```
{
   "guid":"64daa91c-9bf5-dae4-5652-49fab66df091",
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

A successful response looks like the following with a code of 1: 
```
{
   "guid":"f17ed669-5fee-d99a-615e-368e57a95b2b",
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

- deletingUserId: ID of the user 

A code 1 (Success) in response indicates success:

```
{
   "guid":"f17ed669-5fee-d99a-615e-368e57a95b2b",
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