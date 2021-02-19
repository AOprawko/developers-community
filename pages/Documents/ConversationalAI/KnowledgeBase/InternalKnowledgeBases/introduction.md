---
pagename: Introduction
redirect_from:
Keywords:
sitesection: Documents
categoryname: "Conversational AI"
documentname: Knowledge Base
subfoldername: Internal Knowledge Bases
permalink: knowledge-base-internal-knowledge-bases-introduction.html
indicator: both
---

### What's an internal knowledge base?

An internal knowledge base is a built-in knowledge base, i.e., one whose content you can create and manage directly within the Knowledge Base application. You can:

* Start from scratch, and add articles directly within the Knowledge Base application.
* Do a one-time import of articles from a CSV file, and maintain the articles thereafter within the Knowledge Base application.
* Link the knowledge base to a Google sheet. You can maintain the Google sheet and sync the knowledge base to overwrite the knowledge base with the sheet’s contents. Or, you can add the knowledge base using the Google sheet and work thereafter entirely within the Knowledge Base application.

An internal knowledge base always makes use of a [Natural Language Understanding (NLU) engine](intent-builder-natural-language-understanding.html) to [evaluate the articles](knowledge-base-using-intents-with-kbs.html#search-modes) in the knowledge base against the consumer’s utterance (the intent). The articles, which have associated intents, are matched and [scored](knowledge-base-using-intents-with-kbs.html#scoring-and-thresholds) by the engine, and the most accurate article is sent to the inquiring consumer.

### High-Level Workflow

When adding an internal knowledge base, follow this workflow:

1. In Knowledge Base:
    1. [Add the internal knowledge base](knowledge-base-internal-knowledge-bases-knowledge-bases.html). In this step you can import articles from a CSV file or link to a Google sheet that contains them.
    2. [Add the articles](knowledge-base-internal-knowledge-bases-articles.html) if necessary.
    3. [Train](knowledge-base-using-intents-with-kbs.html) the articles to match consumer utterances.
2. Expose the articles to consumers by:
    * (Conversation Builder) [Adding a Knowledge Base integration](conversation-builder-integrations-knowledge-base-integrations.html) in a bot
    * (Conversation Orchestrator) Adding a Knowledge Base integration as a part of an [Agent Assist](conversation-orchestrator-agent-assist-overview.html) component

### Domain intents versus Knowledge Base intents

When dealing with a knowledge base, an intent is the question that the consumer has ("What's your return policy?") or a problem the user needs to solve ("I can't remember my password."). 

While the article's title represents the intent of the article, you can also specify the intent using alternate formats. You can define these alternate formats in a domain in Intent Builder. Here, they're called "training phrases":

<img style="width:300px" src="img/ConvoBuilder/kb_intents_IB.png">

Or, you can do this inside the article by adding what are called "intent qualifiers":

<img style="width:350px" src="img/ConvoBuilder/kb_intents_KB.png">

When you [add an internal knowledge base](knowledge-base-internal-knowledge-bases-knowledge-bases.html), you'll specify which configuration approach you're using for the knowlege base:

<img class="fancyimage" style="width:400px" src="img/ConvoBuilder/kb_intentAssociation.png">

* **Domain Intents**: Select this if you're using intents from a domain that's defined in Intent Builder. Then specify the domain.
* **Knowledge Base Intents**: Select if this if you're using intents (i.e., intent qualifiers) that reside in the article itself. **Note**: Knowledge Base intents are a *legacy feature*, as discussed below.

#### Choosing between Domain intents and Knowledge Base intents

With Domain intents, you can be using any of the following [NLU engines](intent-builder-natural-language-understanding.html) for intent matching:
* LivePerson
* LivePerson (Legacy)
* A third-party NLU engine 

Domain intents make it possible for you to use the same domain in multiple ways, i.e., in knowledge bases, bots, or other features like [Intent Analyzer](intent-analyzer-overview.html).

Knowledge Base intents are a *legacy feature* that uses the LivePerson (Legacy) engine for intent matching. Knowledge Base intents don't allow for reuse, as the intent qualifiers themselves are defined within the articles in the knowledge base.

### Languages

When you [add an internal knowledge base](knowledge-base-internal-knowledge-bases-knowledge-bases.html), you'll specify its language.

When a text-to-text search is performed and a match is found, the knowledge base's language doesn't play a role. Whatever article is matched is simply returned.

When an NLU search is performed, the knowledge base's language works differently based on how you've defined the knowledge base:

* If the knowledge base uses *Knowledge Base* intents, the knowledge base's language is used. In this case, only English and Spanish are supported.

* If the knowledge base uses *Domain* intents, the domain's language is used instead. In this case, the languages supported depend on the domain's [NLU provider](intent-builder-natural-language-understanding.html).

### Active versus inactive articles

An article is either active or inactive. Active articles are returned in knowledge base searches in Knowledge Base integrations while inactive articles aren't returned.

* An article is *active* if its **Enable Article** setting is turned on. If the article has **Valid From** and/or **Valid To** dates (optional, specified in [UTC](https://www.timeanddate.com/worldclock/timezone/utc)), it is only active during the specified time period. If no dates are specified, an enabled article is always active.
* An article is *inactive* if its **Enable Article** setting is turned off. It's also inactive if the setting is turned on, but the current date and time in UTC falls outside the time period specified by **Valid From** and/or **Valid To**.

<img class="fancyimage" style="width:700px" src="img/ConvoBuilder/kb_validDates.png">

In the Knowledge Base application, you can easily identify an article's current status:

<img class="fancyimage" style="width:700px" src="img/ConvoBuilder/kb_articleStatus.png">

#### Using Valid From and Valid To dates

You can specify optional **Valid From** and/or **Valid To** dates when you [add an article](knowledge-base-internal-knowledge-bases-articles.html). 

For example, you might run a promotion over a holiday that has an associated FAQ, and that FAQ should only be used for a finite period of time. Or, you might have an article that you want to become active on a certain date and remain so indefinitely. You can satisfy requirements like these with **Valid From** and/or **Valid To**.