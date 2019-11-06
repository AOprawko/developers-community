---
pagename: All iOS Messaging SDK Release Notes
Keywords:
sitesection: Documents
categoryname: "Messaging Channels"
documentname: Mobile App Messaging SDK for iOS
permalink: mobile-app-messaging-sdk-for-ios-all-release-notes.html
indicator: messaging
---

<br>

Listed below are all of the Release Notes for previous versions of our Mobile App Messaging SDK for iOS. To learn more about the SDK and how to get started, see the [MobileSDK iOS Quick Start guide](/mobile-app-messaging-sdk-for-ios-quick-start.html).

<a href="mobile-app-messaging-sdk-for-ios-release-notes.html">View all release notes</a>
{% for operatingsystem in site.data.releasenotesios %}
{% for release in operatingsystem.releases %}
{% if forloop.first %}
<a href="mobile-app-messaging-sdk-for-ios-latest-release-notes.html">Mobile App Messaging SDK for iOS Release Notes {{ release.releasename }}</a>
{% else %}
<a href="/{{ release.releasename | slugify }}.html">{{ release.releasename }}</a>
{% endif %}
{% endfor %}
{% endfor %}
