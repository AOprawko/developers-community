---
pagename: Set Up Maven in LivePerson
redirect_from:
Keywords:
sitesection: Documents
categoryname: "Conversational AI"
documentname: Maven
subfoldername: AI Powered Routing
permalink: maven-ai-powered-routing-set-up-maven-in-liveperson.html
indicator: messaging
---

You can use policies you have created to route conversations by sending conversations to the Maven Skill in LiveEngage. Maven will evaluate all the policies and then execute those policies (e.g. transferring the conversation to a skill). 

This is appropriate if:
- You are only routing based on Inbox and Custom (FaaS or Static) attributes
- You don't have a concierge bot
- You do not need high levels of customizations and therefore programmatic access to Maven capabilities

#### Create a Maven Bot In LiveEngage

1. Login to LiveEngage, and go to the Users tab, click Action and select Add

2. Select bot type and type in a name (e.g. mavenBot)

3. Select “Generate API Key”

4. Copy the API key for later use

5. Select the Agent role


#### Provide Bot User Credentials to Maven

1. On the Left Navigation, select Intent & Context Policies, and then click the Bot config button

2. Fill out the credentials

