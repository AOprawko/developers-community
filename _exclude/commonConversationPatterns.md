---
pagename: Common Conversation Patterns
redirect_from:
Keywords:
sitesection: Documents
categoryname: "Conversational AI"
documentname: Conversation Builder
subfoldername: Best Practices
permalink: conversation-builder-best-practices-common-conversation-patterns.html
indicator: both
---

### How does Conversation Builder jump between different dialogs?

#### Stack-Based Dialog Flow

Every time the user enters a new utterance in the conversation, this has the chance of matching a new intent and "jumping" you out of one dialog and into another one for the newly matched intent.

**Note**: Depending on if the user’s utterance is in response to 

A) open-ended statement 

<img class="fancyimage" width="500" src="img/ConvoBuilder/bestPractices/tips_image_24.png">

… or ...

B) specific question type 

<img class="fancyimage" width="500" src="img/ConvoBuilder/bestPractices/tips_image_25.png">

will affect your ability to restrict, redirect or prevent this behaviour.

Let’s first look at the scenario A):

In this situation, the bot has presented a text statement: "please type your question" variation and awaits the user’s input.

Because this is not being captured in the response to a specific (pink) question type, no response conditions can be applied to what the user types - **_whatever the utterance is, will be processed by the NLU engine to try and find a matching intent and dialog flow._**

Therefore if a new matching dialog flow is detected, this will be "pushed" onto the top of a virtual “stack” of dialogs in progress…

Take the following bot automation where there are 2 defined intents - balance and billing questions.

1. In the first step, the user types an utterance ("balance") to trigger the first intent and dialog...

    <img class="fancyimage" width="500" src="img/ConvoBuilder/bestPractices/tips_image_26.png">

    <img class="fancyimage" width="300" src="img/ConvoBuilder/bestPractices/tips_image_27.png">

    *This represents the "stack" of dialogs being tracked by CB. Newest matched intent / dialogs are on the top - removed after completion of the final step*

2. Mid Balance Enquiry dialog, the user types "billing question" utterance This matches the Billing Question intent and dialog, which is immediately **pushed to the top of the stack.** The user then sees the steps for that intent...

    <img class="fancyimage" width="500" src="img/ConvoBuilder/bestPractices/tips_image_28.png">

3. Once the billing question dialog flow ends, the user is returned to the next dialog still on the stack in this case : Balance... 

    **Note**: The last interaction to be presented before leaving is shown again - in this case the "provide email address" question.

    <img class="fancyimage" width="500" src="img/ConvoBuilder/bestPractices/tips_image_29.png">

Note: This behaviour allows for many dialogs to stack and then pop as they are completed. In the following example, we start with Balance, then trigger Billing Question and then a specific answer in Billing Question routes us to "Sub Billing" Dialog. After that completes it is removed from the stack, then Billing Question is removed and finally we return to the Balance dialog and see the same interaction before we branched out.

<img class="fancyimage" width="300" src="img/ConvoBuilder/bestPractices/tips_image_30.png"><img class="fancyimage" width="300" src="img/ConvoBuilder/bestPractices/tips_image_31.png">

#### How to Prevent Stack-Based Flow

The following strategies will prevent this stack-based flow:

1. If one of the dialogs added to the stack [ends the conversation](conversation-builder-conversation-builder-keyword-triggers.html#close-conversation) - this should prevent the system falling back to the dialog underneath in the stack.

2. If one of the stacked dialogs contains a question type that uses conditional matching to route to an end conversation interaction.

<!--
3. (TBC) an open question type with no routing conditions that just continues to next interaction, *appears* to block the NLU detection and dialog jumping behaviour - more testing required to confirm 100%
-->

Apart from that, the system will remove any stacked dialogs as they complete and return you to the previous one.

{: .important}
This stack-based flow does **not** apply to a fallback dialog. If you are in the fallback and you jump to another dialog, you will never be returned to fallback after you complete the new dialog.

#### Solution for Integrations

Add the following pre-process code to your escalation / transfer to agent action to prevent the above behaviour

```javascript
botContext.setBotVariable('skipPreviousDialog', 'true',true,false);
```

### Avoid Consumer Interruptions During Bot Sending of Information

How to ignore customer responses while the bot is sending out instructions before the next question is asked?

A common scenario is having the bot present several text interactions of information with small delays between each interaction (to aid accessibility issues). After these interactions have been sent in sequence, the bot asks the user a question to help decide the next course of action.

However, there is nothing stopping the consumer from typing utterances while the bot is still in the process of sending its sequence of text interactions. 

In this situation, an utterance would be processed for any matching intents/patterns to dialogs and potentially "jump" the user into another dialog, fallback or be treated as a response to an upcoming question in the current dialog.

#### Solution

By setting the following Automation [Environment Variables](conversation-builder-best-practices-environment-variables.html) at Bot Automation level, the bot can be set to "ignore" utterances sent by the consumer within a dialog until the next logical branching option is reached - e.g. a question with conditions that allow you jump out to other dialogs.

<table>
<thead>
 <tr>
 <th>Environment Variable Name</th>
 <th>Value</th>
 <th>Description</th>
 </tr>
 </thead><tbody>
 <tr>
 <td>system_handleIntermediateUserMessage</td>
 <td>true</td>
 <td>Enables the behaviour to catch intermediate user messages mid dialog flow</td>
 </tr>
 <tr>
 <td>system_intermediateBotMessage</td>
 <td>(Optional) Message String to respond with if triggered
E.g.
"Please wait...we are still responding to your last message"</td>
 <td>The message the bot will respond with if the feature is triggered by the user sending an utterance.
If this value is not set then the bot will simply not respond with anything should the ignore feature be enabled.</td>
 </tr>
 <tr>
 <td>system_intermediateBotResponseTimeout</td>
 <td>Time Out period in milliseconds
E.g 15000 = 15 seconds</td>
 <td>If for some reason, bot is waiting on Message #1 and the wait is way too long, then it should timeout and move on to Message #2, instead of waiting for message #1 to complete.</td>
 </tr>
 </tbody>
</table>

*If you have an existing set of Automation Variables assigned to your bot already, please **_append_** these variables to that existing set to enable this feature.*

<img class="fancyimage" width="500" src="img/ConvoBuilder/bestPractices/tips_image_32.png">

*Otherwise, create a new automation environment set to store these settings inside and assign them to your bot automation…*

<img class="fancyimage" width="700" src="img/ConvoBuilder/bestPractices/tips_image_33.png">

<img class="fancyimage" width="500" src="img/ConvoBuilder/bestPractices/tips_image_34.png">

<img class="fancyimage" width="700" src="img/ConvoBuilder/bestPractices/tips_image_36.png">

Essentially, you will be "locked" to the current dialog and be shown the `system_intermediateBotMessage` text each time you try and interrupt the bot mid-flow

<img class="fancyimage" width="300" src="img/ConvoBuilder/bestPractices/tips_image_37.png">

### How to keep people inside a single dialog flow

#### Why

Depending on the use case, it might be needed to ensure users do not jump out into other dialogs due to the next utterance, reply matching another intent within the NLU.

At various branching points within certain dialogs, users are asked to answer questions or make choices on how to proceed.

while effort can be made to help them pick certain predefined options, there is always a possibility that users will just type their needs separately and not pick from menus of buttons or multi-choice answers.

Unless these types of input are caught, they could allow the user to jump from the current flow into the next dialog. 

while this may be an OK experience for certain dialogs, others may have specific questions and answers to collect and therefore we need a way to contain them in the current dialog until we have collected answers we need.

#### How

When asking questions (pink) that specifically await user input, this allows the opportunity to define specific "response conditions" that can either allow/deny non-expected input to jump out into other dialogs.

As an example, this Balance dialog has 2 sequential questions: account number and email address. Both have specific conditions defined to validate the input.

6 digits and valid email address respectively.

<img class="fancyimage" width="400" src="img/ConvoBuilder/bestPractices/tips_image_20.png">

However, because no "catch all other" condition has been added, if the user types anything other than valid responses, the utterance is evaluated against NLU for matching intents which could jump dialogs. In the below example, nothing matches the response so the **fallback** dialog is invoked. Because **fallback** does not wait for any input, it immediately drops out of the bottom and returns them to the previous dialog…

<img class="fancyimage" width="400" src="img/ConvoBuilder/bestPractices/tips_image_21.png"><img class="fancyimage" width="400" src="img/ConvoBuilder/bestPractices/tips_image_22.png">

If you wanted to prevent this behaviour, the following condition could be added to each question:

Using the *pattern match* as **the final condition** will catch any other responses to that question which do not match any of the previous conditions defined.

You can use this condition to repeat the same question if you desire or redirect anywhere else within the automation if you desire.

<img class="fancyimage" width="300" src="img/ConvoBuilder/bestPractices/tips_image_23.png">

### Create Reusable "yes" and “no” intents

Rather than having to constantly enter and re-enter patterns inside response conditions  for variations of "yes" or “no”, it is  recommended to build out two specific intents which encompass these.

* Affirmation

* Negative Affirmation

You can wrap all variations that encompass "yes" or “no”. Once done you can create response conditions that match “Response Intents” linked to these and never have to re-enter the same patterns again for these types of questions!

<img class="fancyimage" width="500" src="img/ConvoBuilder/bestPractices/tips_image_3.png"><img class="fancyimage" width="500" src="img/ConvoBuilder/bestPractices/tips_image_4.png">

<img class="fancyimage" width="500" src="img/ConvoBuilder/bestPractices/tips_image_5.png"><img class="fancyimage" width="500" src="img/ConvoBuilder/bestPractices/tips_image_6.png">

### Create "resolve and close" dialog

This will help to avoid having to repeatedly design and build the following sequence of questions within every dialog/intent flow:

```
Did that resolve your question?

* Yes

* No

Yes = send goodbye message and close conversation

No = Ask for user input and match intent etc.
```

#### How 

1. Create a new dialog inside your bot called "Confirm Resolution and Close"

    <img class="fancyimage" width="500" src="img/ConvoBuilder/bestPractices/tips_image_9.png">

2. Setup the following interactions

    <img class="fancyimage" width="500" src="img/ConvoBuilder/bestPractices/tips_image_10.png">

3. Ask for confirmation of resolution

4. Setup Affirmation and Negative Affirmation response intents to capture their answer

    Affirmation goes to your interaction where you ask them to enter the next question. In my example that interaction is named "wait for next utterance"

    <img class="fancyimage" width="500" src="img/ConvoBuilder/bestPractices/tips_image_11.png">

    Negative Affirmation goes to the "happy to help message" which then goes to the [**LP_CLOSECONVERSATION** code to end the conversation.](conversation-builder-conversation-builder-keyword-triggers.html#close-conversation)

5. Wire up all your other existing intents to this one dialog flow when they have reached their logical end and you want to ask for confirmation the problem has been resolved.

In my example bot I have two separate intents: billing and balance.

At the end of both these dialogs, the "next interaction" of the final steps directly links to the new “confirm resolution and close” dialog we just built.

E.g.

<img class="fancyimage" width="500" src="img/ConvoBuilder/bestPractices/tips_image_12.png">

<img class="fancyimage" width="500" src="img/ConvoBuilder/bestPractices/tips_image_13.png">

<img class="fancyimage" width="500" src="img/ConvoBuilder/bestPractices/tips_image_14.png">

#### Example

In my example bot I will start the billing flow…

<img class="fancyimage" width="500" src="img/ConvoBuilder/bestPractices/tips_image_15.png">

I say Yes and then after answer is sent from the billing flow, I am pushed into the "confirm resolution" dialog…

<img class="fancyimage" width="500" src="img/ConvoBuilder/bestPractices/tips_image_16.png">

If I say "yes" I can type my utterance for triggering “balance” intent

<img class="fancyimage" width="500" src="img/ConvoBuilder/bestPractices/tips_image_17.png">

Once I complete balance, I send the consumer back to the confirm resolution dialog…

<img class="fancyimage" width="500" src="img/ConvoBuilder/bestPractices/tips_image_18.png">

**Note**: the red line is added to show the end of balance and below is back in the confirm resolution 

Now I can type another utterance and allow the system to match me against another known intent and repeat the sequence again…

Or say "no" and we close the conversation

<img class="fancyimage" width="500" src="img/ConvoBuilder/bestPractices/tips_image_19.png">