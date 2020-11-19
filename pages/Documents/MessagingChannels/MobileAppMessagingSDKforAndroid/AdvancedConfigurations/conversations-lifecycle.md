---
pagename: Conversations Lifecycle
redirect_from:
  - android-conversations-lifecycle.html
Keywords:
sitesection: Documents
categoryname: "Messaging Channels"
documentname: Mobile App Messaging SDK for Android
subfoldername: Configuration

order: 40
permalink: mobile-app-messaging-sdk-for-android-configuration-conversations-lifecycle.html

indicator: messaging
---

During the course of the conversation, consumers can take several actions such as Mark as urgent to receive a faster service, or Resolve conversation to let your agents know they have received their answers.

LivePerson API:

```java
public static void checkActiveConversation(final ICallback<Boolean, Exception> callback)
public static void checkConversationIsMarkedAsUrgent(final ICallback<Boolean, Exception> callback)
public static void checkAgentID(final ICallback<AgentData, Exception> callback)
public static void markConversationAsUrgent()
public static void markConversationAsNormal()
public static void resolveConversation()
public static boolean clearHistory()
```

*Note: Click [here](android-methods.html) for more information.*

Also via Callbacks:

```java
void onConversationStarted(LPConversationData convData);
void onConversationResolved(LPConversationData convData);
void onConnectionChanged(boolean isConnected);
```

*Note: Click [here](android-callbacks-index.html#livepersoncallback) for more information.*
