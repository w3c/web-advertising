# Cross-Browser Anonymous Conversion Reporting

## Motivation:

As browsers make changes to prevent cross site tracking, they are also proposing reporting APIs for specific ads measurement use cases. There are now three proposals for private conversion measurement: [Private Click Measurement](https://wicg.github.io/ad-click-attribution/index.html) (Webkit/Apple), [Conversion Measurement API](https://github.com/csharrison/conversion-measurement-api) (Chrome/Google) and [Private Lift Measurement](https://github.com/w3c/web-advertising/blob/master/private-lift-measurement-conceptual-overview.md). 

The central idea in all 3 of these proposals is to perform attribution **within the browser**. This allows for the matching of events which occur on different domains, without sharing user-level unique identifiers.

Unfortunately, none of these three proposals enable the measurement of conversions which occur on a different web-browser than the one where the ad was displayed. 

We know from lift studies (test group sees ads, control group does not) that ads shown on one browser often drive statistically significant increases in conversions from another. A common example is showing ads in mobile browsers will often drive conversions on Desktop computers. 

If we cannot find a privacy-preserving way to attribute conversions from Desktop browsers to ads shown on mobile websites, it will lead to less revenue for mobile web publishers. It’s also just not accurate. Lift studies tell us that without cross-browser conversion attribution, we will be undercounting the true value mobile websites drive for advertisers. 

## High-level goal:

Ideally, we’d be able to sync the locally stored browser data between different browsers belonging to the same person. Crucially, this would not necessitate any changes to the private conversion report format. We are not suggesting any kind of differentiation to help distinguish between same-browser and cross-browser conversions. 

The example below shows a flow for a click-to-conversion report across different browsers:

![diagram showing how click data could be synced across browsers](https://github.com/w3c/web-advertising/blob/master/Cross-Browser%20Anonymous%20Conversion%20Reporting.png)


## Threat Model:

1. In short, it should inherit the threat model of the underlying conversion measurement API. Regardless of whether we are adding this on top of the [Webkit/Apple proposal](https://wicg.github.io/ad-click-attribution/index.html), the [Chrome/Google proposal](https://github.com/csharrison/conversion-measurement-api), or the [Lift proposal](https://github.com/w3c/web-advertising/blob/master/private-lift-measurement-conceptual-overview.md), this extension shouldn’t change the basic guarantees of the proposal.
2. This syncing operation should be private. We need to be very careful to ensure that this data does not leak out to any entity beyond the browsers belonging to that person.
3. The “click” or “opportunity” data we are syncing should be authentic. By “authentic” we mean that it should reflect actual clicks from that actual person in an actual browser. We should have protections to prevent publishers from “click stuffing”; that is, generating a bunch of inauthentic clicks / opportunities and somehow exploiting some weakness in this approach to “sync” them to real browsers.

## Solution Constraints:

* An ideal solution will only require significant engineering work to be done by browsers, ad-networks, and perhaps a handful of other companies. Specifically, it should not require work from advertisers.
* There are only a few entities who can potentially answer the question of which devices belong to the same person:
  * Large publishers with a service that many people log-in to across multiple devices (e.g. Gmail, Yandex, Facebook, Reddit, Twitter, YouTube, Instagram, Mail.ru, vk.com, etc.)
  * Large technology/device providers that people sign-in to on multiple devices (e.g. iCloud, Chrome-Tabs, Google account, etc.)

## Disclaimer

This is not a perfect proposal. This is an early stage idea. The intention is to start a conversation with the broader community, and collaboratively work to improve on this starting point, to find even better solutions to this problem within the threat model and solution constraints described above.

## High-level description of a possible approach

There are a few important aspects of this proposed solution:

1. Enabling browsers that belong to the same person to discover one another
2. Once they have discovered one another, using end-to-end-encrypted communication between these browsers to sync click / opportunity / blind-signature data

# Enabling browsers that belong to the same person to discover one another

## Abstract:

* Leverage large publishers with a service that many people log-in to across multiple devices (e.g. Gmail, Yandex, Facebook, Reddit, Twitter, YouTube, Instagram, Mail.ru, vk.com, etc.)
* These websites know which browsers belong to the same person, as evidenced by the fact that the same username and password were provided at login time.
* Such publishers can implement a standard API, and submit to auditing requirements to qualify as an “Identity Provider” service (details of this API to follow).
* We want to protect the ecosystem from malicious “Identity Providers” who might try to intentionally compromise the privacy of people. As such, there should be a rigorous approval process websites must undergo in order to become an approved “Identity Provider”.
* Even assuming there is a rigorous approval process in place, browser vendors should be able to independently decide which “Identity Providers” they want to whitelist as “trusted” from the perspective of their own browser. It should be possible to change this list at any time, for example, if one of these companies is acquired or merges with another company, or if an audit uncovers evidence of non-compliance.
* Even if we assume that auditing requirements and whitelists are in place, we have attempted to design this proposal so as not to need to place “trust” in any particular “Identity Provider”. We have attempted to design it so that even a malicious “Identity Provider” would be unable to compromise the privacy of people.
* As such, we require two separate connections, through two publishers that are fully independent of one another, before “trust” is established with another browser.
* This only requires us to “trust” that there is no collusion between that pair of companies. Put another way, an “Identity Provider” who does not collude with any other “Identity Provider” would be unable to compromise the privacy of users.
* The implication of this design decision is that two browsers will not be linked unless the person logs into at least 2 websites with each, and even then only if those two websites are whitelisted “Identity Providers” in both browsers, and even then only if those two websites are fully independent 
* Example: If I sign into both my Reddit and YouTube account on my mobile browser and Desktop browser, that would suffice. 
* Counter-example: If I signed into both Gmail and YouTube on both browsers that would not suffice, since Gmail and YouTube are both controlled by Google.
* It’s technically possible to extend this solution to require 3 or more connections if we deem 2 insufficient, but for simplicity’s sake, we will explain how it works if you only require 2.

## Complete Explanation:

Full details in a [separate document](https://github.com/w3c/web-advertising/blob/master/enabling-browsers-that-belong-to-the-same-person-to-discover-one-another.md).

# Once they have discovered one another, using end-to-end-encrypted communication between these browsers to sync click / opportunity / blind-signature data

## Consideration of various approaches:

There are two approaches we have considered for this part:

1. “Identity Providers” provide end-to-end-encrypted messaging APIs that enable browsers to sync data with one another
2. Browser companies provide end-to-end-encrypted messaging APIs that enable browsers to sync data with one another

There are pros and cons to each approach. I’m currently leaning towards #2, “Browser companies provide E2EE messaging APIs that enable browsers to sync with one another”. I’ll just describe that approach in this document for brevity, but if there is interest we can flesh out the details of a #1 approach as well.

In the following chart, I’ll try to explain what we believe to be tradeoffs between these two approaches:


|                                               | “Identity Providers” provide E2EE messaging services | Browser vendors provide E2EE messaging services |
|-----------------------------------------------|------------------------------------------------------|-------------------------------------------------|
| When does the browser check for new messages? | When you open their website. Technically we could do so at other times, but it might confuse / surprise people to see this network activity. | When you launch the browser after a period of inactivity. Technically we could do it even more frequently than this, but that might result in major scalability concerns. |
| Who has access to click / opportunity / blind-signature data aside from the two browsers? | Nobody | Nobody |
| Who learns the graph of which browsers belong to the same person? | Nobody learns anything they don’t already know | A sufficiently motivated, malicious browser vendor could potentially attempt to learn *some* information about the graph… but only partial information, and at great computational cost | 
| How many cross-device conversions will this help measure? | If the browser only syncs data when someone opens the website of an identity provider, we might miss quite a few conversions. We would only be able to measure cross-browser conversions when there was an intervening visit to an “Identity Provider” website between the ad impression and the ad conversion. | Nearly all, assuming that we send out messages as soon as an ad interaction occurs, and that we check for new messages as soon as soon as a browser is launched after a period of inactivity. |
| Code complexity for browser vendors | Higher | Lower |
| Who bears the cost of operating an E2EE messaging service | Identity Providers | Browsers |
| Barrier to entry / cost associated with becoming an approved, whitelisted “Identity Provider” | Very high | Much lower |

## Abstract of approach #2 (browser vendors provide E2EE messaging service):

![Abstract diagram outlining how end-to-end-encrypted syncing of data between two browsers would occur](https://github.com/w3c/web-advertising/blob/master/Abstract%20of%20Cross-Browser%20E2EE%20data%20syncing.png)

* Similarly to how Signal / WhatsApp work, this would utilize asymmetric encryption.
* Each browser would have a single, globally scoped public/private key-pair, randomly initialized at the time of installation.
* To prevent this “public key” from being used for cross-site tracking, it would need to be kept hidden - inaccessible via any JavaScript API. 
* This globally scoped public key would only ever be sent to another browser via a secure, E2EE channel established in part 1 of this proposal: “enabling browsers that belong to the same person to discover one another”. 
* In this way, even the “Identity Provider” would never see this “public key”.
* Over time, each browser would build up an internal “contact list” of the public keys of other browsers belonging to the same person (and proven to be authentic by virtue of at least 2 shared logins).
* This “contact list” must also be kept absolutely secret, totally inaccessible via any JavaScript API.
* Each time an ad click / opportunity occurs, the browser would not only store a small payload of data locally, it would also broadcast it to every other browser in the “contact list”. 
  * These messages should be quite small. We can impose size limits to ensure this channel isn’t being used to send crazy stuff like image files.
  * The largest size payload I can think of at the moment would be if we were to combine the “private fraud prevention” proposal with Chrome’s Conversion Measurement API, in which case the decrypted message body would contain key-value pairs like this:
    * **From:** cooktopcove.com
    * **To:** nike.com
    * **ReportingDomain:** criteo.com
    * **CampaignId:** 0x85674244e6714253
    * **Nonce:** 0x56b5ca324f9ccde8265
    * **Publisher-Signature:** 0x79c4d52f70927abb1
* Each browser vendor would need to operate a piece of server-side infrastructure that can receive messages from anyone, and to allow only the intended recipient to download messages addressed to them. 
* In order to try to avoid disclosing the graph to the browser vendor, we can omit the public key of the sender, instead just sending a signature which can be validated by the receiver (by iterating over its own internally stored “contact list” to see if the signature is valid for any of those public keys).
* Messages would thus be sent with the following metadata:
  * **To:** `<recipient public key>`
  * **Encrypted-message-payload:** `<data payload encrypted with recipient’s public key, and only decryptable with the recipient’s private key>`
  * **Sender-signature:** `<signature of encrypted-message-payload, signed with sender’s private key, can be verified by anyone in possession of the sender’s public key>`
* We could build this atop a standard end-to-end-encryption framework like MLS.

## Threat Modeling various attack scenarios and suggested mitigations

See [separate document](https://github.com/w3c/web-advertising/blob/master/threat-modeling-for-cross-browser-anonymous-conversion-reporting.md)
