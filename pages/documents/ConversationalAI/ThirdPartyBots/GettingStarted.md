---
pagename: Getting Started
redirect_from:
  - customer-facing-bots-deploying-bots-to-liveengage.html
  - customer-facing-bots-considerations.html
  - customer-facing-bots-deploying-bots-on-live-chat.html
  - customer-facing-bots-deploying-bots-on-messaging.html
  - customer-facing-bots-getting-started.html
  - customer-facing-bots-limitations.html
  - customer-facing-bots-overview.html
  - customer-facing-bots-prerequisites.html
  - bot-connectors-getting-started.html
sitesection: Documents
categoryname: "Conversational AI"
documentname: Third-Party Bots
permalink: third-party-bots-getting-started.html
indicator:
---

### Introduction

External Bot frameworks and Bot builders can be enabled and managed through LiveEngage just like a normal human agent.

Using Third-Party Bots, you can provision a connector for IBM Watson, Google Dialogflow, Amazon Lex, Microsoft Bot
 Framework or leverage LivePerson Functions with the Custom Integrations Option.

{: .important}
If you need to connect a external bot that does not have a pre-built connector, see [this document](bot-connectors-custom-third-party-bots.html) for instructions.

Each connector provides the functionality to

- send/receive text messages

- send [structured content](getting-started-with-rich-messaging-introduction.html)

- transfer the conversation to other skills

- change Time To Response for a messaging conversation

- close a conversation

Some connectors may provide more or less functionality depending on the specifics of the product to which it is connecting.

### Bot Lifecycle

The running Bot can have different states based on the health status of the services it utilizes, such as LivePerson APIs, AI vendors, etc.

#### Offline

The Bot is offline and won't accept any conversations.

#### Online

The Bot is online and will accept and process new conversations.

#### Vendor Interruption

The Bot is online and will accept new conversations, but will directly escalate them to the default transfer skill, because the set up AI Vendor is not reachable/working.

#### Service Interruption

The Bot is in the delayed state and will not accept new conversation or process existing conversations. This state is a result of an interruption within Liveperson APIs/servers.

In this state the bot will try to restart automatically once every minute until the interruption is resolved.


### Limitations

#### Supported customer content

Currently the connectors only support text input from the customer. If the customer sends an image or a file to the Bot, the Bot will replace it with a special identifier so the Bot can handle this special use-case with custom code.

The send identifier is **com.liveperson.bot-connectors.consumer.send-file**

#### Support for different messaging channels and the corresponding rich content

The Bot Connector system is designed to support [all relevant rich content](getting-started-with-rich-messaging-rich-messaging-channel-capabilities.html), since it only forwards the received structured content and metadata to LivePerson's messaging and chat service. We have verified and tested the support for **Web Messaging**, **Facebook** and **Apple Business Chat**. All other channels are not verified, but should work if you send the right structured content for the channel. If you experience any issues, please contact LivePerson support or your account team.


#### Creating and starting Bots

Due to limitations within LiveEngage's permission system, it is not possible for an operator with the Agent or the Agent Manager profiles to create new bots or start bots. However, they are still able to stop, edit and delete existing bots.

If you want to enable creating and starting bots for the Agent and Agent Manager profiles, you need to create a new custom Profile, which will derive its base permissions from the Campaign Manager or Admin profiles. Then, make sure to enable the needed permissions for creating and starting bots only while disabling any other permissions. Afterwards, you will need to assign this new Profile to the Agent/Agent Manager who should be able to start/create bots.

<img style="width:600px" src="img/botconnectordashboard/campaign_manager_bot_permissions.png">

Minimal set of permissions for creating and starting bots for Campaign Manager Profile

<img style="width:600px" src="img/botconnectordashboard/administrator_bot_permissions.png">

Minimal set of permissions for creating and starting bots for Administrator Profile

#### Intent and actions evaluation

Please note that your bot setup should always return an intent or an action as a response. If you return an intent without actions or messages, or return no intent at all, the bot will consider this an error and escalate to the default escalation skill.

### Create Bot User in LiveEngage

1. Add a new user in LiveEngage, choose "Bot" for “User type”. If “User type” is not available, contact your LivePerson account manager to enable the feature.

  <img style="width:600px" src="img/dialogflowversion2/image_0.png">

{:start="2"}  
2. Add login method as "API key" and generate new API key for the new user

   <img style="width:600px" src="img/dialogflowversion2/image_1.png">

{:start="3"}
3. Make sure the user has chat and/or messaging slot > 0 based on the target channel of the bot.

{:start="4"}
4. Set Max No of Live Chats

   - If Chat in the drop down select - Value > 1.

   - If Messaging Max No of Live Chats -> **No Chats and Max No of Messaging Conversations to Custom Setting and enter a value greater than 0**

{:start="5"}
5. Find api key name in bot user profile

   <img style="width:400px" src="img/dialogflowversion2/image_2.png">

---

### Provision a connector 

To access Third-Party Bots, contact your Account Manager to enable the the feature in LiveEngage for your account.

Upon logging in to LiveEngage, you will see the Conversation AI Tab:

<img class="fancyimage" style="width:750px" src="img/botconnectordashboard/conversational_ai_app.png">

Follow the steps below to add a new bot.

1. Click on the "Third-Party Bots" App.

2. Click “+ Add Bot” in the bot table

   <img style="width:800px" src="img/botconnectordashboard/add_new_bot.png">
{:start="3"}
3. Assign agent: Create a new Bot Agent or select an existing one

4. Choose conversation type: Chat or Messaging

Settings for Chat:
<img style="width:900px" src="img/botconnectordashboard/chat_settings.png">

- Time until warning: Set up the time span after which the consumer will get an inactivity warning.

- Warning message: The warning message the chat consumer gets if he reaches the threshold.

- Time until conversation close: Set up the time duration after which the consumer chat conversation will be closed if the customer is inactive

- Close message: The message which the consumer will receive prior to closing the conversation

Settings for Messaging:
<img style="width:900px" src="img/botconnectordashboard/messaging_settings.png">

- No special settings are needed for messaging bots
{:start="5"}
5. Setup Escalation: Skill to transfer to in the event of an error during connection to the AI service

  <img style="width:900px" src="img/botconnectordashboard/error_handling.png">

- Transfer message to Customer: Default escalation message to the consumer in case the bot encounters an error

- Transfer message to Agent: Message to the Agent from the escalating bot which will be provided together with the conversation when it is transferred

- Transfer failure message: Message to the customer in case the escalation to the default escalation skill did not work.

- Transfer to skill: Default escalation skill the bot should escalate to in case of any error.

**Note**: if no other skills are configured, it might be that the bot will escalate the conversation to itself. However in this case only new messages will be processed.

6. Connect to A.I.: Choose an AI engine from a list. Add the configuration of AI. See [Next Steps](#next-steps).

### Next Steps

Move on to the product guides to learn how to connect and configure your specific bot framework/builder.

- [Watson Assistant](bot-connectors-ibm-watson-assistant.html)

- [Dialogflow V1](bot-connectors-google-dialogflow.html)

- [Dialogflow V2](bot-connectors-google-dialogflow-version-2.html)

- [Amazon Lex](bot-connectors-amazon-lex.html)

- [Microsoft Bot Framework](bot-connectors-microsoft-bot-framework.html)

- [Custom Third Party Bots](bot-connectors-custom-third-party-bots.html)
