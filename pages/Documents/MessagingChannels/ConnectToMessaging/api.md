---
pagename: API
redirect_from:
  - ConnectToMessaging.html
Keywords:
sitesection: Documents
categoryname: "Messaging Channels"
documentname: Connect To Messaging
permalink: connect-to-messaging-api.html
indicator: messaging
---

### Introduction

Connect To Messaging (C2M) is a product offering from LivePerson allowing brands to offer consumers an option to deflect to messaging when they call into their IVR. C2M API serves as an intermediary between the brand’s IVR System and LivePerson Conversational Cloud, ensuring that the consumer is invited to join a conversation with an agent via eligible messaging channels. Once a consumer responds to a message from that channel, C2M ensures that the conversation is routed to an agent of the appropriate skill specified by the brands.


### Getting Started

1. Onboarding to C2M is a mandatory process before running APIs.
2. Brand’s system should integrate with two C2M API endpoints, which are <strong><i>Eligibility</i></strong> and <strong><i>Invite</i></strong>. 
  * <strong><i>Eligibility:</i></strong> Brands call this endpoint to check whether a consumer is reachable via a messaging channel.
  * <strong><i>Invite:</i></strong> Brands call this endpoint to send a messaging invitation to transfer the customer from IVR to one of their supported channels.


### API Specifications
## API Domain

C2M is deployed in three regions. **North America**, **EMEA**(Europe, Middle East and Africa), **APAC**(Asia Pacific). Use the domain api to identify the zone of C2M api which is to be used for an account.

### Eligibility API

Click [**Eligibility**](https://connect-to-messaging.dev.fs.liveperson.com/api/api-docs/?api=c2m#/default/post_account__accountId__eligibility) to go through API spec and use example here to get started.

| Method | URI  |
| :--- | :--- |
| POST | https://{domain}/api/account/{accountId}/eligibility?v={version}

**Path/Query Parameters**

| Name  | Description | Value/Example |
| :--- | :--- | :--- |
| domain   | see API Domain | va.connect-to-messaging.liveperson.net or lo.connect-to-messaging.liveperson.net or sy.connect-to-messaging.liveperson.net |
| accountId | LivePerson site ID | 12345678 |
| version | API Version | 2.0 |

**Request Headers**

| Header | Description | Value/Example |
| :--- | :--- | :--- |
| Content-Type | Used to indicate the media type of the resource | application/json |
| Authorization | Use OAuth 1.0 credentials or get [AppJWT](https://developers.liveperson.com/connector-api-send-api-authorization-and-authentication.html#get-appjwt) | OAuth 1.0 or Bearer «APP_JWT» |

**Request Body Parameters**

| Name | Datatype | Required | Definition |
| :--- | :--- | :--- |:--- |
| skill | string | yes | Engagement skill |
| consumerPhoneNumber | string | yes | Consumer’s phone number(E.164 format with leading "+") |
| handoffId | string | yes | C2M handoff Id |
| sde | array | no | Array of ctmrinfo and/or personal SDEs |
| templateVariables | object | no | Key-value pairs of variables for the template |
| ivrNumber | string | no | The ivrNumber that brands want to use. Some brands have more than 1 ivrNumber and this field clears the ambiguity. |

**Request Body Example - JSON Payload**

```json
{
    "consumerPhoneNumber": "+12061234567",
    "handoffId": "H123456789",
    "templateVariables": {
        "1": "test"
    },
    "skill": "support"
}

```

**Response Body Parameters**

| HTTP Status | Name | Datatype | Required | Definition |
| :--- | :--- | :--- |:--- | :--- |
| 200 | availableChannels | array | true | list of channels that business can send messages from, can be empty |
|  | recommendedChannelName | string | true | recommended channel for sending a message based on channel priorities, can be empty |
|  | eligible | boolean | true | true if consumer is eligible for messaging, otherwise false |
|  | callId | string<<uuid v4>> | true | the uuid associated with this call |
|  | recommendedChannelName | string | true | recommended channel for sending a message based on channel priorities, can be empty |
| 4xx/5xx | errorCode | number | true | C2M API specific error code, not same as the HTTP status code |
|  | errorMessage | string | true | Error message description |

**Response Example**

```json
{
    "availableChannels": [
        "sms","wa" 
    ],
    "recommendedChannelName": "sms",
    "eligible": true,
    "callId": "b52403dc-b140-45cc-a9ca-d749a39b1b56"
}

```

### Invite API

Click [**Invite**](https://connect-to-messaging.dev.fs.liveperson.com/api/api-docs/?api=c2m#/default/post_account__accountId__invite) to go through API spec and use example here to get started.

| Method | URI  |
| :--- | :--- |
| POST | https://{domain}/api/account/{accountId}/invite?v={version}

**Path/Query Parameters**

| Name  | Description | Value/Example |
| :--- | :--- | :--- |
| domain   | see API Domain | va.connect-to-messaging.liveperson.net or lo.connect-to-messaging.liveperson.net or sy.connect-to-messaging.liveperson.net |
| accountId | LivePerson site ID | 12345678 |
| version | API Version | 2.0 |

**Request Headers**

| Header | Description | Value/Example |
| :--- | :--- | :--- |
| Content-Type | Used to indicate the media type of the resource | application/json |
| Authorization | Use OAuth 1.0 credentials or get [AppJWT](https://developers.liveperson.com/connector-api-send-api-authorization-and-authentication.html#get-appjwt) | OAuth 1.0 or Bearer «APP_JWT» |

**Request Body Parameters**

| Name | Datatype | Required | Definition |
| :--- | :--- | :--- |:--- |
| callId | string<<uuid v4> | yes | Correlates to eligibility response |
| overrideChannel | string | no | Override Channel to send the message through. 'sms' and 'twilio' are interpreted as the same thing. |
| overrideMessage | string | no | Override the message sent through SMS only. The maximum length is set to 1600 as it is the maximum limit set by Twilio. |
| overrideSkill | string | no | Overrides the current skill which will be used in routing the messages. If the new skill is not present in handoff, an error will be sent. |

**Request Body Example - JSON Payload**

```json
{
    "callId":"ec88bd52-3d1e-40a7-a2fc-95565a528258"
}
```

**Response Body Parameters**

| HTTP Status | Name | Datatype | Required | Definition |
| :--- | :--- | :--- |:--- | :--- |
| 200 | callId | string | true | the uuid associated with this call |
| 4xx/5xx | errorCode | number | true | C2M API specific error code, not same as the HTTP status code |
|  | errorMessage | string | true | Error message description |

**Response Example**

```json
{
    "callId":"ec88bd52-3d1e-40a7-a2fc-95565a528258"
}
```

### Common Error Responses

```json
{  
  "errorMessage":"Not Found",
  "errorCode":99
}
```

| HTTP Status | Error Code | Error Message | 
| :--- | :--- | :--- |
| 400 | 1000 | Invalid request |
| 400 | 1001 | Invalid customerPhoneNumber |
| 400 | 1002 | Invalid version |
| 400 | 1200 | No internal user is available |
| 400 | 1201 | No internal app is available |
| 400 | 1300 | No engagement found for skill <<skill>> |
| 400 | 1400 | Message cannot be sent |
| 400 | 1500 | No handoff is available |
| 400 | 1501 | No handoff channel is available |
| 400 | 1600 | No setting is available |
| 400 | 1900 | Overridden channel is not available |
| 400 | 1901 | No engagement provided for the overridden skill |
| 401 | 1100 | Invalid Bearer token |
| 403 | 1101 | Not enough privilege to perform this operation |
| 404 | 1004 | Not Found |
| 405 | 1005 | Method Not Allowed |
| 415 | 1015 | Unsupported Media Type |
| 429 | 1029 | Rate limit hit |
| 500 | 5000 - 7000 | Internal Server Error |


### Frequently Asked Questions

<strong>What is the rate limit for the API?</strong>
The current rate limit is 30 transaction per second per brand. 

<strong>What is the recommended action from brands for 429 responses?</strong>
We recommend a request be retried (3 attempts with exponential retry with delay of 5 sec) when witnessing 429 status code.

<strong>Which channels are supported as of now?</strong>
C2M supports SMS-Twilio and WA channels.

<strong>Is there a throughput limitation for the data that gets passed from Twilio to LP? For example, if brand sends 100 Twilio msgs/sec (their max throughput), then can the data flow through to LP at the same rate?</strong>
- C2M does not have any limitations on the message size while sending messages to twilio or other channels. However a large message may translate to more than one message when the recipient receives it.
- Example: A message of more than 140 characters will be divided into two messages and sent to recipients.
 
<strong>What is the lifespan of the app JWT? When we do need to get a new JWT, do we have to first make the call to LivePerson Domain API in order to get the sentinel service domain, or is that domain consistent enough that we can hard code that in?</strong>
- An APP JWT expiration time is 1 hour from the time it is created. To get an app JWT from sentinel API, a call to domain api has to be made to get the sentinel api domain. This domain can be cached for some duration. We expect the domain to change in very rare cases. It’s still recommended that cache duration should not be more than 1 day.

- When using app JWT to call C2M API, a response below indicates the jwt is expired and new app jwt to be obtained from sentinel api.
	```json
	{
		"code": 0,
		"message": "jwt expired"
	}
	```

<strong>Do we need any other JWT other than APP JWT e.g. Consumer JWT?</strong>
C2M service does not create or consume consumer JWT or other JWT except APP JWT. C2M API consumes AppJWT created from provided clientId and Secret for authentication.

<strong>What should the authentication header look like, is the bearer token the only thing required even in production usage? Do we need to include our ConsumerKey/Secret or our AccessToken/Secret that we use in the 1.0 API at all, or any other information?</strong>
App Jwt will be consumed as Bearer Token. No other key, secret or token will be consumed by C2M Messaging api.

<strong>How does C2M 2.0 api provide a report?</strong>
We will have the report API, so stay tuned. 

<strong>Of the error cases described above, which of those errors should we consider "retry-able"? For example, a bad request due to a missing field is not retry-able because it will just always fail, but a case where one of the downstream services was temporarily unavailable could warrant a retry.</strong>
C2M Messaging service has retry mechanism internally on dependent services to reduce failures due to transient errors.

<strong>What’s the lookback period?</strong>
- Lookback period is how long will LP services maintain context (like campaign info, skill etc) for a reply of a message that is sent to the recipient/consumer.  
- Lookback period can be pre-configured up to 30 days. Current maximum lookback period is 30 days from when messages are sent using C2M API. Example: When a message is sent to consumer using C2M API and if consumer replies within 30 days from when message was sent, the response will be redirected to LE agent according to specified skill. A response after 30 days will not be treated as a conversation. Please note, if a consumer has an existing active conversation with a brand in any channel, the outbound message won’t be delivered.

<strong>How do we know which field is optional or required?</strong>
Refer to each API's <strong>Request Body Parameters</strong> or [swagger](https://connect-to-messaging.dev.fs.liveperson.com/api/api-docs/?api=c2m).

<strong>What's the restriction on request body parameters?</strong>

| Field Name | Limitation |
| :--- | :--- |
| consumerPhoneNumber | 15 char max length |
| skill | 255 char max length |
| overrideMessage | 1600 char max length |
| handoffId | 16 char max length |

