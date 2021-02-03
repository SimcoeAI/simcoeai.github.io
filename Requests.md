# Typescript Library of Gateway Requests

## Table of Contents

[Base Request](#base-request)

[User Management](#user-management)

[Lake Management](#lake-management)

[Job Description Management](#job-description-management)

[Interview Management](#interview-management)

[Screening Queue Management](#screening-queue-management)

[Interviewing Candidates](#interviewing-candidates)

## Base Request

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

[Back to top](#)

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

## Job Description Management

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
   "userId": "28f26030-17ed-4fcf-a71c-e30bca7ed966",
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
               "userId": "28f26030-17ed-4fcf-a71c-e30bca7ed966",
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

## Interview Management

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
   "guid": "e7f3fb06-72e8-4c89-800b-4e5d0462759f", 
   "version": 1,   
   "domain" : "CognitiveApp", 
   "companyId" : 2034,
   "userId": "28f26030-17ed-4fcf-a71c-e30bca7ed966",
   "typeName": "InterviewsQueryRequest",
   "jobDescriptionId": "C66E53EB1E56AE79F47958BFEFD7C680"
}
```

**Sample Response**

```
{
   "guid":"e7f3fb06-72e8-4c89-800b-4e5d0462759f",
   "version":0,
   "result":{
      "results": {
         "jobDescriptionId": "C66E53EB1E56AE79F47958BFEFD7C680",
         "interviews": [
            {
               "id": "302ba2ed61f44888a73e152d9b6f3663",
               "title": "Initial interview",
               "validForHours": 24,
               "qnas": [
                  {
                     "id": 1,
                     "question": "First question",
                     "answer": "Expected answer to first question",
                     "weight": 40
                  },
                  {
                     "id": 2,
                     "question": "Second question",
                     "answer": "Expected answer to the second question",
                     "weight": 60
                  }
               ],
               "published": true
            },
            {
               "id": "18b8d943eea84965b89f993ffe603be5",
               "title": "Second interview",
               "validForHours": 48,
               "qnas": [
                  {
                     "id": 1,
                     "question": "First question of the second interview",
                     "answer": "Expected answer to the first question of the second interview",
                     "weight": 75
                  },
                  {
                     "id": 2,
                     "question": "Second question of the second interview",
                     "answer": "Expected answer to the second question of the second interview",
                     "weight": 25
                  }
               ],
               "published": false
            }
         ]
      },
      "code":1,
      "details":[
         
      ],
      "technicalDetails":[
         
      ]
   }
}
```

### Save Interview

To save interviews against a job description, use the following request:

```
export class SaveInterviewsRequest extends BaseRequest {
    public model: InterviewsSet;

    constructor() {
        super();
        this.typeName = 'InterviewSaveRequest';
        this.model = new InterviewsSet();
    }
}
```

This is a create and update request. If 

Property to populate:

- model: an object of type InterviewsSet. See following section.

**InterviewsSet Objects**

```
export class InterviewsSet {
    public jobDescriptionId: string;
    public interviews: Interview[];
}
```

```
export class Interview {
    public id: string;
    public title: string;
    public validForHours: number;
    public qnas: QnA[];
    public published: boolean;
}
```

```
export class QnA {
    public id: number;
    public question: string;
    public answer: string;
    public weight: number;
}
```

**Sample Request**

The following request creates a brand new interview set for a job description (notice the empty id field of the single interview in the request). If any interviews existed before, they will be removed.

```
{
   "guid": "db7939e6-8937-a26a-8915-bb7b2103572a", 
   "version": 1,   
   "domain" : "CognitiveApp", 
   "companyId" : 3095,
   "userId": "1421eac3-2db1-4862-ac31-583521590388",
   "typeName": "InterviewSaveRequest",
   "model": {
      "jobDescriptionId": "C66E53EB1E56AE79F47958BFEFD7C680",
      "interviews": [
         {
            "id": "",
            "title": "the title",
            "validForHours": 12,
            "published": false,
            "qnas": [
               {
                  "id": "1",
                  "question": "some question",
                  "answer": "some answer",
                  "weight": 100
               }
            ]
         }
      ]
   }
}
```

The following request will add a new interview to the job description, and updates the interview added by the above request. Notice the now populate id field of the interview titled "Updated title".

```
{
   "guid": "db7939e6-8937-a26a-8915-bb7b2103572a", 
   "version": 1,   
   "domain" : "CognitiveApp", 
   "companyId" : 3095,
   "userId": "1421eac3-2db1-4862-ac31-583521590388",
   "typeName": "InterviewSaveRequest",
   "model": {
      "jobDescriptionId": "C66E53EB1E56AE79F47958BFEFD7C680",
      "interviews": [
         {
            "id": "37da583e077b4be58142061e9300aa20",
            "title": "Updated title",
            "validForHours": 12,
            "published": false,
            "qnas": [
               {
                  "id": "1",
                  "question": "Updated question",
                  "answer": "Updated answer",
                  "weight": 70
               },
               {
                  "id": "2",
                  "question": "New question",
                  "answer": "New answer",
                  "weight": 30
               }
            ]
         } , {
            "id": "",
            "title": "Second interview",
            "validForHours": 12,
            "published": false,
            "qnas": [
               {
                  "id": "1",
                  "question": "First question",
                  "answer": "First answer",
                  "weight": 100
               }
            ]
         }
      ]
   }
}
```

**Sample Response**

```
{
   "guid":"620bd24b-c899-edf8-07ef-a2cfaeff06d3",
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

## Screening Queue Management

Screening queue is a list of candidates that are being screened for a particular job description. Users send interviews (associated to the job description) to candidates in the queue. Interview results are calculate per each screened candidate in the queue.  

### Get Screening Queue

To get the screening queue associated with a job description, use the following request:

```
export class ScreeningQueueByJobDescriptionRequest extends BaseRequest {
    constructor(
        public jobDescriptionId: string
    ) {
        super();
        this.typeName = "ScreeningQueueByJobDescriptionRequest";
    }
}
```

Property to populate:

- jobDescriptionId: ID of the job description 

**Sample Request**

```
{
   "guid": "3bd9d66d-2905-38f5-6e20-05d0b38410fc", 
   "version": 1,   
   "domain" : "CognitiveApp", 
   "companyId" : 3095,
   "userId": "1421eac3-2db1-4862-ac31-583521590388",
   "typeName": "ScreeningQueueByJobDescriptionRequest",
   "jobDescriptionId": "D77CD30D3D09ECC015765F09BB7DE08E"
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
         {
            "jobDescriptionId": "D77CD30D3D09ECC015765F09BB7DE08E",
            "candidateId": "jane.doe@testing.com",
            "email": "jane.doe@testing.com",
            "fullName": "",
            "phone": "",
            "resumeUrl": "https://storage456897654.blob.core.windows.net/1453ee4e360634d11a431d48b7d3af31/0b1d4e35a81746faabee988b7d1515d1.docx",
            "status": 0
         },
         {
            "jobDescriptionId": "D77CD30D3D09ECC015765F09BB7DE08E",
            "candidateId": "alice.smith@gmail.com",
            "email": "alice.smith@gmail.com",
            "fullName": "",
            "phone": "",
            "resumeUrl": "https://storage456897654.blob.core.windows.net/1453ee4e360634d11a431d48b7d3af31/01fde52fb97d4b36a9acdfbe207e72cf.docx",
            "status": 0
         }
      ],
      "technicalDetails":[
         
      ]
   }
}
```

Currently the "fullName" and "phone" are blank in the response. This will be enhanced in the future.

The "status" comes from the following enum:

```
export enum ScreeningQueueStatuses {
    New = 0,
    Interview = 300,
    Complete = 600
}
```

### Add Candidate to Screening Queue

To add a candidate to a job description's screening queue, use the following request:

```
export class AddCandidateToScreeningQueueRequest extends BaseRequest {
    constructor(
        public jobDescriptionId: string,
        public candidateId: string
    ) {
        super();
        this.typeName = "AddCandidateToScreeningQueueRequest";
    }
}
```

Properties to populate:

- jobDescriptionId: ID of the job description
- candidateId: Id of the candidate

**Sample Request**

```
{
   "guid": "98df1354-8dc7-e805-c9e1-3135da3ece9d", 
   "version": 1,   
   "domain" : "CognitiveApp", 
   "companyId" : 3095,
   "userId": "1421eac3-2db1-4862-ac31-583521590388",
   "typeName": "AddCandidateToScreeningQueueRequest",
   "jobDescriptionId": "D77CD30D3D09ECC015765F09BB7DE08E",
   "candidateId": "jane.doe@somecompany.com"
}
```

**Sample Response**

```
{
   "guid":"620bd24b-c899-edf8-07ef-a2cfaeff06d3",
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

### Remove Candidate from Screening Queue

To remove a candidate from a job description's screening queue, use the following request:

```
export class RemoveCandidateFromScreeningQueueRequest extends BaseRequest {
    constructor(
        public jobDescriptionId: string,
        public candidateId: string
    ) {
        super();
        this.typeName = "RemoveCandidateFromScreeningQueueRequest";
    }
}
```

Properties to populate:

- jobDescriptionId: ID of the job description
- candidateId: Id of the candidate

**Sample Request**

```
{
   "guid": "620bd24b-c899-edf8-07ef-a2cfaeff06d3", 
   "version": 1,   
   "domain" : "CognitiveApp", 
   "companyId" : 3095,
   "userId": "1421eac3-2db1-4862-ac31-583521590388",
   "typeName": "RemoveCandidateFromScreeningQueueRequest",
   "jobDescriptionId": "D77CD30D3D09ECC015765F09BB7DE08E",
   "candidateId": "jane.doe@somecompany.com"
}
```

**Sample Response**

```
{
   "guid":"620bd24b-c899-edf8-07ef-a2cfaeff06d3",
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

## Interviewing Candidates

### Interview Invitation

To send an invitation email to a candidate in a screening queue to take an online interview, use the following request:

```
export class InterviewInvitationRequest extends BaseRequest {
    constructor(
        public jobDescriptionId: string,
        public interviewId: string,
        public candidateId: string
    ) {
        super();
        this.typeName = "InterviewInvitationRequest";
    }
}
```

Properties to populate:

- jobDescriptionId: ID of the job description
- interviewId: ID of the interview to invite to
- candidateId: ID of the candidate

The interview invitation will contain a link with a token that will redirect the user to the interview page.

**Sample Request**

```
{
   "guid": "f7b9b4c8-e163-9fce-d1a9-266b46562150", 
   "version": 1,   
   "domain" : "CognitiveApp", 
   "companyId" : 3095,
   "userId": "1421eac3-2db1-4862-ac31-583521590388",
   "typeName": "InterviewInvitationRequest",
   "jobDescriptionId": "62DEE413209A0977168F2B6E2979CDB1",
   "interviewId": "8907baa0255f439184cfd8bd57d9d57b",
   "candidateId": "jane.doe@somecompany.com"
}
```

**Sample Response**

```
{
   "guid":"f7b9b4c8-e163-9fce-d1a9-266b46562150",
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

### Get Candidate Interviews

To get a list of a candidate's interview results, use the following request:

```
export class CandidateInterviewsRequest extends BaseRequest {
    constructor(
        public jobDescriptionId: string,
        public candidateId: string
    ) {
        super();
        this.typeName = "CandidateInterviewsRequest";
    }
}
```

Properties to populate:

- jobDescriptionId: ID of the job description
- candidateId: ID of the candidate

**Sample Request**

```
{
   "guid": "fc75f378-8696-17b0-e27d-5174719db988", 
   "version": 1,   
   "domain" : "CognitiveApp", 
   "companyId" : 3095,
   "userId": "1421eac3-2db1-4862-ac31-583521590388",
   "typeName": "CandidateInterviewsRequest",
   "jobDescriptionId": "62DEE413209A0977168F2B6E2979CDB1",
   "candidateId": "jane.doe@somecompany.com"
}
```

**Sample Response**

In this sample, there are two interview associated with the job description in the request:

```
{
   "guid":"fc75f378-8696-17b0-e27d-5174719db988",
   "version":0,
   "result":{
      "results": [
         {
            "candidateId": "jane.doe@somecompany.com",
            "jobDescriptionId": "62DEE413209A0977168F2B6E2979CDB1",
            "interviewId": "8907baa0255f439184cfd8bd57d9d57b",
            "status": 500,
            "answers": [
               {"questionId":1,"answer":"some answer to question 1"},
               {"questionId":2,"answer":"some answer to question 2"}
            ],
            "totalScore": 47.595058602760986
         }, {
            "candidateId": "jane.doe@somecompany.com",
            "jobDescriptionId": "62DEE413209A0977168F2B6E2979CDB1",
            "interviewId": "8907baa0255f439184cfd8bd57d9d57b",
            "status": 0,
            "answers": [],
            "totalScore": 0
         }
      ]
      ,
      "code":1,
      "details":[
         
      ],
      "technicalDetails":[
         
      ]
   }
}
```

The status field is of following enum type:

```
export enum CandidateInterviewStatus {
    NotSent = -1,
    Pending = 0,
    Complete = 500
}
```

The totalScore field is calculated based on the weight of each interview question and the given answer, and is between 0 and 100. The higher the score the closer the candidate's answer to question's expected answer.

### TODO: how to work with Interview endpoint

## Master Formula Management

### TODO: MasterFormulaQueryByCompanyIdRequest

### TODO: MasterFormulaSaveRequest

## Resume Upload and Processing

### TODO: SasTokenRequest

### TODO: ProcessResumeBlobRequest