---
pagename: Tips & Tricks
redirect_from:
Keywords:
sitesection: Documents
categoryname: "Conversational AI"
documentname: Conversation Builder
subfoldername: Best Practices
permalink: conversation-builder-best-practices-tips-tricks.html
indicator: both
---

This document is a collection of useful information, tips and tricks accumulated over many months of using conversation builder.

There is no set order to the document - each heading is meant to be a single defined topic taken on its own.

### Pre-Launch "Chaos Monkey" Testing

random, unfamiliar user testing the unhappy paths

* Be as disruptive as possible to try and break the flows you have already built.

* **Find the gaps / cracks in your bot now -** _not when it’s in front of all your customers._

* From within any/all intent dialogs deliberate enter gibberish  messages and see how the fallback plays out from this trigger?

    * are you catching answers that are not expected question responses using * patterns and the repeat question approach we have built?

Consider the following scenario:

- If someone unexpectedly jumps to the fallback mid-dialog flow, do you want to try and recognise this when they hit the fallback and offer them a way back to the previous intent dialog?

    - ***Note*** : doing this would require more conditional logic and some code to track and create this functionality.

- maybe we set a "current_consumer_intent" variable at the top of each dialog?

    - eg "current_consumer_intent" = “CARD_ACTIVATION”

- then if they suddenly enter unrecognised utterances mid-dialog which trigger fallback, your fallback dialog can check this variable before decided what contextual message to show?

*if* **current_consumer_intent** has a value 

*then*  
        
    - jump to fallback contextual message and skip known intents menu

        - "I see you were just in “card activation" but I didn’t understand your last message. Would you like to go back to where you were? Or speak to an agent?”

            - "go back to CARD ACTIVATION"

            - "talk to an agent"

### Post-Launch Optimization

Anticipate how you will react to unmatched intents on day 1

* pre-build a dialog flow which recognises the customer wants help with a popular intent / question that the bot cannot currently handle

* build out the dialog steps where you explain this and gracefully bring in an agent via transfer

* then during the retraining downtime of the bot on day 1, you just need to create a new intent from the unmatched intents data, and connect it to this dialog.

* you can then quickly put this live for round 2 of the bot on day 1 and quickly demonstrate how quickly you can *learn* from what customers are asking you.

* extending this concept, you could also pre-build some dialogs which refer to incidents around specific service outages that might occur whilst the BOT is live - again hooking them up to common phrases people might ask in the event of an outage.

* You can then quickly enable these dialogs on the fly should an incident occur as a way to help deflect / contain people who flood the channel during outages / issues.

* Again, this can all be pre-built and tested ahead of the go-live based on the knowledge you have on how you respond / communicate service outages when customers come into agents at the moment.

### Avoid mismatched intents for unhandled use cases

If there are known phrases / patterns for sensitive customer intents that you are not handling in the bot for launch, the recommendation is to create placeholder dialogs that target these specific patterns and immediately transfer to a human agent.

This avoids the bot trying and failing to respond with the appropriate tone for something the customer says which you have not anticipated.

E.g.

"My partner has passed away" relates to bereavement and should be transferred immediately. 

Create a dialog with the pattern for the User Says interaction of  "*passed away*" and other variations and have this immediately transfer to agent.

This prevents hitting the fallback or mismatched intent with another dialog.



### Agent to Bot Transfer Scenarios

#### Use case: Offer consistent greeting regardless of last utterance prior to transfer to the bot

Agent hands-off conversation to a bot to perform some automated process.

#### Why is this a problem?

When the conversation hits the bot, it will attempt to match the last message in the conversation with a given intent.

However, this cannot be guaranteed to always be consistent. Whilst the agent may send a predefined message telling the consumer they are about to be transferred to a bot, there is nothing stopping the consumer responding into the conversation with an acknowledgement such as "OK" before the agent completes the transfer in LE.

In this situation, the first message the bot receives would be "OK" utterance - which is likely to trigger the fallback dialog and we are already off to a poor start customer experience and conversation wise.

#### Solution

[How to respond to transfer into a bot with custom Fallback dialog routing](https://docs.google.com/document/d/1SjzCsHOhLK6QRbZRsLOCveQpG53_oESlpkhBsuQqPLg/edit?usp=sharing)

This document steps through how to create logic in your fallback and welcome/greeting dialogs to always offer a consistent greeting when the bot receives a transfer from human agent when the last utterance does not match a known intent/pattern dialog within the bot.

[https://docs.google.com/document/d/1SjzCsHOhLK6QRbZRsLOCveQpG53_oESlpkhBsuQqPLg/edit?usp=sharing](https://docs.google.com/document/d/1SjzCsHOhLK6QRbZRsLOCveQpG53_oESlpkhBsuQqPLg/edit?usp=sharing)

### NLU

#### How best to program the intents and entities for compound sentences?

E.g.

##### should entities be used for variations of "don’t" “can’t” “won’t” ? 

Will this benefit the system NLU in anyway? What about "cannot" “will not” “do not” variations?

* You don’t have to define variations. During runtime, we expand sentences and process it. Can’t transforms to Cannot

* Do not include I/We/You..

* NLU takes care of compound phrases. It is good to test and tune the matched status from within the domain before using it in a dialog. Testing and tuning the sentence is a good practice.



#### Avoiding False Positives (How to Optimise / Train NLU Intent Detection)

[UPDATE: New document from Ravi here](https://docs.google.com/document/d/1Dw2sXrMmUPWZd-fkV_3vr3b77altwKfHlJKweVcQtmo/edit?usp=sharing)

##### Beware overuse of entities in training phrases

The greater the number of matching entities in a single training phrase, the higher the score level.

Therefore overuse of entities in training phrases for irrelevant information such as greetings or other general terms, will incorrectly inflate the score and lead to false positives.

E.g

The more entities matched in the intent training phrase, the stronger the match. For example, in the training phrase* "ENT_hello I am going ENT_travelling"* if I match both **"ENT_hello"** AND **"ENT_travelling"** it is a stronger match than if I just match **"ENT_travelling"**. Remove entities that are not indicative of the intent to reduce false positives.

Entity matches increase the intent match strength. Sometimes this can result in false positives because of the strength of the entity match. When I tested using the entity match I got a GOOD match at 71%. When I removed the entity match and just used the word "from" in the training phrases I got a FAIR match at 55%.

Further HSBC specific use cases / examples linked here [https://docs.google.com/a/liveperson.com/spreadsheets/d/1CWsDLKIA6kRLuge70nPZjjP_n-YkEu5wbu512RSYPAs/edit?disco=AAAAC4Su_Qk](https://docs.google.com/a/liveperson.com/spreadsheets/d/1CWsDLKIA6kRLuge70nPZjjP_n-YkEu5wbu512RSYPAs/edit?disco=AAAAC4Su_Qk)

[https://docs.google.com/a/liveperson.com/spreadsheets/d/1CWsDLKIA6kRLuge70nPZjjP_n-YkEu5wbu512RSYPAs/edit?disco=AAAAC4Su_Qk](https://docs.google.com/a/liveperson.com/spreadsheets/d/1CWsDLKIA6kRLuge70nPZjjP_n-YkEu5wbu512RSYPAs/edit?disco=AAAAC4Su_Qk)

##### Training Phrases should be one sentence - not many

In intent "Activate a Card" remove text "I think I need to ENT_activate it." from training phrase 

*"I have been unable to use a new ENT_card linked to this account. **I think I need to ENT_activate it.**".*

Training phrase should be one sentence, not multiple. Multiple sentences increase risk of false positive as they provide more opportunity for a wider range of matching.

### Troubleshooting

#### "My Quick Reply question is not showing"

*Quick Replies only show a maximum of **3** options when used inside FB Messenger*

#### *Unmatched Phrases Not Showing in Analytics*

##### Reason

When using a Knowledge Base without using any intents in intent builder, analytics does not track the unmatched phrases.

##### Mitigation

Add the following global function

```javascript
function __initConversation(){

   botContext.setBotVariable('__recordUnMatchedPatternPhrases__',true,true,false);

}
```

This function should be called at the start of every conversation processed by your bot automation. Within this function the code sets the flag to recording unmatched pattern phrases.

### Exporting/Importing Bot Automations to other environments

##### Process Guide

[https://docs.google.com/document/d/1d9rbMetEX4DRKgi85XWCg7LFTT1EW3GTHEhDR4dZTyg/edit#](https://docs.google.com/document/d/1d9rbMetEX4DRKgi85XWCg7LFTT1EW3GTHEhDR4dZTyg/edit#)