# Online Advertising Capabilities: Hit List

Discussion Paper for the
W3C Web Advertising Business Group

Contributors include: Michael Kleber <[kleber@google.com](mailto:kleber@google.com)\>, Mike O'Neill <[michael.oneill@baycloud.com](mailto:michael.oneill@baycloud.com)\>, Sanjay Saravanan (Facebook)

Draft

## Purpose

The mission of the W3C's Improving Web Advertising Business Group is to identify areas where standards and changes in the Web itself can improve the online advertising ecosystem.  This document aims to collect various needs expressed by the advertising-interested users of the Web platform which seem like they might be met by changes to browsers.

For terminology and an explanation of the parties involved, see [OnlineAdvertisingParticipants.md](OnlineAdvertisingParticipants.md).

## The List

### Measurement of Specific Single-Domain Events

Capabilities here are well-developed, but there are a lot of events that can be counted, so there is always room for improvement.

* Fraud prevention.  Limitations on the availability of cookies have made it harder to notice and identify unusual or potentially-fraudulent behavior.

* Viewability.  APIs like [Intersection Observer](https://developer.mozilla.org/en-US/docs/Web/API/Intersection_Observer_API) and its [v2 successor](https://developers.google.com/web/updates/2019/02/intersectionobserver-v2) are examples of dedicated APIs for things that are otherwise hard to measure.

#### Short-term Round Trip Communication Integrity

Often a single server communicates with a single browser multiple times during the same page view — for example, serving an ad in response to an ad request, and later receiving some information about the user's interaction with that ad.  There is risk of fraud if the server can't tell whether the two requests really came from the same browser.  This need could be met with some notion of browser identity that is short-term (e.g. per-site per-day/hour/page) and provable (e.g. based on cryptography, not just a bearer token).  Without identity we are stuck relying on in-band signals, which are notoriously easy to forge.

This is one tool to help with the "Fraud prevention" use case.

### Measurement of Related Cross-Domain Events

The ability to tie together two events on different domains has historically relied on 3rd-party cookies.  There is room for Web platform APIs that allow measurement in which the user agent helps tie together two events, without needing or providing any common notion of identity.

* Ad click attribution / conversion measurement.  The WebKit [Ad Click Attribution for the Web draft spec](https://trac.webkit.org/wiki/ad-click-attribution-draft-spec) describes one approach, with privacy properties based on capping the amount of information allowed in a measurement.  Mozilla's [experiments with Prio](https://hacks.mozilla.org/2018/10/testing-privacy-preserving-telemetry-with-prio/) and the [Secure Aggregatation component of Google's Federated Learning](https://federated.withgoogle.com/) are examples of infrastructure that could support another approach, with privacy based on requiring some level of aggregation across many users.

* Audience measurement.  The proposal by this Group [Privacy protecting metrics for web audience measurement](https://github.com/w3c/web-advertising/blob/master/admetrics.md) introduced the idea of collecting ad visibility information in privacy preserving way, enabling the collection of aggregated statistics gathered from user agents without communicaing user identifiers, or "singling out" the user. The key ability here is to count the numer of _unique_ users for whom some event happened — that is, the combination of some "Measurement of Specific Single-Domain Events" use case with cross-site de-duplication.  As long as the population being counted and reported is large enough, and it is verifiably impossible to collect personal data about an individual, this measurement can be revealed while preserving users' privacy.

  There are several layers of complexity in the setup for audience measurement:
  
  - In the simple version of this use case, a single party knows everything about the event that took place.  Example: counting the number of people who saw a particlar ad. 
  
  - However, advertisers almost always buy media on multiple digital publishers, and rely on 3rd party measurement providers (ComScore, Nielsen, Kantar, etc.,) to provide deduplicated unique reach across media. This addes an extra layer of complexity, because the 3rd party measurement provider aims to track information about the media being consumed (while knowing nothing about _who_ consumes the media), but rely on audience data providers that aim to provide information about demographics (age, gender, etc.,) of users consuming the media (while knowing nothing about _what_ media is being consumed). In essence, there are two parties who each know something about the event which they do not want to share with each oher, but they wish to learn joint distribution statistics.  For example, one party (3rd party measurement provider) knows what ad was just shown and another party (audience data provider) knows the demographics of the user it was shown to, and the first party aims to provide an aggregate like "Number of women age 18-34 in NYC who saw this ad" without any of the parties learning information about individual users to protect their privacy.
  
  - Further complexity is introduced if a party knows some data about a user while they are visiting one site, but want to count events sliced by that data while the user is visiting a different site.  Here it might be possible to preserve user privacy while at the same time having the browser transport the per-user data across site boundaries, but the question is much more subtle and relies on aggregation for a level of protection beyond mere deduplication.

### A/B Experimentation

Some questions about the impact of ads are best answered by comparing the overall behavior of two groups of users — for example, one group who are exposed to the ads and one group who are not.  Experiments are widespread — solutions here should accomodate hundreds of different parties wanting to run experiments, and parties running millions of experiments at a time.

* Treatment on a Single Domain.

  If all decisions about whether a user is in the A or B group only need to be made in a context where a consistent user identity is available (for example on a single domain, or across multiple sites where the user has a single logged-in identity), then the already-listed capabilities would be sufficient: users can be divided into groups based on the consistent identity, and outcomes for each group could be measured using the techniques from "Measurement of Related Cross-Domain Events" above.

* Treatment Across Domains.

  The desire here is to run experiments that begin with dividing users into an A and a B group in a way that is consistent as they traverse multiple domains, without access to a shared cookie space or signed-in notion of the user's identity.

  This seems difficult.  One user might be in the A or B branches of many different experiments, run by many independent third-party contributors to a page's overall content.  Revealing each of dozens of independent A-vs-B assignments would readily fingerprint the user and introduce an undesirable tracking vector.   Some sort of "entropy budget" could mitigate the fingerprinting threat but might lead to race-condition competition for access to experiment state, which would be undesirable in many ways (including invalidating the experiment results).

### Consent contingent signaling

It would be valuable for users to control what advertisements they see, 
which would help make the advertisements more effective. 
At the moment ad "personalisation" is done via UID cookies with external services processing the personal data,
but this is usually without the user being aware, and beyond their ability to control.
If users were asked what data about themselves they were prepared to allow advertisers or intermediaries to use, e.g. their purchase intentions, 
it could be far more accurate and users likewise more responsive to the ads.

Browsers could have a role in enabling 1st party (e.g. publisher) sites to request such information and then communicate it in a
site-specific manor to intermediaries i.e. embedded subresources on their sites.

### Publisher controlled tracking protection.

If publishers choose to implement some of the privacy preserving mechanisms proposed here or elsewhere, they
may want to control how a subset of their embedded subresources have access to third-party cookies and the like.
If they are establishing user trust by taking care to minimise the broadcasting of personal data to third-parties they might value the ability
to constrain third-party cookies and other tracking by embedded subresources, for example
by enforcing the `SameSite=Lax` attribute on embeddee `Set-Cookie` headers and `document.cookie` writes. 
