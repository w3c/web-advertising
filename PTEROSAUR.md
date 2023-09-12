# PTEROSAUR

   * [Introduction](#introduction)
   * [Motivating Use Case](#motivating-use-case)
   * [API Example Flow](#api-example-flow)
   * [Design Elements](#design-elements)
   * [Privacy Considerations](#privacy-considerations)
   * [Alternatives Considered](#alternatives-considered)
   
# Authors

* George London george@survata.com

## Introduction

### Overview

This proposal is intended to be comprehensive, which means it’s long. Much of it can be safely skipped on a first read.

*If you already understand and agree with the need to do third-party measurement of brand ad campaigns*: read just “Proposal” and “API Example Flow”.
*If you need background on why measurement is important and is not currently possible through existing proposals*: also read “Background”, and “Motivating Use Case” / “Challenges”
*If you find yourself confused by (or disputing) a technical detail of the proposal*: read “Design Elements”
*If you find yourself skeptical of a privacy-related claim*: please read “Design Elements” and “Privacy Considerations”

### Background

Online advertising falls into two broad categories:

* **Direct response**, which is focused on driving an immediate, observable online action, i.e. a "conversion" (such as buying a pair of shoes).
* **Brand Advertising**, which is focused on increasing consumer awareness of a brand, on changing consumer 
attitudes (such as favorability towards a brand), or on driving some later (potentially much later) offline action like
buying a particular car.

Because brand advertisers typically cannot observe the outcomes they're paying to promote, they work with "measurement vendors"
to conduct surveys and statistically analyze the results to learn e.g. whether awareness increased among people who saw an ad.

The upcoming deprecation of third party cookies will break all currently available reliable methods of measuring the efficacy of
digital brand advertising. Without third party cookies, all current systems of collecting reliable brand lift survey data will cease to function.

### Proposal

We propose a method for measuring the efficacy of brand advertising while still protecting user privacy (without relying on third party cookies). We tie together ideas from [TURTLEDOVE](https://github.com/michaelkleber/turtledove/blob/master/README.md), 
 "[Private Click Measurement](https://github.com/privacycg/private-click-measurement)" and  Facebook's "[Private Lift Measurement](https://github.com/w3c/web-advertising/blob/master/private-lift-measurement-conceptual-overview.md)".
  We call
the new system PTEROSAUR, for **P**rivate **T**URTLEDOVE-based **E**lection of **R**elevant **O**nline **S**urveys with **A**synchronicity-**U**tilizing **R**eporting.
 
 In a nutshell: as in "Private Lift Measurement", ad campaigns will be allowed to serve a special ad unit that contains 
 not just the ad to be shown, but also a mechanism for selecting between a "test" ad and an alternative "control" ad. 
 The browser chooses which to show, and keeps a (private, secret-to-the-browser) record of each
 impression from that campaign and of whether those impressions were "test" or "control" with respect
 to each particular campaign. **In addition**, the ad unit will be able to ask the browser to add itself to a TURTLEDOVE-style
 "interest group" that corresponds to the campaign. These "interest groups" will be explicitly tagged with a registered measurement vendor,
 and at some (randomized) point after recording the impression group membership, the browser will make a request to
 the measurement vendor asking for a prepackaged [web bundle](https://tools.ietf.org/html/draft-yasskin-wpack-use-cases-00).
 Where an advertiser would provide an advertisement inside this bundle, the survey vendor instead provides a survey (which
 must be able to run offline). 
 
Later, when a browser visits a website that wishes to run a survey, that site can create a TURTLEDOVE iframe. That iframe selects the content to show exactly as normal in TURTLEDOVE - the iframe makes a contextual request and then checks if there is more demand (expressed as an auction bid) to target the interest group than there is to fill the slot based on page context. 
  The difference is that the survey would actually run in the ad slot (instead of an actual "ad") if it wins the auction.
   If a survey wins, the browser will look up its stored, web bundled copy of the survey and conduct the survey **without
    communicating any data back to the vendor**.

 
 When the survey is complete, the browsers will submit the survey answers to a new reporting API (which may 
 be based on a slightly enhanced version of the [reporting API](https://github.com/w3c/web-advertising/blob/master/private-lift-measurement-third-party.md#reporting-conversions) used in "Private Lift Measurement"). At some
 (randomized) point in the future, the browser will submit a report to the measurement vendor that looks like:
 
 ````json
{
"interest-group": "com.measurement-vendor.nike.campaign-123",
"survey_answers": {"A": "yes", "B": "No", "C": 10},
"is_test": 1,
"exposure_history": ["2020-05-05 | placementID-123,creativeID-456"]
}
````
 
This report contains no user identifier, and no other identifiable or linkable information (see “Reporting Results” for details). Because the timing is randomized,
the vendor cannot know that the report came from a particular browser.

The results that measurement vendors deliver to brand advertisers are inherently statistical and aggregated, e.g.
"People who saw the movie trailer were 10% more likely to see the movie." Measurement vendors need survey responses
purely for generating these statistics; they do not need or want to link survey responses or browsing history to any 
particular individuals. So anonymous, asynchronous reports would be a sufficient way for the vendors to learn the results of
the survey (without having the opportunity to link survey responses or ad exposure history to any particular individual).

So overall, the PTEROSAUR system is a win-win, because it provides brand advertisers what they need (the necessary survey
data to statistically measure efficacy) while at no point allowing the measurement vendor to learn anything about the 
browsing history of any particular known individual, nor to associate survey responses or browsing history with any 
stable individual identifier. It also builds almost entirely on browser APIs that have already been proposed and so will
require a relatively minimal amount of additional effort for browser vendors to implement.

## Motivating Use Case

Brand advertising makes up a large and growing share of digital advertising. As such, brand ads
provide a vital source of revenue for many web publishers. Please see ["Advertising 101"](#advertising101.md) for a more
in depth discussion of brand advertising.
 
Brand advertisers face substantial economic pressure to justify their spending on advertising, but their ads typically do
not produce online conversions that can be counted and attributed to establish a causal link between spending and outcomes.
So brand advertisers really instead on **surveys** to prove efficacy.

Currently, these surveys are conducted by placing a measurement vendor's third party tracking pixel inside brand ad units,
and then waiting until these "ad exposed" consumers subsequently visit a website (or network of websites) where surveys
can be conducted. The measurement vendor uses the cookie to re-identify the consumer and serve them a survey that's
pertinent to an ad campaign they've been exposed to.

Although building a cross-site profile of user behavior is not a *goal* of brand lift measurement, it is currently an
incidental but *necessary* component. Moving to a more private web while still allowing brand measurement will require browsers
to provide new functionality that allows measuring efficacy without sacrificing privacy.

The use case around brand lift measurement (and specifics as to why it's crucial that brand lift be measured by **neutral third parties**) is
discussed extensively in "[Support For Advertising Use Cases](https://github.com/w3c/web-advertising/blob/master/support_for_advertising_use_cases.md#brand-lift-measurement)".

The goal of the PTEROSAUR API is to support this use case with the following outcomes:

*   Brand advertisers can reliably learn the (statistically aggregated) causal impact of their advertising on the web.
*   Measurement vendors can target pertinent surveys to respondents who are relevant to measuring a
 particular campaign (i.e. they are **either** exposed **or** control with respect to that campaign). But vendors cannot
  learn which respondents took which survey (or why).
*   Neither advertisers nor measurement vendors nor publishers can learn browsing behavior or ad exposure history of any 
identified (or even pseudonymously identified) individual.
*   People who do not wish to take surveys or to have their opinions recorded (even on an anonymous basis) can avoid being
included in any brand lift measurement by declining to be surveyed. 

Please also see [How Brand Lift Measurement Works](https://github.com/w3c/web-advertising/blob/master/brand_lift_overview.md)
for a detailed discussion of the mechanics of the current system.

### Challenges

In order to 
measure the effectiveness of brand ads, measurement vendors need to survey a sufficiently large sample of "exposed" (a.k.a. "test")
respondents who are known to have seen a particular ad, and also a sufficiently large sample of "control" respondents who are known
not to have seen the ad. If these exposed and control groups are large enough and similar enough (or can be adjusted via "causal inference" 
techniques to be sufficiently similar), the measurement vendor can conclude that any observed difference between the exposed group
and control group were *caused* by the advertisement being measured.

So measurement vendors face two key challenges:

1. In order to statistically analyze the responses to a survey, the measurement vendor needs to know whether the
 response came from someone who did vs. did not see the ad. “Lift”, the key metric measurement vendors report,
  is defined as the difference between the opinions of people who saw the ad vs. comparable people who didn’t see the ad.
   So at least in the reporting phase, vendors need to know whether a person actually saw the ad.  
   But the surveys happen on different domains than the ad exposures do (and the ad exposures
    for a single campaign may happen across many different domains). So the measurement vendor unavoidably
     requires a piece of information about an event (the ad impression) that happened on a different domain.
 
2. Measurement vendors may be measuring hundreds of different campaigns for different brands/products simultaneously and the KPIs for 
each campaign are idiosyncratic and campaign-specific. Honda wants to know if their ads are driving purchases of Hondas, not
just whether they're driving purchases of cars. Because surveys are expensive to conduct, the vendors cannot ask every
 possible respondent every possible survey question; "[spray and pray](https://github.com/w3c/web-advertising/blob/master/brand_lift_overview.md)" 
 surveying is completely uneconomical. This means that vendors must make a decision *at survey time* about which survey
 to serve a potential respondent. And this decision must be based on specific knowledge of cross-domain advertising exposure.
 
At first glance, it may seem like the requirements of the measurement vendor are strongly opposed to the goals of preventing the linkage
of user identity with cross-site information as described in Google's [Privacy Model for the Web](https://github.com/michaelkleber/privacy-model).
Fortunately, a closer look reveals that the goals can actually be made compatible by realizing that although measurement vendors
**do** require cross-site information, they do **not** need (or even want) to join that cross site information to *any* 
first party identity (not even their own). Measurement vendors are interested in respondents' contributions to statistical 
aggregates, not in their identities. And by using the browser as a neutral broker, the PTEROSAUR system can build a wall
that allows selecting the right surveys and aggregating the survey responses without giving even a malicious measurement
vendor any opportunity to link browsing history to a first party identifier.


## API Example Flow

This section describes one way the PTEROSAUR flow might work end-to-end.  There are many open design choices; see the [Design Elements](#design-elements) section for explanations and discussion of the various steps.

In the course of browsing the web, browsers will be served "impressions" of many different ad campaigns. In the current world, 
a tracking pixel inside the ad would "phone home" to the measurement vendor to 
record the ad impression, keyed to a third party cookie. 

The new API aims to allow the measurement vendors to continue to measure the effectiveness of these ad impressions, but 
in a way that does not reveal the browsing history of anyone who does not explicitly consent to participate in a 
measurement study.

In PTEROSAUR, the ad server will provide the browser with ad HTML that is formatted similarly to (if not exactly like)
what's proposed in Facebook's Private Lift Measurement proposal:

```html
<iframe campaignId="123" set-interest-group="com.measurement-vendor.honda.campaign-123" reportingdomain=”https://measurement-vendor.com”>
<a testGroupPercentage=”50”>
  <testGroup>
    <div>
      <img src=”...” / placementId="456" creativeId="789">
      <p>...</p>
      ...
    </div>
  </testGroup>
  <controlGroup>
    <div>
	    <video src=”...” / placementId="456" creativeId="control-1">
      <p>...</p>
      <img src=”...” />
      ...
    </div>
  </controlGroup>
</a>
</iframe>
```

On load, the browser makes a (secret) selection of whether to show the "test" ad or the "control" ad. This selection happens 
exactly like in the Private Lift Measurement proposal. Once the ad loads, the browser makes an (internal, secret-to-the-browser)
record that keeps track of the test/control assignment with respect to the particular campaign, the placement ID of the ad slot, the 
time of the impression, and the creative ID that was actually selected and shown.

The first major "new" element of PTEROSAUR is that the ad iframe gets the opportunity to add the browser to a TURTLEDOVE
"interest group". In the above example, this is done through an attribute in the iframe `set-interest-group="com.measurement-vendor.honda.campaign-123"`
but it could also be done through a JavaScript API, or the interest group identifier could be automatically derived from
some combination of the advertiser ID, the measurement vendor ID, and the campaign ID. 

The impression record the browser retains (which will never leave the browser) might look like:

```json
{
  "interest_group": "com.survata.honda.campaign-123",
  "campaign_id": 123,
  "is_test": 1,
  "placement_id": 456,
  "creative_id": 789,
  "timestamp": "2020-05-20T12:34:56+01:00"
}
``` 

(These records can be compressed to minimize storage space, but each record will be only at most a few hundred bytes uncompressed).
The records can be made to age out of the browser storage after some fixed time (e.g. 90 days). 

One difference vs TURTLEDOVE is that in this case, **the interest group is being set by the advertiser/iframe, not by the publisher**.
It would be acceptable for the publisher to need to set some sort of "feature policy" to permit the iframe to set interest groups,
but having the publisher actually be the one to set measurement interest groups would add substantial and undesirable operational complexity.

Once the interest group is set, the flow proceeds just like in TURTLEDOVE: after a random 
(though ideally <= 12 hour) period, the browser makes a un-credentialed request to `reportingdomain`, specifying the interest
group that's been set and asking for a prepackaged [web bundle](https://tools.ietf.org/html/draft-yasskin-wpack-use-cases-00)
which the measurement vendor would like to show to the browser. 

```
GET https://measurement-vendor.com/.well-known/fetch-bundles?interest_group=com.measurement-vendor.honda.campaign-123
```

This "pre-fetching" process is exactly analogous to the process where TURTLEDOVE pre-fetches interest group-relevant ads after
 a browser has been added to an interest group. For advertisers using TURTLEDOVE, these packages will contain
ad units. But for measurement vendors, they would contain surveys (which must be possible to conduct fully offline). 
 
The response points at a [Web Bundle](https://wicg.github.io/webpackage/draft-yasskin-wpack-bundled-exchanges.html) holding 
a survey, along with some browser-intelligible metadata about when that survey can appear, and some ad network logic
 and data for later decision-making.


```
[{'group-owner': 'measurement-vendor.com',
  'group-name': 'honda.campaign-123',
  'bundle-cbor-url': 'https://measurement-vendor.com/surveys/honda-campaign-123.wbn',
  'cache-lifetime-secs': 21600,
  'auction-signals': {honda_campaign_value': 250},
  'decision-logic-url': 'https://measurement-vendor.com/js/on-device-bid.js'}]
```


The browser fetches the on-device bidding JS and the Web Bundle.  The bidding JS is very simple:

```javascript
function(adSignals, contextualSignals) {
    return adSignals.honda_campaign_value;
}
```


A while later the browser visits a website which has agreed to run "rewarded" surveys for the measurement vendor. 
(Perhaps the site offers a browser game which the visitor may only play after completing a survey). The site loads a 
TURTLEDOVE iframe, pointing to `measurement-vendor.com`.  The response requests an in-browser auction to pick between a 
an "agnosticly" targeted survey and the previously-fetched interest-group-targeted survey:

```
const contextualBid = 107;
const metadata = {'network': 'measurement-vendor.com',
                  'auction-signals': {'incentive-value': high}};
const rendered = navigator.renderInterestGroupAd(contextualBid, metadata);
if (!rendered) {
  ...render the contextually-targeted ad or survey...
}
```

The measurement vendor's on-device auction logic runs, and since the the browser has been exposed to the Honda campaign
the decision function returns the value 250.  The browser compares that to the contextual ad's bid of 107, finds that
 the contextual bid is lower, and renders the previously-downloaded web bundle. Thus the Honda-specific survey begins.
 
Ideally the bidding script will be allowed to know some metadata about the slot itself which can be used to know
if it's an appropriate opportunity to run a survey at all. Please see [Opportunity Metadata](#opportunity-metadata) for a discussion.

Up to here, everything has worked essentially exactly the same as in a normal advertising-based TURTLEDOVE flow. After
this point some additional PTEROSAUR-specific enhancements are required:

1. The survey will run as a JavaScript-based single page application (SPA) which will take the user interactively
through some set of predetermined questions. In order to avoid leakage of information about which survey was selected, the
 TURTLEDOVE iframe must allow execution of JavaScript without access to the network or to any writable storage.
2. Some users will prefer not to take a survey and should be allowed to early (or immediately) abort the survey. However,
if the host origin is providing valuable consideration for survey-takers, it is reasonable for them to withhold that
benefit from people who do not complete the survey. So the survey should be able to have a minimal communication channel
with the host page in order to indicate whether a survey completed vs. was ended early. This could be handled
by allowing the iframe to contain `onSuccess` and `onAbort` handlers (as attributes) and allowing JavaScript running
within the iframe to invoke one of these handlers after finishing its other business.
3. A survey is useless without *some* way to report the results. Giving network access to the survey would make it difficult
to guarantee privacy (since the survey could use the network to report back to the measurement vendor's server that
User 123 took Survey XYZ (though this information would be of limited utility if third party cookies are disabled and the
measurement vendor would not be able to recognize this user on any foreign domain)).
   
When the survey is complete, the iframe files a **reporting request** via a new reporting API that is conceptually very
similar to the existing [Conversion Measurement API](https://github.com/WICG/conversion-measurement-api) proposal. The iframe
asks the browser to asynchronously submit a report formatted like:
  
````json
{
"interest-group": "com.measurement-vendor.honda.campaign-123",
"survey_answers": {"A": "yes", "B": "No", "C": 10},
}
````

At this point, the records stored by "Private Lift Measurement" becomes relevant. When a reporting request is filed against
an interest group which also has associated impression records, the browser's reporting system should automatically join
the test/control bit and the impression history records to these survey answers and produce an annotated report that looks like:

````json
{
"interest-group": "com.measurement-vendor.honda.campaign-123",
"survey_answers": {"A": "yes", "B": "No", "C": 10},
"is_test": 1,
"exposure_history": ["2020-05-05 | placementID-123,creativeID-456"]
}
````
 
At some random time in the future, the browser will submit this report to the `reportingdomain`. Notably, the report contains
**no user identifier**, , and can be subject to privacy-preserving limitations such as automatically added noise 
(for differential privacy) or minimum group sizes (for k-anonymity). Please see the section on “[Reporting Results](#reporting-results)” below for more details. 

The measurement vendor can aggregate this report with all other submitted reports for the same survey, partition the reports
into *test* and *control*, and use them to draw the necessary statistical conclusions in order to report the ads' efficacy
back to their brand advertising clients. Because the measurement vendor and the advertising clients are only interested in
deriving statistical insights, these anonymized reports are sufficient for their purposes.

Note that the end-to-end PTEROSAUR flow manages to pass the necessary data between the necessary parties while **adhering fully** to the 
stated goals of the Google privacy model:

1. There is no linkage of 1st party identifiers. The measurement vendor *has* no identifier for the respondent. They 
use no third party cookies, and they are never allowed to place a cookie within a TURTLEDOVE iframe. There is no identifier
to link.
2. There is no sharing of cross-site activity that is in any way keyed to any stable global (or even local) identifier. In
this example, the measurement vendor does learn that *some* person visited Site A and Site B, but they cannot learn
*who* made those visits. They cannot share that information with another party in a way that would allow linkage to 
another party's identity-space, and they cannot recognize that person again in the future because, again, they have no
identifier.

## Design Elements

The browser can be trusted to protect data about the user to the best of its ability. If the impression storage mechanism is designed so as to never allow raw exposure history to leave the browser without express user consent, then this data can be safely considered "private." 
Because the records are kept locally, it is important to be respectful to the user by keeping resource consumption to the minimum necessary. Fortunately, the proposed impression records are very small (<1kb per impression), require minimal computation to produce, require no network access to write, can be aged out after some time, and can be made clearable by the user. (Though it would be preferable if test/control assignment persisted through a browser data clear, so as to prevent control group pollution).

Keeping the recorded information local is relatively straightforward -- simply never allow read access to the data
in any context that has write access to anything (whether network, disk, cache, etc.). This is analogous to the security model behind
a [Sensitive Compartmented Information Facility](https://en.wikipedia.org/wiki/Sensitive_Compartmented_Information_Facility),
as used to manage access to classified information.

As discussed in [Reporting](#Reporting Results) below, some of the impression data may be released in the reporting phase.
But in reporting, the impression data is severed from *any* identifier for the browser/user who viewed those impressions. 
And if necessary, an additional layer of privacy can be provided by withholding any impression data that's connected groups small enough that they fail to meet some k-anonymity threshold.

### The Interest Group Label

The interest groups set by a PTEROSAUR ad will be used later by a TURTLEDOVE iframe to determine that the user of the browser should be surveyed
about this particular ad campaign. Because the measurement vendor will need to later supply a survey (and possibly bidding
logic) tied to the interest group, the interest group needs to have a stable identifier. A few desirable properties for
these identifiers are:

1. They should be compact (to minimize space required to store interest group memberships)
2. They should be namespaced (so that Nike and Adidas could have their own respective "shoe-buyers" interest groups)
3. They should be at least basically human readable, so that the browser can show end users a meaningful list of 
interest groups they're part of (and who added them to those groups)

Labels structured like `com.measurement-vendor.nike.running-shoes` would satisfy these constraints.

One property that might initially *appear* desirable would be that interest groups should not be able to distinguish between
"test" groups and "control" groups for a specific ad campaign. If the ad unit is forced to apply
the same interest group membership to browsers regardless of whether the browser selects a test or control ad, that would
appear to provide a desirable layer of "plausible deniability". 

But closer examination reveals that such deniability is of very limited value. 

1. The linkage between a user and an interest group assignment never leaves the browser. The only place the interest groups are read at all is inside of a) the TURTLEDOVE bundle
pre-fetching logic b) inside of the bidding logic inside a TURTLEDOVE iframe c) in an anonymized result report. In none
 of these cases can any external party read or capture the group in a way that is linkable to a user identifier. So the privacy-relevant question
  reduces to "what information could an external party gain from 
distinguishability between "test" and "control" interest groups
*in the context where the interest group is accessible*?" The answer is: not much.
2. Knowing that someone is in the "control set" for a particular ad campaign nevertheless conveys a significant implication: i.e.
 that that person was in the "targeting set" for the campaign. E.g. if Nike targets an ad campaign exclusively to people in the interest group `com.runnersworld.distance-runners`,
and puts everyone who's *served* the ad opportunity into `com.measurement-vendor.nike.running-shoes`, then anyone who ends up in `com.measurement-vendor.nike.running-shoes` 
can be assumed to have been in `com.runnersworld.distance-runners`, even if they saw the control ad. 
3. Conditional on knowing that someone was in the targeting set for an ad campaign, it's in many cases somewhat immaterial
whether a person actually saw the ad in question. My status as "a long distance runner" is a fact about me that has
durable interest (e.g. it predicts my receptivity to discounts on running shoes). But "my having actually seen
a specific banner ad" is of much less durable interest. Other than the measurement vendor, it's highly unclear what
economic value any party could derive from knowing I actually saw the specific ad (if they already know I'm a long distance
runner).

Still, why not just "be safe" and retain the veneer of plausible deniability? Because the fact of whether someone
actually saw an ad **is** of substantial utility to the measurement vendor.

The reason it's of interest is that the size of test and control groups will often be asymmetric. In the current 
advertising ecosystem, using randomized control groups is expensive -- advertisers have to pay to win ad auctions
and then show blank ads. Using a 50/50 test control split would double the cost of the campaign.

Hopefully once "Private Lift Measurement" is more broadly adopted, ad servers will build in mechanisms to run randomized
control trials more cost-effectively (e.g. by allowing the auction winner to "yield" their purchased "control" slots to the next highest bidder, so 
that a real, paying ad still gets shown). But even if the direct monetary cost is reduced, large control groups still 
have a practical cost: they reduce the maximum attainable *reach* of a campaign. Large brand advertisers
often have large scale goals like "reach *every* long distance runner in the western United States". If advertisers find a 
perfect channel that contains every single member of the target audience, then doing a 50/50 test control split suddenly means
that they can only even possibly reach half of the runners. This limits the maximum total efficacy of even campaigns that
 are very per-capita effective.
 
Whether because of direct cost or because of reach, advertisers will generally want to keep control group sizes down to 
the minimum necessary size. If that means e.g. a 90/10 split, then respondents from the control group may be 
even more rare (and thus more valuable to the measurement vendor) than exposed respondents. 

If measurement vendors are allowed to set both an exposed interest group (e.g `com.measurement-vendor.nike.running-shoes.exposed`)
and a control group (e.g `com.measurement-vendor.nike.running-shoes.control`) depending on whether the ad was actually shown,
then they can employ bidding strategies that pay more for control respondents than exposed, 
thus maximizing the odds that they are able to collect enough of those harder-to-find respondents to be able to draw
the necessary statistical conclusions.   
 
  
### Survey pre-fetching

PTEROSAUR should provide measurement vendors with a) no more information about
survey respondents than is commercially necessary and b) *no information at all* about end users who do not voluntarily
 elect to respond to a survey.

Using the TURTLEDOVE "pre-fetching" mechanism allows a sufficiently "blinded" arrangement whereby the measurement vendor
can convey surveys to the right respondents without learning unduly much about any end users.
   
The trick is to simply package surveys as offline-capable [Web Bundles](https://wicg.github.io/webpackage/draft-yasskin-wpack-bundled-exchanges.html) and otherwise treat them just like any other web
bundle that TURTLEDOVE might fetch (even though those other bundles will normally contain ads instead of surveys). 

Because the request is randomly delayed, the vendor cannot collude with the ad server to learn that a particular ad impression
produced a particular survey pre-fetch. Because the pre-fetch request is uncredentialed, the vendor cannot associate the 
survey request with any user identifier. In order to avoid allowing the use of IP addresses as a tracking vector, the 
browser could require the vendor to use [willful IP blindness](https://github.com/bslassey/ip-blindness) to respond to 
the pre-fetch request as anonymously as possible.

Just like any other party supplying bundles to TURTLEDOVE, the measurement vendor is now equipped to win a TURTLEDOVE auction
and "serve" the survey in a TURTLEDOVE iframe.

### Opportunity Metadata

Not all slots where a TURTLEDOVE iframe might run will be appropriate for running a survey. Unlike ads which can be viewed passively, surveys require some engagement. Some surveys have enough questions that they take several minutes to answer. In order to convince surveytakers to invest that time, measurement vendors often need to provide a reward to people who agree to take the survey. This reward might take the form of "unlocking" some premium content like a video.

Measurement vendors would want to bid on opportunities where a survey is likely to be completed (e.g. where the TURTLEDOVE iframe is gating some sufficiently valuable content) and not bid on opportunities that are unlikely to produce a response (e.g. a slot at the bottom of a news article).

Survata currently pre-arranges for various online publishers to carry our surveys. It should be possible to continue this basic arrangement by prearranging for publishers to place TURTLEDOVE iframes that list survata.com as the “ad network” from which the contextual request is made. Currently these publishers are paid per survey completion, so they are well incentivized to place the survey opportunities in appropriate places. But depending on how PTEROSAUR is implemented, it may become challenging to pay only for survey completes instead of survey “starts”. If we pay only for starts, publishers would be incentivized to encourage people to start surveys even if the content they’ll receive in exchange will not be sufficiently valuable to motivate the respondent to actually complete the survey. So we’d need a way to “keep our partners honest” and ensure that they’re only starting surveys at appropriate opportunities.

Moreover, some surveys are longer than others and need commensurately more valuable rewards. So giving the host page a mechanism to pass certain signals into the auction would be very helpful. This can probably work much like the way TURTLEDOVE is currently discussing handling providing context to ad auctions: the page can opt to pass any useful context “one way” into the auction where it can be used by the bidding scripts to make appropriate decisions, or any information returned from the contextual request can also be made available to the interest-group bidding scripts. 


### Running the survey

Currently, measurement vendor surveys typically run in a normal, unrestricted iframe that is placed inside a "host" website.
The "host"  might be a "4th party" publisher who is agreeing to conduct surveys in exchange for some per-response compensation,
or it might be a "panel" provider, or it might be a "hosted" survey provider (e.g. Qualtrics). The measurement vendor 
decides which survey to run (based on exposure metadata) and then asks a series of questions on successive "screens." The
responses are usually "saved" through a request to the server (either after each answer, or at the end of the survey).

Taking a survey is an entirely optional, voluntary act (even when participation in the survey is compensated). 
And it is nigh-universally understood that the point of a survey is to share one's opinion with interested researchers. 
So although it is desirable to prevent a vendor from learning information about the user that the user did not intend to
 disclose, it is reasonable to assume that the end user is comfortable with the measurement vendor learning any information
 they explicitly provide as part of their survey responses. (And any well-intentioned measurement vendor would be happy 
 to provide survey takers with a clear summary of how their answers will be used before they elect to participate). 

Some users will not want to be surveyed -- their preferences should be respected. Users must retain the ability to
opt-out of any particular survey. In the case of a "surveywall", the TURTLEDOVE iframe may be directly blocking some 
content the user is interested in, so the iframe should have a way to communicate to the host page that it's finished. It
should also be able to convey to the host page whether the survey was "successful" or not so that the host page can apply
appropriate clean up logic. For example, if the survey was "unsuccessful" (e.g. because the user opted out) then the page
might decide not to unlock the content that was gated by the surveywall. 

In order to manage their costs, measurement vendors
would like to be able to know whether particular host sites are producing better or worse completion rates, so ideally
the iframe could pass some sort of cryptographically signed "success" token to the host domain (which it could "redeem")
with the measurement vendor to record that "some survey was successfully completed on my domain" (without revealing which 
survey was shown or which person was surveyed). This "success token" scheme could also potentially help with measuring
"viewability" or "brand safety": the ad unit could contain some (locally running JavaScript) that assesses whether the
ad made it into the viewport (or whether the text of the web page (somehow conveyed into the iframe) was free of blocklisted terms)
and only issue a success token if the ad was viewable and "safe".
 
Since the survey has been provided as a web bundle, it can run as an offline "single page application" within the TURTLEODVE iframe.
It can ask the questions it needs to and then terminate.
 
Once the survey ends, instead of sending the responses over the network it files them with the [reporting API](#reporting-results).  

### Reporting Results

 Reporting is the final major piece of the puzzle. Vendors need to analyze the surveys they conduct, and to do that 
 they need to collect the results.
 
 As discussed above, results are currently reported synchronously over the network during or immediately after a survey is run.
 The downside of this direct reporting is that it allows the vendor to connect a particular device with the surveys
 taken on that device. This has operational value to the survey vendor - vendors don't want to give the
 same person the same survey twice so knowing who took which survey helps avoid resurveying. 
 
 With third party cookies being eliminated, much of the ability for a measurement vendor to abuse knowledge of cross-site
 activity disappears. Even if vendors could build a dossier of cross-site ad exposure history keyed to their own cookie,
 they'd be unable to recognize these respondents again on different domains (unless they could "sync" cookies
 with those other sites. But without third party cookies, that would require a user to navigate directly from the measurement
 vendor's iframe to the specific site which would seek to recognize them (a journey users would be unlikely to make at scale)). 
 So there would be little value in building dossiers of cross-site exposure and little danger in being able to link
 individual pseudonymously-identified survey responses to cross-site ad exposure history. 
 
 If browser vendors nevertheless maintain that even this linkage risk is too great, PTEROSAUR allows "breaking the connection" 
 that links a particular survey response and browsing history to a particular browser. The specific identity of a survey respondent 
is much less important to measurement vendors than the respondent's "role" as an "exposed" or "control" with respect to 
a particular ad campaign. By only allowing survey data to be reported
 through a browser-controlled, asynchronous reporting API, the browser can release just the anonymized data that vendors 
 need, without allowing them to link the data to individuals (or allowing the possibility of re-identification).
 
 Basically, at the end of the survey (which has run offline within a TURTLEDOVE iframe),
  the survey would "file a report" with the browser reporting API. At a randomized time in the future, the browser would
  send an uncredentialed request to the vendor with a payload like:
  
````json
{
"interest-group": "com.measurement-vendor.nike.running-shoes.exposed",
"survey_answers": {"A": "yes", "B": "No", "C": 10},
"is_test": 1,
"exposure_history": ["2020-05-05 | placementID-123,creativeID-456"]
}
````

This report would contain no user identifier and so could not be linked back to any particular survey respondent. But because
vendors are only interested in statistical aggregations anyway, these de-identified reports provide vendors with the data they need to draw
population-level conclusions (e.g. "compared to a control group, people who saw Ad X on Publisher Q were 10% more likely
to be aware that Nike makes running shoes).

It should be noted that these reports would include the ad exposure history, probably as a list of timestamped "impression records"
that contain a creative ID and a placement ID. The vast preponderance of brand advertising on the web is served by "Google Campaign Manager",
Google's ad server. GCM uses creative IDs to identify particular "creatives", i.e. the actual content of an ad. So for example
"Creative-123" might be a picture of a puppy and "Creative-456" might be a picture of a picnic. Placement IDs identify particular
targeting parameters, for example "banner ads on CNN" or "video ads targeted to a purchased segment of first time homebuyers."

This information is extremely useful for statistical analysis because it allows answering granular questions like "was my
ad more effective on recipe websites than on home improvement websites?" or "were people more likely to notice an 
ad featuring a puppy than an ad featuring a picnic?" These questions produce answers that are actionable -- brand advertisers
 can actually adjust their advertising tactics to be more effective at conveying their message. Without this exposure data,
 vendors can only answer questions like "was my campaign effective as a whole?" That's certainly crucial to know, but it doesn't
 suggest anything brand advertisers can actually do differently except "spend more money" or "spend less money."
 
Browser vendors might have reasonable concerns about releasing exposure information (particularly placement IDs, because
they are often tied to individual websites where ads were run). But there are several reasons why this is a less 
substantive privacy concern than it might initially appear:

1. The exposure histories are de-identified. The report that browsers file contains no user identifiers, and it's filed
at a random time to avoid timing relinkage. So vendors are not able to learn that "John Smith saw an ad on CNN",
only that "some person saw an ad on CNN." The threat models discussed in the Privacy Sandbox focus on the dangers of linking
first party identities to cross-site data. In this case, there is no first party identity. Measurement vendors cannot
learn anything about any *particular* respondent and so the privacy of any "named" individuals remains fully protected.
2. As discussed above, survey participation is voluntary. Vendors can be required to clearly convey to participants that 
participating will cause their de-identified exposure history to be released to measurement vendors. There are good reasons to prevent
vendors from learning data users do not wish to disclose, but it may border on paternalistic to prevent users from being allowed to willfully disclose
data.
3. The timing of many impressions across different sites does form something of a fingerprint, but without third
party cookies no single party has a sufficient record of these exposures to cross reference against. Suppose John Smith saw an 
ad on The New York Times on July 3rd, on The Washington Post on July 4th, and on CNN on July 5th. Many other people
 saw ads on those individual sites on the same day. To identify John Smith's particular pattern, the three sites 
 would have to collude and comprehensively pool their data to narrow down the set of people who'd had that particular cross
 site pattern. It is unlikely that many competitive sites would collude both with the measurement vendor and with their
 own competitors to allow this type of re-identification. (If a user had multiple exposures on a single site then
 that single site could potentially collude with the measurement vendor to reidentify an individual, but not if the
 timestamp granularity is sufficiently reduced).
 
If the risk of releasing impression data still seems too high, a further layer of security could be added by applying a "k-anonymity" filter to dynamically
reduce the granularity of the reporting. Brand ad campaigns typically 
serve millions of total impressions, and the ad server already records how many impressions are served of each placement and creative.
Browsers could require that placement / creative data can only be released 
if e.g. 1000 or 10,000 other people have been exposed to those
particular IDs. IDs without enough impressions could be dropped from reporting. This would prevent advertisers from being able
to potentially identify respondents by using excessively narrowly-targeted placements. Sequences of time stamped impressions could have 
their timestamp granularity reduced (e.g. reporting the week of exposure instead of the day). That would ensure that 
advertisers are only learning about large-group level browsing patterns. (Of course, such a k-anonymity scheme would add substantial implementation
complexity because some aggregation server (most likely GCM) would need to count impressions and authorize data releases).

### Avoiding resurveying

Duplicate responses (i.e. the same respondent re-taking the same survey) lower the quality and usefulness of a survey 
sample. Currently, vendors use cookies to record the fact that a particular respondent has taken a particular survey
and so to avoid offering that same survey again. If cookies are unavailable, it becomes more difficult for a vendor
to know that "even though this person has seen Campaign X, I should not survey them now because I've already surveyed
them."

One relatively simple way to address this problem is to have the browser record a single `counter` variable for each web bundle ID
(which would only need to be retained for 9-12 months, since campaigns rarely run longer than that). Each time the survey 
is shown, the `counter` would be incremented. The counter value should be private to the browser, but could be made available
to TURTLEDOVE bidding scripts. This would have the added benefit of allowing graceful frequency capping for advertisements 
within TURTLEDOVE. Advertisers could simply decline to bid if their ad has already been shown more than the desired number of times.

An alternative option would be to allow the measurement vendor to "unset" the measurement interest group upon running a survey.
But this has problems: someone could be exposed at T0, surveyed
at T1, and re-exposed to the same campaign at T2. If they came across a surveywall again, it would be hard for the vendor
to know that they should not be resurveyed. Also, many ad campaigns will decline to spend the extra money necessary to 
do a randomized control trial; instead of creating a "marked" control group they'll just treat "non-exposed" as controls.
If a respondent were surveyed at T1 as exposed and then exposure info were cleared, they might be surveyed again
later as a control. That would be particularly compromising to the statistical results.

## Privacy Considerations
  
### Small campaign / small placement targeting

The most potentially sensitive sensitive information revealed in the PTEROSAUR scheme is that "*some* person saw `Ad X` on `Site A` at time `T0` and then
 saw `Ad Y` on `Site B` at time `T1`." 
 
 This disclosure introduces two risks:
 
 1. If placements or creative identifiers have excessive granularity then the placement and creative IDs could be used as 
 a covert tracking vector. Suppose an advertiser attaches a distinct placement ID to each individual ad impression. They allow the website 
 hosting the ad to learn these placement IDs. The host site then sends a report to the advertiser saying "our logged in user
 `john@smith.com` saw placement 987654321." If the measurement vendor then receives a survey response that is known to have
 seen placement 987654321 then the vendor (if they collude with the advertiser) can conclude that that particular respondent is `john@smith.com`. I.e.
 they're able to learn cross site activity of a specific named individual.
 2. Even if vendors don't link browsing history to named individuals, they do learn that particular "behavior patterns" *exist* on
 the internet and this mere fact might be considered to convey sensitive information. For example, suppose "placement-123"
 was only served on articles about hedgehog ownership and placement-456 was only served to local news sites in California. 
 If a vendor saw that someone had seen an ad with placement-123 and a different ad with placement-456, they could conclude
 with some reasonable probability that *someone* in California owns a hedge hog. This is a notable conclusion because
 it's illegal to own a hedgehog in California. (To be very clear, the vendor does not learn *who* owns the hedgehog. But
 it is a very open question whether the unnamed hedgehog owner has a right to object to some external party learning
 that there are in fact illicit hedgehog owners in California).  
 
 These risks both have similar mitigation options:
 
 1. Ignore them. 
    * For #1, doing this type of excessive-granularity tracking would introduce substantial operational 
 complexity into running an ad campaign and would produce a very non-self-evident amount of economic value for the attacker.
 Even if the advertiser identifies John Smith as having seen a particular ad, they're not able to meaningfully retarget John Smith without third party cookies. And they would not have been able to show him the ad in the first place without some kind of
 pre-existing targeting system (meaning the data to target him already existed elsewhere) *or* unless the targeting was random
 (in which case learning that John saw the ad reveals nothing durable about him). 
    * For #2, it would be a reasonable philosophical "line in the sand" to grant web users the right to prevent other 
     people from knowing that they themselves are a particular type of person, but to *not* grant them a right to 
     prevent other people from knowing that the type of person they (anonymously) are *exists*. This is a question of
     ethical values and as such it doesn't have an empirically right answer. But extending the right to privacy to include
     even anonymous activities would implicitly re-categorize even such banal activities as basic *website analytics* as 
     unethical (i.e. if it's unethical to observe the behavior of an anonymous user on multiple websites, why should it be
     any more ethical on a single website)? That extended concept seems a step too far. Furthermore, we should remember
     that this impression data will only be (anonymously) released about people who have volunteered to participate
     in a survey (an activity that is universally understood to involve contributing at least some data about oneself
     to a researchers' analysis). End users who are concerned about protecting the existence of their particular behavior
     pattern can simply avoid answering PTEROSAUR surveys.
2. Solve both problems simultaneously by adopting "thresholding" to create k-anonymity. The browser could limit the 
granularity of exposure reporting such that details are only provided about past exposure when those details
 are shared with at least *k* (perhaps 1000) other individuals. So if a campaign ID had been seen by <1k users, the browser would not report
 a survey based on it at all. If the campaign was big enough but a particular placement ID had only 10 exposures, the reports could include the
 campaign ID, but not mentions of placement IDs with insufficient other observations. If there were many others with the same placement IDs, the 
 browser could provide timestamp information at a granularity that maintained k-anonymity (e.g. reporting only the week of exposure, then with larger
 sample reporting the day, then with larger sample reporting the hour). There would need to be some system for evaluating the size of the 
 k-sets, but if there needs to be a reporting gateway anyway for IP blinding, the gateway could strip out information without sufficient k-size.

## Alternatives Considered

Unfortunately, no other viable alternatives have been proposed that would allow economically feasible measurement of brand lift
without the use of third party cookies.