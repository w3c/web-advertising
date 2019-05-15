# Online Advertising Capabilities: Hit List

Discussion Paper for the
W3C Web Advertising Business Group

Author: Michael Kleber <[kleber@google.com](mailto:kleber@google.com)\>

Draft, updated 2019-05-14

## Purpose

The mission of the W3C's Improving Web Advertising Business Group is to identify areas where standards and changes in the Web itself can improve the online advertising ecosystem.  This document aims to collect various needs expressed by the advertising-interested users of the Web platform which seem like they might be met by changes to browsers.

For terminology and an explanation of the parties involved, see [UseCasesofOnlineAdvertising](UseCasesofOnlineAdvertising.md).

## The List

### Measurement of Specific Single-Domain Events

Capabilities here are well-developed, but there are a lot of events that can be counted, so there is always room for improvement.

* Fraud prevention.  Limitations on the availability of cookies have made it harder to notice and identify unusual or potentially-fraudulent behavior.

* Viewability.  APIs like [Intersection Observer](https://developer.mozilla.org/en-US/docs/Web/API/Intersection_Observer_API) and its [v2 successor](https://developers.google.com/web/updates/2019/02/intersectionobserver-v2) are examples of dedicated APIs for things that are otherwise hard to measure.

### Measurement of Related Cross-Domain Events

The ability to tie together two events on different domains has historically relied on 3rd-party cookies.  There is room for Web platform APIs that allow measurement in which the user agent helps tie together two events, without needing or providing any common notion of identity.

* Ad click attribution / conversion measurement.  The WebKit [Ad Click Attribution for the Web draft spec](https://trac.webkit.org/wiki/ad-click-attribution-draft-spec) describes one approach, with privacy properties based on capping the amount of information allowed in a measurement.  Mozilla's [experiments with Prio](https://hacks.mozilla.org/2018/10/testing-privacy-preserving-telemetry-with-prio/) and the [Secure Aggregatation component of Google's Federated Learning](https://federated.withgoogle.com/) are examples of infrastructure that could support another approach, with privacy based on requiring some level of aggregation across many users.

### A/B Experimentation

Some questions about the impact of ads are best answered by comparing the overall behavior of two groups of users — for example, one group who are exposed to the ads and one group who are not.

* Treatment on a Single Domain.

  If all decisions about whether a user is in the A or B group only need to be made in a context where a consistent user identity is available (for example on a single domain, or across multiple sites where the user has a single logged-in identity), then the already-listed capabilities would be sufficient: users can be divided into groups based on the consistent identity, and outcomes for each group could be measured using the techniques from "Measurement of Related Cross-Domain Events" above.

* Treatment Across Domains.

  The desire here is to run experiments that begin with dividing users into an A and a B group in a way that is consistent as they traverse multiple domains, without access to a shared cookie space or signed-in notion of the user's identity.

  This seems difficult.  One user might be in the A or B branches of many different experiments, run by many independent third-party contributors to a page's overall content.  Revealing each of dozens of independent A-vs-B assignments would readily fingerprint the user and introduce an undesirable tracking vector.   Some sort of "entropy budget" could mitigate the fingerprinting threat but might lead to race-condition competition for access to experiment state, which would be undesirable in many ways (including invalidating the experiment results).
