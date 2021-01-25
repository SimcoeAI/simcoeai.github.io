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
   "companyId" : 1234,
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
   "companyId" : 1234,
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

### List Lakes

To get a lit of all resume lakes, use the following request:

```
export class LakeListQueryRequest extends BaseRequest {
    constructor(
        public type: LakeQueryTypes
    ) {
        super();
        this.typeName = 'LakeListQueryRequest';
    }
}
```

The property to populate is:

- type: Specifies the type of list. There are two possible values:
  - Company: Returns all lakes created by all company users
  - User: only returns the lakes created by the user (identified by the userId property of the request) 

**Sample Request**

```
{
   "guid": "10187207-ba5f-4095-a018-9248ac32048a", 
   "version": 1,   
   "domain" : "CognitiveApp", 
   "companyId" : 1234,
   "userId": "28f26030-17ed-4fcf-a71c-e30bca7ed966",
   "typeName": "LakeListQueryRequest",
   "type": "Company"
}
```

**Sample Response**

```
{
   "guid":"10187207-ba5f-4095-a018-9248ac32048a",
   "version":0,
   "result":{
      "results": 
        [
            {
                "id": "0CD27AB00B3F7096F1176971E7E3A69C",
                "companyId": 1234,
                "userId": "f13c654e-3b54-4e18-baf0-410196311b98",
                "userEmailAddress": "joh.doe@somecompany.com",
                "userFirstName": "John",
                "userLastName": "Doe",
                "lakeName": "Information Technology",
                "lastUpdated": "2020-09-14T11:51:17.302076Z"
            },
            {
                "id": "422B19C9E3290E7B8687DADF543A644F",
                "companyId": 1234,
                "userId": "279b46a5-0bfd-4900-bc39-6b5c3e5fd876",
                "userEmailAddress": "alice.jones@somecompany.com",
                "userFirstName": "Alice",
                "userLastName": "Jones",
                "lakeName": "Civil Engineering",
                "lastUpdated": "2020-09-14T11:21:32.1529718Z"
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
   "companyId" : 1234,
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

### Delete Lake

To delete a lake of resumes, use the following request:

```
export class DeletePoolRequest extends BaseRequest {
    poolId: string;

    constructor() {
        super();
        this.typeName = 'PoolSoftDeleteRequest';
    }
}
```

Note that the pool is soft deleted.

The following property should be populated:

- poolId: Unique ID of the resume lake to delete

**Sample Request**

```
{
   "guid": "10187207-ba5f-4095-a018-9248ac32048a", 
   "version": 1,   
   "domain" : "CognitiveApp", 
   "companyId" : 1234,
   "userId": "28f26030-17ed-4fcf-a71c-e30bca7ed9663",
   "typeName": "PoolSoftDeleteRequest",
   "poolId": "7F6B254D9AC32F27E6A748A66EBDB1B9"
}
```

**Sample Response**

```
{
   "guid":"10187207-ba5f-4095-a018-9248ac32048a",
   "version":0,
   "result":{
      "results":true,
      "code":1,
      "details":[
         
      ],
      "technicalDetails":[
         
      ]
   }
}
```

### Rename Lake

To rename a lake of resumes, use the following request:

```
export class RenamePoolRequest extends BaseRequest {
    poolId: string;
    poolName: string;

    constructor() {
        super();
        this.typeName = 'PoolNameUpdateRequest';
    }
}
```

Properties to populate are:

- poolId: ID of the resume lake to rename
- poolName: The new name of the resume lake 

**Sample Request**

```
{
   "guid": "cec320a9-62e2-6f7e-66c7-b72a92e6a5ea", 
   "version": 1,   
   "domain" : "CognitiveApp", 
   "companyId" : 1234,
   "userId": "28f26030-17ed-4fcf-a71c-e30bca7ed966",
   "typeName": "PoolNameUpdateRequest",
   "poolId": "422B19C9E3290E7B8687DADF543A644F",
   "poolName": "IT"
}
```

**Sample Response**

```
{
   "guid":"cec320a9-62e2-6f7e-66c7-b72a92e6a5ea",
   "version":0,
   "result":{
      "results":true,
      "code":1,
      "details":[
         
      ],
      "technicalDetails":[
         
      ]
   }
}
```

## Job Description

### Get Job Descriptions

To get a list of job descriptions associated with a lake of resumes, use the following request:

```
export class AcquireJobDescriptionByPoolId extends BaseRequest {
    public poolId: string;

    constructor() {
        super();
        this.typeName = `JobDescriptionQueryByCompanyIdAndPoolIdRequest`;
    }
}
```

Property to populate:
- poolId: ID of the lake that the job description is associated with

**Sample Request**

```
{
   "guid": "4c756a58-2c40-53cc-c780-4d1beeed1867", 
   "version": 1,   
   "domain" : "CognitiveApp", 
   "companyId" : 2034,
   "userId": "d3cc45ec-3df6-410c-aac4-b58042f35029",
   "typeName": "JobDescriptionQueryByCompanyIdAndPoolIdRequest",
   "poolId": "422B19C9E3290E7B8687DADF543A644F"
}
```

**Sample Response**

```
{
   "guid":"4c756a58-2c40-53cc-c780-4d1beeed1867",
   "version":0,
   "result":{
      "results": {
         "jobDescriptions": [
            {
               "createdDate": "January 24, 2021",
               "title": "Information Technology",
               "content": "Set up workstations and peripheral devices. Install and configure software. Maintain networks",
               "poolId": "422B19C9E3290E7B8687DADF543A644F",
               "userId": "d3cc45ec-3df6-410c-aac4-b58042f35029",
               "keywords": null,
               "isActive": true,
               "partitionKey": "2034",
               "rowKey": "49FB384DEB86245627634BF94F2622BF",
               "timestamp": "2021-01-24T17:39:49.0586005-05:00",
               "eTag": "W/\"datetime'2021-01-24T22%3A39%3A49.0586005Z'\""
            },
            {
               "createdDate": "May 28, 2020",
               "title": "software developer",
               "content": "    Develop and test software to meet consumers' and clients' needs.\n    Develop upgrades for existing applications.\n    Monitor quality and performance of applications through testing and maintenance.\n    Document all work for future reference.\nmanagement of projects\ncreation of plans for future\n",
               "poolId": "422B19C9E3290E7B8687DADF543A644F",
               "userId": "0030270f-3c9e-4e61-ab56-560d7dc4f3bc",
               "keywords": null,
               "isActive": true,
               "partitionKey": "2034",
               "rowKey": "81459E0A8AC2CAF78AA48149C74EBABA",
               "timestamp": "2020-05-27T21:11:26.73065-04:00",
               "eTag": "W/\"datetime'2020-05-28T01%3A11%3A26.73065Z'\""
            }
         ],
         "poolName": "IT",
         "resumeCount": 637
      },
      "code":1,
      "details":[
         
      ],
      "technicalDetails":[
         
      ]
   }
}
```

### Create Job Description

To create a new job description in a lake of resumes, use the following request:

```
export class CreateJobDescriptionRequest extends BaseRequest {
    public model: CreateJobDescriptionModel;

    constructor() {
        super();
        this.typeName = 'JobDescriptionSaveRequest';
        this.model = new CreateJobDescriptionModel();
    }
}
```

The model property is of type CreateJobDescriptionModel which is defined as:

```
export class CreateJobDescriptionModel {
    public title: string;
    public content: string;
    public poolId: string;
}
```

Properties of the model to populate are:

- title: The title of the job description
- content: The job description details which defines job requirements
- poolId: ID of the lake to associate this job description with 

**Sample Request**

```
{
   "guid": "71bfc8d8-55ba-41ec-9286-f8a9e38528a8", 
   "version": 1,   
   "domain" : "CognitiveApp", 
   "companyId" : 1234,
   "userId": "28f26030-17ed-4fcf-a71c-e30bca7ed966",
   "typeName": "JobDescriptionSaveRequest",
   "model": {
      "title": "Information Technology",
      "content": "Set up workstations and peripheral devices. Install and configure software. Maintain networks",
      "poolId": "422B19C9E3290E7B8687DADF543A644F"
   }
}
```

**Sample Response**

```
{
   "guid":"71bfc8d8-55ba-41ec-9286-f8a9e38528a8",
   "version":0,
   "result":{
      "results":true,
      "code":1,
      "details":[
         
      ],
      "technicalDetails":[
         
      ]
   }
}
```

### Update Job Description

To update an existing job description associated with a lake of resumes, use the following request:

```
export class UpdateJobDescriptionRequest extends CreateJobDescriptionRequest {
    constructor() {
        super();
        this.typeName = 'JobDescriptionUpdateRequest';
    }
}
```

The model property is of type CreateJobDescriptionModel which is defined as:

```
export class CreateJobDescriptionModel {
    public title: string;
    public content: string;
    public poolId: string;
}
```

Properties of the model to populate are:

- title: The new title of the job description
- content: The new job description details which defines job requirements
- poolId: ID of the lake associated with this job description 

**Sample Request**

```
{
   "guid": "1b19039e-544e-4420-86c2-9210a2cbc3b7", 
   "version": 1,   
   "domain" : "CognitiveApp", 
   "companyId" : 1234,
   "userId": "28f26030-17ed-4fcf-a71c-e30bca7ed966",
   "typeName": "JobDescriptionSaveRequest",
   "model": {
      "title": "Information Technology",
      "content": "Set up workstations and peripheral devices. Install and configure software. Maintain networks",
      "poolId": "422B19C9E3290E7B8687DADF543A644F"
   }
}
```

**Sample Response**

```
{
   "guid":"1b19039e-544e-4420-86c2-9210a2cbc3b7",
   "version":0,
   "result":{
      "results":true,
      "code":1,
      "details":[
         
      ],
      "technicalDetails":[
         
      ]
   }
}
```

### Delete Job Description

(Issue #106 will address a bug with this request - If no interview is associated with this request, it fails)

To delete an existing job description associated with a lake of resumes, use the following request:

```
export class DeleteJobDescriptionRequest extends BaseRequest {
  rowKey: string;
  constructor() {
    super();
    this.typeName = 'JobDescriptionDeleteRequest';
  }
}
```

Property to populate:

- rowKey: ID of the job description to delete

**Sample Request**

```
{
   "guid": "b2845afa-65ec-fa46-f3dc-392ced93f63b", 
   "version": 1,   
   "domain" : "CognitiveApp", 
   "companyId" : 1234,
   "userId": "28f26030-17ed-4fcf-a71c-e30bca7ed966",
   "typeName": "JobDescriptionDeleteRequest",
   "rowKey": "E66B7B3AE1D38066C49DD82B5C9444BE"
}
```

**Sample Response**

```
{
   "guid":"620bd24b-c899-edf8-07ef-a2cfaeff06d3",
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

## Interview

### Get Interviews

To get a list of interviews associated with a job description, use the following request:

```
export class GetInterviewsRequest extends BaseRequest {
    jobDescriptionId: string;

    constructor() {
        super();
        this.typeName = 'InterviewsQueryRequest';
    }
}
```

Property to populate:

- jobDescriptionId: ID of the job description to return interviews for

**Sample Request**

```
{
   "guid": "da2f4ce4-5c1b-f0a4-e4d5-2ccf6fcdce6b", 
   "version": 1,   
   "domain" : "CognitiveApp", 
   "companyId" : 2034,
   "userId": "d3cc45ec-3df6-410c-aac4-b58042f35029",
   "typeName": "InterviewsQueryRequest",
   "jobDescriptionId": "49FB384DEB86245627634BF94F2622BF"
}
```

**Sample Response**

```
{
   "guid":"620bd24b-c899-edf8-07ef-a2cfaeff06d3",
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
