---
pagename: AskMaven API
redirect_from:
Keywords:
sitesection: Documents
categoryname: "Conversational AI"
documentname: Maven
subfoldername: AskMaven
permalink: maven-askmaven-askmaven-api.html
indicator: messaging
---

### What is the AskMaven API

The AskMaven API is a REST API that allows you and your bots, web sites, and apps to call Maven capabilities programmatically. You can use the API to ask Maven for the next best actions (route to skill, KB article, etc) for a concierge bot/app.

At this time, the API only provides interfaces to read and execute policies. Maven will not provide interfaces for other CRUD operations such as creating or updating an existing policy.

### Use Cases

Use the AskMaven API to accomplish the following example workflows:

* Channel Switching from Website or App: 
     * A brand wants to provide different channels based on customer and Intent. E.g. “For an order cancellation intent send     conversation to messaging involving a bot, unless it’s a high value customer, in that case have them talk on the phone to a human agent”
     * They configure this policy in Maven hub 
     * They use AskMaven API to return the which routing decision to make
     * Using the decision that Maven API returns, they provide the right experience to the consumer

 * Get AI Powered Routing Decisions
   * Returns the next action(s) by running all enabled policies in order
   * Inputs: JSON payload of context variables required to run the policies
   * Outputs: List of outcomes by running all enabled policies. Should be list of actions with following info
      * Policy ID
      * Outcome (next action, for example transfer to agent or skill)

Note: This API will run through all enabled policies. The API does not allow a brand to run a specific policy. 

### Typical use of AskMaven from a Bot
This provides a typical implementation of AskMaven API from a bot to handle routing decisions. Please note, that AskMaven may also be used from Apps, Websites, and other entry points in various ways

<img class="fancyimage" style="width:700px" src="img/maven/askmaven2.png">

### Developer Key

To use AskMaven APIs you will need to create and use an API key. To get your unique key:

4. Login to Maven with your LiveEngage credentials and then navigate to **Developer Key**.

5. Copy and paste the key you see in the experience and use it in your API headers. 

6. To generate a new key, click on the **Regenerate Key** button. Please note that this will invalidate the previous key. The key is shared for all Maven APIs and therefore you will have to use the new key wherever the APIs are being called.  

<img class="fancyimage" width="600" src="img/maven/image_47.png">

5. Sample Policies

### Methods

Every API call to the AskMaven service requires the following Auth Headers to be accepted

`Content-Type : application/json`

`maven-api-key : <INSERT YOUR API KEY HERE>`

#### Query Parameters

<table>
    <thead>
        <tr>
            <th>Name</th>
            <th>Value</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>conversationId</td>
            <td>string</td>
            <td>Optional - The conversation id of the current conversation</td>
        </tr>
        <tr>
            <td>groupId</td>
            <td>string</td>
            <td>Optional - The group id associated with the Context API custom namespace variable call to set values</td>
        </tr>
        <tr>
            <td>conversationId</td>
            <td>string</td>
            <td>Optional - The conversation id of the current conversation</td>
        </tr>
    </tbody>
</table>

#### Get Next Actions

<table>
    <thead>
        <tr>
            <th>Method</th>
            <th>Path</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>GET</td>
            <td>/v1/account/{accountId}/next-actions</td>
            <td>Get maven routing decision based on maven configured policies</td>
        </tr>
    </tbody>
</table>

Response Body: 

```bash
{
    nextActionId: ‘UUID’,  // some uuid 
rule: {
"id": "12345",
"name": "This is VIP rule"
actions: [  

{
    "type": "TRANSFER_TO_AGENT",
    "payload": { agentId: ‘g23hasd234’, fallbackSkillId: ‘12345’ }
  },
  {
    "type": "SEND_MESSAGE",
    "payload": { text: ‘hello from maven” }
  }
]
},

noMatchReason: “NO_MATCHED_RULES” // only added if no rules are matches, rule will be null
noMatchReason: “NO_POLICIES_ENABLED” 
}
```

