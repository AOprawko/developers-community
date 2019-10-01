---
pagename: Campaign for messaging Routing
redirect_from:
  - cmp-routing-example.html
sitesection: Documents
categoryname: "Messaging Channels"
documentname: Connector API
subfoldername: Examples
order: 65
indicator: messaging
permalink: connector-api-examples-campaign-for-messaging-routing.html

---

In this example we create a conversation and pass the **Engagement ID** and **Campaign ID** to LiveEngage in order to route the consumer conversation to the desired skill as designed by the Campaign Manager.

### Getting Started

1. **Retrieve your domain**. Use the [LivePerson Domain API](agent-domain-domain-api.html) to retrieve this information by providing the following service name:

	* asyncMessagingEnt

2. [Here are the API terms of use](https://www.liveperson.com/policies/apitou).

### Create new conversation and send campaign information

**Request URI**

| Method | URI  |
| :--- | :--- |
| POST | https://[{domain}](/agent-domain-domain-api.html)/api/account/{accountid}/messaging/consumer/conversation?v=3 |

**Request Headers**

| Header | Description |
| :--- | :--- |
| Authorization | The AppJWT token (see details [here](Create_AppJWT.html)) |
| X-LP-ON-BEHALF | The ConsumerJWS token (see details [here](Create_ConsumerJWS.html)) |

**Example Request Body - JSON Payload**

```json
[  
  {
   "kind": "req",
   "id": "1,",
   "type": "userprofile.SetUserProfile",
   "body": {
     "authenticatedData": {
       "lp_sdes": [{
           "type": "ctmrinfo",
           "info": {
             "socialId": "1234567890",
             "ctype": "vip"
           }
         },
         {
           "type": "personal",
           "personal": {
             "firstname": "John",
             "lastname": "Doe",
             "gender": "MALE"
           }
         }
       ]
     }
   }
 },
   {  
      "kind":"req",
      "id":"1,",
      "type":"cm.ConsumerRequestConversation",
      "body":{  
         "ttrDefName":"NORMAL",
         "campaignInfo":{  
            "campaignId":"2739101812",
            "engagementId":"2739121912"
         },
         "channelType":"MESSAGING",
         "brandId":"{accountid}"
      }
   }
]
```

**Note**:

For more information about campaigns, please [click here](https://www.liveperson.com/services/technical-support/about-campaigns).
