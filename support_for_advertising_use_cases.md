# Advertising Use Cases

This document provides an overview of key advertising use cases that depend on cross-site data sharing.

This document is broken down into 4 main sections based on the needs of the various entities in the web advertising ecosystem:
- [Advertiser Needs](#advertiser-needs)
  - [Basic Advertiser Needs](#basic-advertiser-needs)
  - [Specialized Advertiser Needs](#specialized-advertiser-needs)
- [Ad Network Needs](#ad-network-needs)
  - [Core Ad Network Needs](#core-ad-network-needs)
  - [Specialized Ad Network Needs](#specialized-ad-network-needs)
- [Publisher Needs](#publisher-needs)
- [User Needs](#user-needs)

# Advertiser Needs

## Basic Advertiser Needs

All advertising aims to drive an outcome. There are many types of desirable outcomes, and these outcomes differ along several key axes:

1. **Immediate vs delayed**: signing up to be an Uber driver right now,
vs. remembering to consider a Mercedes next time they're buying a car
2. **Actions vs. Attitudes**: taking a specific action like dialing a lawyer's office vs.
just remembering to "Think Different"
3. **Direct vs. Indirect**: buying glasses directly from warbyparker.com, vs. buying Ray-Ban's from their
 local optician after seeing a Youtube ad
 
Outcomes that are immediate, direct actions are typically referred to as "conversions" and the type of
advertising that prioritizes conversions is typically called "direct response advertising." Conversely, advertising that
focuses on outcomes that involve a change in attitudes or involve actions that are delayed (and often indirect) is typically called "brand advertising." 

(Please see the document on [Advertising Outcomes](advertising-outcomes.md) in this repo for a list of more potential outcomes.)

Both types of advertising are prevalent on the internet, and many ad campaigns by larger advertisers include a blend of
 both types. Direct response advertising represents the majority of digital advertising, but in recent years 
 spending on digital brand advertising has grown rapidly, particularly on premium publishers like the Washington Post and The New York Times.
 Some channels, like paid search, are dominated by direct response advertising. Others, like online video, are dominated by brand advertising. 

When an advertiser spends money to run an ad campaign they want to answer three very basic questions about it:
- How many people saw the ad? Was the ad visible, and was it shown to the people I intended to see it?
- How successful was the campaign at driving the desired outcomes/conversions?
- Are you confident that the ad impressions and ad conversions are from real, authentic people?

Although they sound like simple questions, in practice answering them well is a rather complex and difficult undertaking. 
The ads industry has endeavored to find good ways of answering these basic questions, and has developed a number of approaches to doing so, which is why there are more than 3 use-cases in this section.

- [Impression and Viewability Measurement](./use-cases-in-detail/impression-and-viewability-measurement.md)
  - [Viewability](./use-cases-in-detail/impression-and-viewability-measurement.md#viewability)
  - [Invalid Traffic](./use-cases-in-detail/impression-and-viewability-measurement.md#invalid-traffic)
  - [Third Party Verification](./use-cases-in-detail/impression-and-viewability-measurement.md#third-party-verification)
  - [Audience Verification](#audience-verification)
- [Aggregate Conversion Measurement](#aggregate-conversion-measurement)
  - [Conversion Lift Measurement](#conversion-lift-measurement)
  - [Brand Lift Measurement](#brand-lift-measurement)
  - [Click-through and View-through attribution heuristics](#click-through-and-view-through-attribution-heuristics)
  - [Multi-touch attribution](#multi-touch-attribution)
  - [Cross Browser / Cross Device Measurement](#cross-browser--cross-device-measurement)
- [Fraud Prevention](#fraud-prevention)
  - [Conversion Fraud](#conversion-fraud)
  - [Click-Flooding](#click-flooding)
  - [Malicious Browser Extensions](#malicious-browser-extensions)
  - [Malicious Mobile Apps](#malicious-mobile-apps)
  - [Malicious browsers](#malicious-browsers)
- [Brand Safety](#brand-safety)
- [Billing Transparency](#billing-transparency)
  
| Use-case | Chrome | Safari | Community Proposals |
|----------|--------|--------|---------------------|
| [Impression and Viewability Measurement](./use-cases-in-detail/impression-and-viewability-measurement.md) | There should be no conflict with Chrome’s proposed “[Privacy Model for the Web](https://github.com/michaelkleber/privacy-model)”. First parties should still be capable of measuring that the ads displayed on their own properties entered the viewport, that images and video were loaded, how much time they spent in the viewport, etc. None of this requires joining up user identity across multiple domains. The only possible complication that might arise would be with "blind rendering" as described in proposals like "[TURTLEDOVE](https://github.com/michaelkleber/turtledove)" and “[PETREL](https://github.com/w3c/web-advertising/blob/master/PETREL.md)”. Here, the publisher would have restricted access to the ad being rendered and we will have to discuss the feasibility of "viewability" measurement. There is a discussion on this [GitHub issue](https://github.com/csharrison/aggregate-reporting-api/issues/10). | Similar answer as that for Chrome. This should not be in conflict with Webkit's [Tracking Prevention Policy](https://webkit.org/tracking-prevention-policy/) given the measurement is entirely within the scope of a single publisher website. | |
| [Audience Verification](#audience-verification) | No support | No support | No Support | |
| [Conversion Lift Measurement](#conversion-lift-measurement) | A [section of the Conversion Measurement with Aggregation proposal](https://github.com/WICG/conversion-measurement-api/blob/master/AGGREGATE.md#view-through-conversions) describes explicit support for view through conversions, which should be sufficient to support lift measurement using experiments on a first party site (e.g. when using a first-party identifier to decide which experiment branch a person is in). | No support | Facebook proposal for “[Private Lift Measurement](https://github.com/w3c/web-advertising/blob/master/private-lift-measurement-conceptual-overview.md)” |
| [Brand Lift Measurement](#brand-lift-measurement) | No support | No support | No support | |
| [Click-through attribution](#click-through-and-view-through-attribution-heuristics) | Proposal: “[Click Through Conversion-Measurement Event-level API](https://github.com/WICG/conversion-measurement-api)” | Proposal: “[Private Click Measurement API](https://github.com/WICG/ad-click-attribution)” |
| [View-through attribution](#click-through-and-view-through-attribution-heuristics) | A [section of the Conversion Measurement with Aggregation proposal](https://github.com/WICG/conversion-measurement-api/blob/master/AGGREGATE.md#view-through-conversions) describes explicit support for view through conversions. | No support | |
| [Multi-touch attribution](#multi-touch-attribution) | The [Event-level API](https://github.com/WICG/conversion-measurement-api/blob/master/README.md) proposal covers last-click attribution.  A section of the [Conversion Measurement with Aggregation](https://github.com/WICG/conversion-measurement-api/blob/master/AGGREGATE.md#multi-touch-attribution-mta) proposal describes support for some built-in multi-touch models, and acknowledges that measurement providers writing their own models would be good to support if technically feasible. | No support | Facebook proposal for “[Privacy-Preserving Multi-Touch-Attribution and Cross-Publisher Lift Measurement](https://github.com/w3c/web-advertising/blob/master/privacy_preserving_multi_touch_attribution_and_cross_publisher_lift_measurement.md)” |
| [Cross Browser / Cross Device Measurement](#cross-browser--cross-device-measurement) | No support | No support | Facebook proposal for “[Cross Browser Anonymous Conversion Reporting](https://github.com/w3c/web-advertising/blob/master/cross-browser-anonymous-conversion-reporting.md)” |
| [Multi-Channel Attribution / Measurement](#multi-channel-attribution--measurement) | No Support | No Support | Adobe proposal for "[Privacy Preserving Multi-Channel Attribution](https://github.com/w3c/web-advertising/blob/master/privacy-preserving-multi-channel-attribution.md)" |
| [Conversion Fraud](#conversion-fraud) | A section of the [Multi-Browser Aggregation Service Explainer](https://github.com/WICG/conversion-measurement-api/blob/master/SERVICE.md#authenticating-inputs) describes using a service that includes an authentication mechanism. This use case is also being explored for the event level API in this [GitHub Issue](https://github.com/WICG/conversion-measurement-api/issues/13). | Confirmation this is a use-case Safari cares about addressing. In the process of collaboratively designing a solution on this [GitHub Issue](https://github.com/WICG/ad-click-attribution/issues/27). | Facebook proposal for “[Private Fraud Prevention](https://github.com/siyengar/private-fraud-prevention)” |
| [Click Flooding](#click-flooding) | No solutions yet for this problem. However, lift measurement could be a potential way of measuring this problem. This might be supported by the “[Aggregate Reporting API](https://github.com/csharrison/aggregate-reporting-api)”. | No solutions yet for this problem. | Facebook proposal for “[Private Lift Measurement](https://github.com/w3c/web-advertising/blob/master/private-lift-measurement-conceptual-overview.md)” a potential way of measuring this problem |
| [Malicious Browser Extensions](#malicious-browser-extensions) | No solutions yet for this problem. | No solutions yet for this problem. | |
| [Malicious Mobile Apps](#malicious-mobile-apps) | No solutions yet for this problem. | No solutions yet for this problem. | |
| [Malicious Browsers](#malicious-browsers) | No solutions yet for this problem. | No solutions yet for this problem. | |
| [Brand Safety](#brand-safety) | There should be no conflict with Chrome’s proposed “[Privacy Model for the Web](https://github.com/michaelkleber/privacy-model)”. When an ad targets an interest group, it is served as a web package with all subresources included and the detail of which interest group won lets you trace down the problematic campaign. However, the ads that were printed less than k times (k being the reporting threshold) would still pose a threat as they would remain undetectable because not reported. | Similar answer as that for Chrome. This should not be in conflict with Webkit's [Tracking Prevention Policy](https://webkit.org/tracking-prevention-policy/) given the measurement is entirely within the scope of a single publisher website. | |
| [Billing Transparency](#billing-transparency) | In TURTLEDOVE, the browser is the sole owner of billing information. | | |

## Specialized Advertiser Needs

These are use-cases that only apply to a (possibly large) subset of advertisers. 

- [Targeting](#targeting)
  - [Audience definition](#audience-definition)
  - [Exclusion Targeting](#exclusion-targeting)
  - [Lookalike Targeting](#lookalike-targeting)
  - [Retargeting](#retargeting)
  - [Display and target environment](#display-and-target-environment)
- [Frequency](#frequency)
  - [Frequency Capping](#frequency-capping)
  - [Frequency Optimization](#frequency-optimization)
- [Businesses with Multiple Domains](#businesses-with-multiple-domains)
- [Ads directing to large marketplaces](#ads-directing-to-large-marketplaces)
  - [Collaborative ads](#collaborative-ads)
  - [Dynamic Ads](#dynamic-ads)
  - [Email Marketing](#email-marketing)
- [Real time spend management](#real-time-spend-management)
- [Catalog management](#catalog-management)
    - [Product availability management](#product-availability-management)
    - [Price management](#price-management)
    - [Products promotion management](#products-promotion-management)
- [Creatives](#creatives)
    - [Coupon Management](#coupon-management)
- [Product Recommendation](#product-recommendation)


| Use-case | Chrome | Safari | Community Proposals |
|----------|--------|--------|---------------------|
| [Audience definition](#audience-definition) |  |  |  |
| [Exclusion Targeting](#exclusion-targeting) | Acknowledgement that this is a valuable use-case and a link to Facebook’s PETREL proposal on this [GitHub Issue](https://github.com/michaelkleber/turtledove/issues/3) | No support | Facebook proposal for “[Private Exclusion Targeting Rendered Exclusively Locally (PETREL)](https://github.com/w3c/web-advertising/blob/master/PETREL.md)” |
| [Lookalike Targeting](#lookalike-targeting) | Might be possible to achieve limited support by leveraging “[Federated Learning of Cohorts (FLoC)](https://github.com/jkarlin/floc)”.  Might require an extension to TURTLEDOVE offering a new way to create interest groups, which Chrome indicated support for in [this issue](https://github.com/michaelkleber/turtledove/issues/26). | No support | Facebook proposal for "[Privacy Preserving Lookalike Audience Targeting](privacy_preserving_lookalike_audience_targeting.md)" |
| [Retargeting](#retargeting) | Proposal: "[Two Uncorrelated Requests, Then Locally-Executed Decision On Victory (TURTLEDOVE)](https://github.com/michaelkleber/turtledove)" | No support | |
| [Display and target environment](#display-and-target-environment) | Supported in TURTLEDOVE thanks to the contextual request. Ability to use this together with interest group request limited by the JS complexity. |  |  |
| [Frequency Capping](#frequency-capping) / [Frequency Optimization](#frequency-optimization) | For ads served via TURTLEDOVE interest-group targeting, the proposal offers a way to [handle frequency capping on-device](https://github.com/michaelkleber/turtledove#on-device-auction).  For other types of targeting, while it will not be possible to enforce a hard "frequency-cap" across multiple websites, it might be possible to calibrate a target average frequency model. See discussion about how to do this on the explainer for the [Aggregate Reporting API](https://github.com/csharrison/aggregate-reporting-api#advanced-example-calibrating-a-frequency-capping-model). | No support | Adobe Proposal for "[Frequency Capping and Frequency Optimization](https://github.com/W3C/web-advertising/blob/master/frequency-capping-and-optimization.md)" |
| [Businesses with Multiple Domains](#businesses-with-multiple-domains) | Proposal: "[First Party Sets](https://github.com/krgovind/first-party-sets/)" | Some discussion in this [GitHub issue](https://github.com/krgovind/first-party-sets/issues/6) indicates weak support for at least the country-specific eTLD use-case, but various concerns with the current "First Party Sets" proposal. | | 
| [Collaborative Ads](#collaborative-ads) / [Dynamic Ads](#dynamic-ads) | Some discussion in this [GitHub issue](https://github.com/WICG/conversion-measurement-api/issues/32). No solutions or even strong acknowledgement of the importance of this use-case yet. | Some discussion in this [GitHub issue](https://github.com/WICG/ad-click-attribution/issues/36). Good collaborative problem solving going on. No firm solution yet. | Facebook proposal for “[Conversion Filters](https://github.com/w3c/web-advertising/blob/master/conversion-filters.md)” |
| [Email Marketing](#email-marketing) | unclear | unclear | |
| [Real time spend management](#real-time-spend-management) | High latency in TURTLEDOVE as the javascript updates have unknown frequency, and reporting is delayed in the [Aggregate Reporting API](https://github.com/csharrison/aggregate-reporting-api#advanced-example-calibrating-a-frequency-capping-model) | Supported as ads are not done in opaque iframe and bidding is not done remotely | |
| [Product availability management](#product-availability-management) | More details required. |  |  |
| [Price management](#price-management) | More details required. |  |  |
| [Products promotion management](#products-promotion-management) | More details required. |  |  |
| [Creatives](#creatives) | The creative building logic and material is all contained in the web bundle. Questions arise about the size of the web bundle. | More details required. |  |
| [Coupon Management](#coupon-management) | Some discussion in this [GitHub issue](https://github.com/WICG/conversion-measurement-api/issues/32). No solutions or even strong acknowledgement of the importance of this use-case yet. |  | Facebook proposal for "Conversion Filters". |
| [Product Recommendation](#product-recommendation) | Some discussion in this [GitHub issue](https://github.com/WICG/conversion-measurement-api/issues/32). Possible solutions addressed in issues [#31](https://github.com/michaelkleber/turtledove/issues/31) and [#36](https://github.com/michaelkleber/turtledove/issues/36) |  | Facebook proposal for "Conversion Filters". RTB House proposal for "Browser-side personalization". Nextroll Proposal for "Dynamic Creative Use Case" |


# Ad Network Needs

## Core Ad Network Needs
These are core essentials ad networks need to deliver value to advertisers.

- [Training ML Models](#training-ml-models)
  - [Click Through Rate (CTR) Model: P(click | impression)](#click-through-rate-ctr-model-pclick--impression)
  - [Conversion Rate (CVR) Model: P(conversion | click)](#conversion-rate-cvr-model-pconversion--click)
  - [Post-view attribution: P(conversion | impression)](#post-view-attribution-pconversion--impression)
  - [P(Display | Request)](#pdisplay--request)
  - [Return on Ad Spend (ROAS) optimization](#return-on-ad-spend-roas-optimization)
  - [Multiple Targets optimization](#multiple-targets-optimization)

| Use-case | Chrome | Safari | Community Proposals |
|----------|--------|--------|---------------------|
| [P(click \| impression)](#click-through-rate-ctr-model-pclick--impression) | There is no conflict with Chrome’s proposed “[Privacy Model for the Web](https://github.com/michaelkleber/privacy-model)”. Should be possible to use 1st party cookies to tie together multiple sessions from the same browser on the same website. | [isLoggedIn](https://github.com/WebKit/explainers/tree/master/IsLoggedIn) may potentially pose problems here for websites without login. Limited storage may make it hard to tie together multiple sessions from the same person. | |
| [P(conversion \| click)](#conversion-rate-cvr-model-pconversion--click) | This is a stated goal of Chrome’s “[Click Through Conversion-Measurement Event-level API](https://github.com/WICG/conversion-measurement-api)” | Not possible to train an ML model. | |
| [Post-view attribution: P(conversion \| impression)](#post-view-attribution-pconversion--impression) | Not possible to train an ML model. However, It may be possible to use the "Aggregate Reporting API" to obtain average conversion value measurement for very coarse-grain buckets of people. This could be used to perform calibration at this coarse-grain bucketed level. | Not possible to train an ML model. |  |
| [P(Display \| Request)](#pdisplay--request) | No support. | Not possible to train an ML model. |  |
| [Return on Ad Spend (ROAS) optimization](#return-on-ad-spend-roas-optimization) | Not possible to train an ML model. However, It may be possible to use the “[Aggregate Reporting API](https://github.com/csharrison/aggregate-reporting-api)” to obtain average conversion value measurement for very coarse-grain buckets of people. This could be used to perform calibration at this coarse-grain bucketed level. | Not possible to train an ML model. | |
| [Multiple Targets optimization](#multiple-targets-optimization) | No support | Not possible to train an ML model. |  |

## Specialized Ad Network Needs

- [Audience Selling](#audience-selling)
- [CRM Targeting](#crm-targeting)

| Use-case | Chrome | Safari | Community Proposals |
|----------|--------|--------|---------------------|
| [Audience Selling](#audience-selling) | Probably possible under "[TURTLEDOVE](https://github.com/michaelkleber/turtledove)" by defining interest groups usable by other parties. | No support | |
| [CRM Targeting](#crm-targeting) | possible if done via interest groups under "[TURTLEDOVE](https://github.com/michaelkleber/turtledove)"  | No support | |

# Publisher Needs

These use-cases are more focused on the publisher's perspective.

- [Problems faced by non-logged in publishers](#problems-faced-by-non-logged-in-publishers)
  - [Serving relevant ads](#serving-relevant-ads)
  - [Running an auction](#running-an-auction)
- [Affiliate Marketing](#affiliate-marketing)
- [Enablers for first parties](#enablers-for-first-parties)
  - [Federated Single Sign-on](#federated-single-sign-on)
  - [View-through Site Personalization](#view-through-site-personalization)
  - [Click-through Site Personalization](#click-through-site-personalization)
- [On-site Sponsored Product](#on-site-sponsored-product)
- [Search](#search)
- [Ad Safety](#ad-safety)
- [Malvertising Protection](#malvertising-protection)
- [Pausing Advertising](#pausing-advertising)
- [Advertisers exclusion](#advertisers-exclusion)
- [Revenue management](#revenue-management)
- [Support directly sold ads](#support-directly-sold-ads)
- [Floor rates](#floor-rates)
- [Competitive exclusions](#competitive-exclusions)
- [Weighted bidding](#weighted-bidding)

| Use-case | Chrome | Safari | Community Proposals |
|----------|--------|--------|---------------------|
| [Serving relevant ads (non-logged in publishers)](#serving-relevant-ads) | Proposal: “[Federated Learning of Cohorts (FloC)](https://github.com/jkarlin/floc)”. | No support | |
| [Running an auction (non-logged in publishers)](#running-an-auction) | The TURTLEDOVE proposal is built around a server-side auction for contextually-targeted ads, and then an in-browser auction for user interest targeting.  [This GitHub issue comment](https://github.com/michaelkleber/turtledove/issues/20#issuecomment-602800377) describes the RTB flow in some more detail. | No support | Verizon / Oath [write-up of this use-case](https://github.com/w3c/web-advertising/blob/master/rtb-use-case.md) |
| [Affiliate Marketing](#affiliate-marketing) | It may be possible to use a combination of the “[Click Through Conversion-Measurement Event-level API](https://github.com/WICG/conversion-measurement-api)” and the “[Aggregate Reporting API](https://github.com/csharrison/aggregate-reporting-api)” to get a first-order estimate of the number of the number of conversions driven by an Affiliate marketing link, but questions remain about “returns” and fraud. | Extremely limited support possible with the “[Private Click Measurement API](https://github.com/WICG/ad-click-attribution)”, but questions remain about “returns” and fraud. | |
| [Federated Single Sign-on](#federated-single-sign-on) | Yes - Proposal: “[WebID](https://github.com/samuelgoto/WebID)”| Generally supportive as per [Tracking Prevention Policy](https://webkit.org/tracking-prevention-policy/) " "*We consider certain user actions, such as logging in to multiple first party websites or apps using the same account, to be implied consent to identifying the user as having the same identity in these multiple places*". Policy also acknowledges unintended impacts to federated SSO setups. General discussion in the context of the [Storage Access API](https://github.com/privacycg/storage-access) which is related but not targeted for this use-case (non-goal)| | |
| [View-through Site Personalization](#view-through-site-personalization) | No support | No support | |
| [Click-through Site Personalization](#click-through-site-personalization) | There should not be any conflict with Chrome's proposed “[Privacy Model for the Web](https://github.com/michaelkleber/privacy-model)” unless this mechanism is used to pass through high-entropy identifiers which could be used to tie user identity across websites. See PING's [Privacy Threat Model](https://w3cping.github.io/privacy-threat-model/#goal-transfer-userid). | Similar to Chrome, this should not fall afoul of Webkit's [Tracking Prevention Policy](https://webkit.org/tracking-prevention-policy/) unless high-entropy identifiers are being used to perform "navigational tracking". See PING's [Privacy Threat Model](https://w3cping.github.io/privacy-threat-model/#goal-transfer-userid). | |
|[On-site Sponsored Product](#on-site-sponsored-product)| There should not be any conflict with Chrome's proposed “[Privacy Model for the Web](https://github.com/michaelkleber/privacy-model)” | Similar answer as that for Chrome. This should not be in conflict with Webkit's Tracking Prevention Policy given the measurement is entirely within the scope of a single publisher website. | |
|[Search](#search) | unclear, but relies a lot on contextual | unclear | |
| [Ad Safety](#ad-safety) | Very limited support as opaque iframe aggregated reporting allow for very little ex post audit. | No Support |  |
| [Malvertising Protection](#malvertising-protection) |  |  |  |
| [Pausing Advertising](#pausing-advertising) |  |  |  |
| [Advertisers exclusion](#advertisers-exclusion) | Unclear. To be defined. |  |  |
| [Revenue management](#revenue-management) | Little support in TURTLEDOVE, as nor reporting nor updating bidding JavaScripts is not real-time. This means delays in both information and action, leading to very complex and inaccurate budget/revenue management. |  |  |
| [Support directly sold ads](#support-directly-sold-ads) | TURTLEDOVE issue: [Publisher ad network control over ad eligibility and auction ranking](https://github.com/WICG/turtledove/issues/70)  |  |  |
| [Floor rates](#floor-rates) | Question raised in TURTLEDOVE issue: [Capabilities of the proposal for publishers](https://github.com/WICG/turtledove/issues/51).  |  |  |
| [Competitive exclusions](#competitive-exclusions) |  |  |  |
| [Weighted bidding](#weighted-bidding) | | | |

# User Needs

- [Why am I seeing this ad?](#why-am-i-seeing-this-ad)
- [Opt-out of an advertiser, category or product](#opt-out-of-an-advertiser-category-or-product)
- [Opt-out of a marketing service](#opt-out-of-a-marketing-service)
- [Seeing ads relevant to me](#seeing-ads-relevant-to-me)
- [Smooth user experience](#smooth-user-experience)

| Use-case | Chrome | Safari | Community Proposals |
|----------|--------|--------|---------------------|
|[Why am I seeing this ad?](#why-am-i-seeing-this-ad)| Both the Chrome team and the Google ads team have on multiple occassions cited a desire show people an explanation of what caused an ad to appear. The TURTLEDOVE proposal specifically aims to provide people with an accurate answer as to why they are seeing a re-targeting ad. | No support. |  |
|[Opt-out of an advertiser, category or product](#opt-out-of-an-advertiser-category-or-product)| Unclear | No support |  |
|[Opt-out of a marketing service](#opt-out-of-a-marketing-service)| Supported |  |  |
|[Seeing ads relevant to me](#seeing-ads-relevant-to-me)| Interest groups and contextual signals should help have relevant ads. |  |  |
|[Smooth user experience](#smooth-user-experience)| More details required. Concerns about the size of the data to load, store and process in-browser. |  |  |

# Impression and Viewability Measurement

Advertisers want to know how many times people saw their ads. One of the most basic metrics ad-servers report to advertisers is the total number of "impressions" served. These counts must be accurate as they are integral to reporting and billing.

There are a few critically important aspects of impression counting that advertisers care about:

- "Viewability": was the ad actually visible to the user
- "Invalid Traffic": was the ad viewed by an actual human (as opposed to a bot / script)
- "Third Party Verification": are the impression counts validated by a neutral third party that will vouch for their accuracy

## Viewability

Sometimes, an ad is returned from an ad-server, but never actually enters the viewport. Sometimes an ad might enter the viewport, but only for a few milliseconds. Sometimes an ad might enter the viewport, but fail to load any images or video before it leaves again. Sometimes there might be other elements drawn over the top of the ad. These cases illustrate "non-viewable" ad impressions. Advertisers only want to pay for "viewable ad impressions". The reasoning is simple, an ad cannot possibly have an effect on someone's future purchasing behavior if they never saw it in the first place!

Over the years, the industry has developed standards to define what constitutes a "Viewable ad impression". These are different for image and video ads, for the mobile and desktop web, and in-app. Industry bodies like the IAB and the MRC regularly audit measurement vendors (ad servers, ad verification vendors) to see if they are compliant with these standards.

## Invalid Traffic

Advertisers want to show ads to humans, not to bots and scripts. The industry has devoted a great deal of time and energy towards identifying invalid traffic (IVT), so that it can be filtered out of impression reports.

## Third-Party Verification

When a measurement vendor (ad server, publisher, etc) reports a certain number of impressions (presumably viewable impressions, with invalid traffic filtered out), how is the advertiser to know if this number is accurate? Advertisers would prefer not to just accept these numbers on faith. For this reason, the industry has established a number of independent, third-party measurement vendors that run their own verification scripts to provide advertisers with the peace of mind and confidence that the number of impressions they are being billed for are accurate, and measured in a consistent way across all of their ad buys.

# Audience Verification

Not all products are suited for all consumers. For example, in the United States sales of alcohol are restricted to 
people aged 21 and over, and there are even regulations limiting the extent to which advertisements for alcohol can be 
shown to people under 21. Thus advertisers of alcohol have not only an economic interest but also a legal obligation
to avoid advertising to teenagers.

The current ecosystem has developed elaborate mechanisms to allow advertisers to reach consumers they believe will have the interest and ability to purchase their products. While the move to a more private web will (and should) eliminate some of the current privacy-unfriendly techniques marketers use for targeting, it will not eliminate the reasonable desire (and in some cases obligation) to avoid showing irrelevant ads to uninterested or inappropriate consumers.  

There is a real cost in assembling and assessing addressable audiences who are united by some common characteristic of 
interest. As such, vendors who amass these audiences (whether those vendors are publishers or third parties) often charge a substantial premium to advertisers who wish to access these audiences.
With so much money at stake, advertisers have a strong interest in knowing whether they're actually reaching the types of people they're paying to reach. As targeting moves behind opaque mechanisms like TURTLEDOVE, there is a real risk that without the possibility of independent verification, advertisers lose so much trust in the accuracy of audience targeting mechanisms that they revert to the inefficient and annoying-to-consumers spray-and-pray tactics of the mass media era. This will both hurt consumers who will be bombarded with irrelevant or lowest-common-denominator ads, and it will hurt small publishers for whom providing advertisers access to a niche audience may be their primary economic value proposition. 
 
As with lift measurement, the conclusions marketers are aiming to draw are inherently statistical, aggregated, and 
 privacy-friendly. Advertisers are not interested in learning or retaining the demographics of any particular ad viewer -
 they only want to know that e.g. "95% of people who saw my ad were over the age of 21."
 
Some publishers have demographic information on their users and are able to report viewership
demographics based on 1st party data. But the vast majority of publishers do not know the demographics of their viewers,
even of viewers who are registered or subscribed. Thus advertisers use products like Nielsen's "Digital Ad Ratings" and
 rely on Nielsen to track ad campaigns and survey people who've seen ads to collect age and gender statistics. 
 
 While age and gender are the most commonly measured attributes, advertisers also often want to target audiences based on 
 finer-grained demographics (e.g. people who live in St. Louis) or interests (e.g. "people who play golf"). 
 
 The data used for audience measurement can come from different sources, but particularly for finer-grained attributes
 surveys often provide the most accurate (or sometimes the only) way of assessing attribute prevalence among the people
  exposed to a campaign. As is the case with [Brand Lift Measurement](#brand-lift-measurement),
   these surveys which must be conducted at some point after the advertising itself is displayed. 
   The data can neither be inferred from the rendering of the advertisement (as e.g. viewability can) nor
 can it be collected contemporaneously with the exposure. This means that the vendor conducting the surveys must have some way to know which
  advertising campaigns individuals have been exposed to in order to ask the right survey questions and to 
  properly aggregate the responses. Asynchronous 
  reporting API's may play a part in supporting the audience verification use-case but they are not themselves sufficient,
  because it's not plausible to ask every potential question about demographics and interests to every respondent. So
  surveys need to be targeted to the relevant groups of advertising-exposed respondents

# Aggregate Conversion Measurement

Advertisers need to know how many conversions happened as a result of their ad campaigns. 

## Conversion Lift Measurement

Ideally advertisers would like to know how many conversions were caused by their ad campaign (i.e. would not have happened were it not for this ad campaign). This is variously refered to as "causality" or "incrementality" measurement. The gold standard measurement approach to answer this question is “Lift Measurement”. This involves a “test group” who is shown ads, and a “control group” who is not shown ads. These groups should be randomly selected prior to running the test to ensure they are well balanced. 

The total number of raw conversion events is counted in both groups, and the difference between the two is called the “lift”. If this is a statistically significant value, and the test and control groups were selected randomly, the only explanation for the difference is the causal effect of the ads.

## Brand Lift Measurement

Much like direct response advertisers want to know how many incremental conversions were caused by their ad campaigns, 
brand advertisers want to know whether their ad campaigns succeeded in making consumers incrementally more aware of, interested in, or
favorable towards their brand, product, or service.

Because these attitudinal outcomes are not linked to a discrete observable action (like an online purchase), they typically 
must instead be measured through surveys that ask questions like "to what extent do you believe
that Volvo makes the safest cars on the market?"

Lift is measured by comparing the prevalence of a given answer among an "exposed" group of respondents who have seen a 
particular ad campaign compared to the prevalence of that same answer among a control group who have not seen the campaign.

Measuring brand lift adds substantial challenges beyond those already involved in measuring conversion lift:
 
 1. Brand advertising rarely produces clicks (or any other direct interaction with the ad unit). For example, video advertising - the [fastest growing online ad 
 format](https://www.iab.com/wp-content/uploads/2019/10/IAB-HY19-Internet-Advertising-Revenue-Report.pdf) - is almost all intended to drive brand outcomes (like awareness), not clicks. This means that any browser API that requires
 clicks to operate will not be suitable for Brand Lift Measurement (though API's that support "view-through conversions" 
 may potentially be sufficient).    
 2. Attitudes towards Toyota are fundamentally different than attitudes towards BMW, and so a Toyota 
ad campaign can only be measured by a survey that specifically asks about attitudes towards Toyota in particular. Because surveys 
are expensive to administer, it would be uneconomical to survey hundreds of thousands of consumers about their attitudes
 towards every car brand, hoping that enough of them turn out to have seen a Toyota ad. Instead, surveys must be 
kept brief and must be narrowly targeted to people who have seen *specific* ad campaigns (or are in a control group with respect
to that particular ad campaign).

The necessity of targeting specific surveys to particular respondents means that at some point, some system must select
the right survey to show to a particular person. And that decision can only be based on that person's past exposure
(or non-exposure) to particular ads. This selection may not be a privacy problem in 1st party contexts where a particular publisher running
an on-site survey just needs to record which ads a particular user has seen on that 1st party domain. (Though that itself 
might be a problem if publishers are blinded to the ads served via a system like TURTLEDOVE).

While some large publishers (e.g. Youtube) do conduct their own brand measurement surveys in a 1st party context,
advertisers are understandably reluctant to allow publishers to "grade their own homework." Also, the vast majority of 
brand campaigns are conducted on many different channels simultaneously. Conducting and analyzing surveys requires expertise
 that is unrealistic for small, independent publishers to develop and deploy on their own. Thus the large majority of                                                               brand lift measurement is conducted via 3rd party channels (either by intercepting visitors on other websites, or by
using 3rd party panels).

When surveys are conducted on a different domain than the original ad exposure, the surveying domain must have some way
to select the right survey to give based on the respondent's ad exposure profile across other domains.

Fortunately, the conclusions of attitudinal brand lift measurement are inherently statistical and aggregated, e.g. "this campaign increased awareness by 
10% among those who saw it". The responses of any individual are irrelevant beyond their contribution to a summary statistic.
 If a privacy-safe way to collect and aggregate the the survey data can be devised, measurement providers have no 
 further need to retain any information about individuals. 
 
 Brand lift measurement does have some additional privacy advantages:
 
* It is inherently opt-in, because surveys used to measure attitudes are inherently optional. Consumers
 who do not wish to have their reactions to brand advertising measured can simply choose not to respond to surveys.
* Although 3rd party brand lift measurement does require some degree of aggregation of cross-site behavior, it does
**not** require the advertiser, the publisher, or the measurement company to collect and share any personally 
identifiable information about about people whose attitudes are being measured.     
* Knowing which survey to administer to a browser does not inherently require any browser-level identification. Vendors
 only need to know the campaign or creative-level exposure attribute (which is shared by thousands or millions of 
 other browsers). I.e. vendors only need to know about membership in groups that are always very large.

## Click-through and View-through attribution heuristics

The alternative to lift measurement is to use some heuristic to “attribute” conversions to ads. The two most popular heuristics are “click-through attribution” and “view-through attribution”. “Click-through attribution” gives credit to an ad if the person had previously clicked on the ad prior to the conversion event. “View-through attribution” gives credit to an ad if the person had previously seen the ad prior to the conversion event.

Both attribution heuristics come with some concept of an “attribution window”. For example, a “one-day view-through” attribution window would only count conversions which happened within one day of an ad view. A “28-day click-through” attribution window would only count conversions which happened within 28 days of a click.

All attribution approaches are imperfect. “View-through attribution” tends to give too much credit to banner ads. “Click-through attribution” tends to give too much credit to search ads. “Instream Video Ads” (such as those shown on YouTube) are unlikely to be clicked. This does not necessarily mean they are ineffective, it just means that “click-through attribution” will likely undercount the value they drive. This effect is even more pronounced for ads shown on Smart-TVs, where it might actually be impossible to click. “View-through attribution” might be a better approach for measuring such ads.

How should one determine a good attribution methodology and attribution window to use to measure the effectiveness of an ad campaign? “Lift Measurement” should be used to calibrate the true value that was causally driven. Attribution heuristics should be chosen that most closely approximate the value measured through a wide set of “Lift Measurement” tests.

## Multi-touch attribution

In practice, advertisers run many ad campaigns simultaneously across multiple platforms. People often see (or even click) on multiple ads for a given product prior to making a purchase. When a person interacted with multiple ads, across several websites before eventually making a purchase, who should get the credit for the conversion? 

One popular approach is “Multi touch attribution”. This is a system for allocating partial credit to each of the ads the person interacted with prior to a conversion. 

The alternative is called “Last touch attribution”. This approach grants 100% of the credit for the conversion to the last ad the person clicked on prior to the conversion. This approach may give too much credit to search ads, as illustrated in the following example. 

Very often ads shown elsewhere generate interest in a product, which leads a person to do some research, including searching for the specific product they already saw an ad for. If the person clicks on a search ad they see after performing such a search, “Last touch attribution” grants 100% of the credit to this search ad, ignoring entirely the effect of the original ad that generated the interest to begin with.

How should one determine a fair allocation of credit across multiple ad interactions across multiple publishers? Ideally this would be calibrated with a cross-publisher lift test that measures the causal contribution of each publisher. The “Multi-touch attribution” methodology which most closely approximates the results of a wide set of “Lift Measurement” tests is the most fair.

## Cross Browser / Cross Device Measurement

People often make a purchase from a different browser (or even a different device) than the one where they saw an ad. A few common scenarios for this include:

- Ad shown on a Smart-TV. Person opens their phone to interact with the advertiser.
- Ad for a big-ticket item shown on a smartphone. Person completes the purchase on their laptop later after doing some price comparisons / web research. 
- Ad is shown within a mobile app. A click on the ad opens a mobile website either within a “Webview” or within the built-in mobile web browser. Person completes the purchase in the browser while the ad click is within a mobile app.
- Ad is shown within a mobile app. A click on the ad opens a different mobile app, but the person does not commit to purchasing then, as they are travelling. Later at their computer, they open a browser and further consider the product. They then switch back to their phone (where they perhaps have Apple Pay or Google Wallet set up) and purchase quickly and easily.

It is very important to provide measurement for these flows given how common they are, and how for certain platforms, they are essentially the only way conversions happen.

## Multi-Channel Attribution / Measurement
Web advertising is not the only contributor driving purchases made on an advertiser's website. Email campaigns, social media posts, call center interactions, promotions internal to the advertiser's site and other channels may also have influenced the user's purchasing decision. In order to properly allocate budgets to the channels that influence conversions, advertisers need to understand the relative importance of the various channels and how they work in sync to drive conversions. 

# Fraud Prevention

## Conversion Fraud

Fraudsters will attempt to send fraudulent conversion events. Possible motivations include:

- Disrupt the ad measurement of a competitor business
- Disrupt the ability of a publisher to provide ad metrics to advertisers
- (When the ad was served by a 3rd party ad network) appear to have a higher conversion rate in order to get higher ads revenue from the ad network

It is very important that we consider fraudsters as a part of the overall threat model and ensure that we are only counting legitimate conversions that actually happened.

## Click-Flooding

Fraudsters frequently engage in “click flooding” or “click spamming” attacks. The aim is to steal credit for conversion events that the click-source had no part in causing.

This is an intrinsic weakness of the “click-through attribution” heuristic, especially the “last click attribution” model. This is what makes it possible for a fraudster to steal credit with an accidental click, or possibly a fake click that was generated by a piece of malware. 

The best remedy is “Lift Measurement”. If a publisher is not actually causally driving any conversions, but rather just sending fake or accidental click events, this should show up as zero lift in a “Lift Measurement” test. 

We can and should do our best to find technical means of differentiating between intentional ad clicks, from real humans, who are using authentic browsers, and fake clicks generated by scripts, malware, etc.

## Malicious Browser Extensions

Fraudsters need scale to perpetrate large attacks. One of the easiest ways to reach large scale is to distribute a malicious browser extension. We should analyze each API we propose from the perspective of potential abuse by fraudsters and consider how they might use a malicious browser extension to commit ad fraud.

## Malicious Mobile Apps

Another common way that fraudsters achieve scale is by distributing malicious mobile apps. These apps may well contain webviews that can simulate web traffic. This is a common attack vector by which ad fraud is committed. We should assume that these apps will masquerade as innocuous applications and that any webview functionality within them could be entirely invisible to the end user.

## Malicious browsers

It is also feasible for fraudsters to modify the source code of a browser and compile a malicious version of it. Malicious browsers might be used within a single base of operations (for example a “click farm” with a dozen employees), or could potentially be distributed to real people in a scaled way (for example, people who accidentally downloaded the malicious software while searching for free copies of otherwise copyrighted programs.)

When designing APIs, we should assume that this is a part of the threat model as well.

# Brand Safety

Some web-pages may contain graphic imagery, or might discuss controversial topics, or include news about disaster or tragedy. Advertisers care a great deal about the types of concepts that people will associate with their brand. For this reason, advertisers might not feel comfortable having their advertisements run alongside these types of content.

For this reason, the industry has developed a lot of "Brand Safety" controls. Advertisers commonly provide ad-servers with either "whitelists" of the websites where they are happy to display their ads, or "blacklists" of websites where they are not happy to have their ads shown. Some ad-servers also support "keyword blocking", where the actual text on the page is crawled, and if certain keywords are present, the ad should not be shown anywhere on the page. 

Another important part of this story is transparency. Once an ad campaign has been delivered, **advertisers want a breakdown of all of the apps and websites where their ads were shown, so that they can review this list from the perspective of "Brand Safety".** This is important to validate that their "Brand Safety" configuration was respected, and also to give them a better idea of where their ad was actually delivered (from the perspective of the adajcent page content).

# Billing Transparency

Transparency and trust in billing data, which must be auditable and produced by an accountable party.
This reporting should be accurate for advertisers and publishers of all sizes and allow for reconciliation mechanisms between the two parties in case of mismatch.

# Targeting

Targeting is the selection of a specific audience to see an ad. There are a few types of targeting that will be particularly affected by the loss of 3rd party cookies.

## Audience definition

As an advertiser, I want to be able to build the audience for my campaign using any combination of either:
- List of users I can provide
- Socio-demo-geo criterias
- Users that have had interactions with my website or store over a certain period, or not. E.g.: "abandoned cart users", "users that have bought baby diapers between J-30 and J-15 but not since then" 

I want to be able to:
- get a understanding of the reach of my audience while defining it, and through the life of my campaign
- my audience to be updated in real-time. E.g. if my audience excludes people that recently made a purchase on my site, I want to stop displaying ads to any user making a purchase in near real time.

## Exclusion Targeting

A very common theme in user feedback about ads, is that people strongly dislike seeing ads for products they have already purchased, or mobile apps they already have installed. Such ads are not just an annoyance to people, they also drive no value for advertisers. As such, a common targeting use-case is “exclusion targeting”, which aims to prevent people from seeing ads for products / apps they already have. This is implemented in much the same way as retargeting ads, by keeping track of the browsers / devices who have fired a particular conversion event, and preventing particular ads from being shown to them.

## Lookalike Targeting

A common problem all businesses face is finding new customers. One highly effective approach is to try to find people who are similar to the business’s existing customers. These people are significantly more likely to be interested in the business’s products than a randomly selected person. This use-case is especially critical for small businesses that do not have large ad budgets, and cannot afford to spend money for a long period of time on broadly targeted ads until the system learns which type of people are most likely to engage. Small businesses need to ensure none of their limited marketing budget goes to waste. 

One common way this is achieved today is with 3rd party cookies. Before starting to run ads, a website owner can instrument their website with code that generates a conversion event each time a customer engages with their website. In this way, an ad-tech platform can learn about the aggregate properties of the current customers of the website, and utilize this information to generate a “Lookalike Audience” of people “similar” to them. Ads can then be targeted to these “similar” people, who are more likely to find this website relevant.

## Retargeting

A common practice in the industry today is to run ads that are shown to previous visitors of a website. While there is a lot of negative sentiment related to “ads that follow you around the internet”, this capability forms a significant fraction of digital advertising.

## Display and target environment

As an advertiser, I want to be able to select on which type of environment my ads are displayed (web, mobile web, app) and what's the target environment for my ads.

E.g. running a mobile web campaign that brings user to make purchase on my Apps.

# Frequency 

## Frequency Capping

People dislike highly repetitive ads. There is ample customer feedback to support this point. Not only that, advertisers do not want to spend their ad budgets showing the same ad to the same user over and over. This is the motivation for "Frequency Capping". This becomes an especially hard challenge when the advertiser is running ads across multiple websites. Without 3rd party cookies, it will be challenging to support this use-case.

There are multiple approaches to frequency capping that exist today. Some frequency caps are at the "device" level, others at the "browser" level, while others might be at the "ad platform" level, for ad-platforms which have some understanding of the user-device graph. To the extent that any of these frequency caps are intended to apply to ad impressions across multiple websites, there is a conflict with proposed privacy changes. 

Frequency caps for a given website do not seem to be in conflict with proposed privacy changes (with the possible exception of the Safari [isLoggedIn](https://github.com/WebKit/explainers/tree/master/IsLoggedIn) proposal, which might make it difficult to tie together multiple browsing sessions from the same logged-out user.)

## Frequency Optimization

While frequency capping tries to cut off the long-tail of an ad being over-delivered to a specific user, there is also the opposite side of the delivery curve where an ad is often under-delivered too.  When you look at when people convert, it's often after they've seen the ad several times.  There's typically a sort of "sweet-spot" range that corresponds to the best conversion rates.  So it can be more effective to move a user who has seen the ad a small number of times into that range by prioritizing delivery to them over users who have never seen the ad at all.

# Businesses with Multiple Domains

Some businesses operate their services across multiple domains. One common example is to utilize country-specific eTLDs, e.g. example.com for US users, example.co.uk for UK users, and example.it for Italian users. Other use-cases involve hosting user-generated content on a separate domain for security reasons, or using a separate domain for resources that are hosted on a CDN. Another use-cases is to have a separate domain for each service a company offers to people, for example itunes.com and icloud.com are both affiliate with Apple.

There are a number of problems that arise when a business operates their service across multiple domains. One example is conversion measurement. Advertisers may run an advertising campaign which directs people to example.com, but based on the IP address of the user, they may be re-directed to a country specific eTLD like example.co.uk. In such a case, the advertiser wants to count conversions that happen across all of their domains, not just those on example.com.

# Ads directing to large marketplaces

Many ads shown on publisher websites direct people to large e-Commerce websites that sell a wide variety of unrelated products. Think of Amazon, Walmart, Target, Wish, etc.

## Collaborative ads

Many producers do not sell directly to consumers via their own website. Instead, they sell their goods in stores and through large e-Commerce websites. As an example, let’s imagine a producer ShaveCo that manufactures shaving supplies. They sell these shaving supplies on e-Commerce platform MegaStore’s domain megastore.com. If they want to run ads promoting their shaving supplies, the destination of the ads would be megastore.com.
When it comes to aggregate ads measurement, ShaveCo is only interested in a tiny subset of the conversion events generated on megastore.com, just conversion events for their shaving supplies.

## Dynamic Ads

Large e-Commerce websites may have product catalogs with millions of items. They have an interesting use-case. When running ads to promote items from their vast catalogs, which product should they show?

Today, much of this is powered by data about which items a person has previously browsed.

## Email marketing

Many companies utilize email as a mechanism for re-engaging with their customers. JavaScript isn't used in email, but tracking pixels are to track things like open rates, etc.

## Real time spend management

Advertisers have very tight control over the budget they spend on the various online marketing channels they run simultaneously. They demand the ability to start / stop / increase / decrease the spend on a campaign with low latency.

This proves to be necessary for ramping-up the campaigns (budgets are increased little by little in order to minimize the risk of any campaign setup error, etc.), peak seasons (strong budget increase during sales periods) or inversely exceptional event management (E.g. shut down all travel campaigns related to a country where a catastrophe just happened), or on a more daily basis, channels and campaigns fine-tuning (increase the budgets for the campaigns best performing and vice versa).

## Catalog

Catalog management including (catalog updates, product price, product availability, geographic availability, coupon management, etc.) is a wide area of expertise of crucial importance for the advertisers.

These elements require low-latency management as well as to be user-specific. Indeed you don't want to create any frustration advertising out of stock products or showing coupons the user is not eligible to.

The travel vertical is particularly sensitive to these questions.

### Product availability management

As an Advertiser, I don't want to display ads for products that I do not have in stock, or have a low volume of.

### Price management

As an Advertiser, I want changes in my products price changes to be reflected in near real time in my ads.

I.e. I don't want to display an incorrect price in an ad.

### Products promotion management

As an Advertiser, I want to be able to push forward a set of product in my ads: i.e. during a certain period of time increase the frequency of display for these products.

This use case is common in sales period. Note that sales period can have very precise start and stop dates.

## Creatives

Advertisers want to optimize the look and feel of their ads to the audience and to the environment. This helps to drive more performance and improve their overall brand recognition.

This can be done programmatically using historical aggregated and/or personal user data, and contextual information. 

## Coupon Management

As an Advertiser I want to be able to add coupons, or any form of targeted discounts to my creatives on specific period of times, potentially for specific products.

## Product Recommendation

Large e-Commerce websites may have product catalogs with millions of items. They have an interesting use-case. When running ads to promote items from their vast catalogs, which product should they show?
Today, much of this is mainly powered by data about which items were previously browsed and by visitor's interactions with e-Commerce websites. Product definition is much greater because e-Commerce consists of diverse business models. ( e.g. For travel e-Commerce or employment portals product catalog consists of offers.)

Typical sources of recommendation models would be :
- Historical Products (compared to historical products browsed): abandoned cart, etc.
- Similar products (compared to historical products browsed)
- Complementary Products (compared to historical products browsed)
- ...

# ML Applications

There are many advertising use-cases which rely on Machine Learning today. Common themes involve selecting from a vast number of options, and making efficient use of historical data. Following are several of the key uses of Machine Learning in advertising today:

## Ad-campaign selection

Ad-campaign selection is a very important task. e.g. There are many advertising campaigns to choose from. These might be promoting different types of conversion events (e.g. some are promoting mobile app installs, some are promoting purchases on websites, some are just optimized to generate link-clicks, some are optimized to generate video-views).

## Ad-component selection

Ads are composed of multiple components (e.g. the title, description, image, video, etc.). Some ads are promoting particular products/offers. Once an ad-campaign is selected, ML is used to select which components to use in each particular ad impression. (e.g. which products to highlights, which title text to use, which image to use). These options are constrained by what the advertiser has provided as options for their ad-campaign and lastly optimized to maximize probability of reaching Advertiser's desired goal.

## Bidding logic

Machine Learning is used to estimate the efficacy of ad placements. Across an ad network, there might be millions of placements, across hundreds of thousands of publisher websites, each with different properties (like size and location on the page). How much return on investment will each placement drive for each advertiser? Machine learning is used to make such estimates.

The lower the accuracy of the prediction in bidding logic, the fewer business outcomes will be generated for the same budget. This in turn leads to lower publisher revenue.

## Formatting customization

Once the ad-campaign and components are selected, choices remain about how exactly to render that set of components (e.g. click button on the right or the left. Which animation style to choose and maximize probability of click?).

# Training ML Models

Machine learning models must be trained with sample data. They must be provided with both “positive” and “negative” training samples. There are a few key types of ML models to consider

## Click Through Rate (CTR) Model: P(click | impression)

The “CTR model” predicts the likelihood of a click given an impression. This can be done entirely with first party data. The publisher should be aware of which ads received were clicked, and which ads were not. These provide “positive” and “negative” training samples which can be used to train a machine learning model that predicts how likely someone is to click on an ad if it is shown to them.

An ad server which just utilizes a “CTR Model” will be able to optimize for the cheapest clicks. It will maximize the number of clicks for a given budget.

## Conversion Rate (CVR) Model: P(conversion | click)

Just optimizing for clicks may not lead to desirable results. Clicks are only a weak proxy for business value. Some clicks will result in conversions and business value generation, but others will not. Some clicks might be accidental, or could even be fraudulent. Advertisers do not want to pay for cheap clicks than seldom result in conversions, they want ads that actually create business value.

For this reason, another important use-case is to train “CVR Models”, that predict the likelihood of a conversion given a click. Training such a machine learning model will require “positive” and “negative” training samples as well. “Positive” samples are clicks which eventually resulted in a conversion. “Negative” samples are clicks which did not result in a conversion.

Bayesian probability tells us that we can compute the probability of a conversion given an impression by just multiplying these two predictions:

P(conversion | impression) = P(conversion | click) * P(click | impression)

## Post-view attribution: P(conversion | impression)

Post-view attribution is still widely used across the industry, particularly for branding campaigns.

## P(Display | Request)

In this competitive environment, particularly in a first-price auctions world, advertisers may want to infer the probability of winning the auction (getting the display) depending on their bid.

## Return on Ad Spend (ROAS) optimization

Not all conversions are equally valuable. There are many advertisers who sell a wide variety of items. Imagine a marketplace where some conversions generate $1 in profit, and other conversions generate $1000 in profit. Such an advertiser does not want to maximize the total number of conversions, they want to maximize their total profit. To this end, it’s desirable to try to estimate the conversion value given a conversion.

We can combine all three predictions to estimate the conversion value given an impression

E(conversion-value | impression) = E(conversion-value | conversion) * P(conversion | click) * P(click | impression)

## Multiple targets optimization (visits, registrations, sales, etc.)

Advertisers may want to track and attribute multiple natures of "conversions". Indeed many advertiser don't only focus on sales but also on visits, leads, etc.

## Audience Selling

Some actors on the internet are able to build custom audiences, revealing strong user intent.  A business model is to sell these audiences to advertisers/brands for targetting purposes.

## CRM Targeting

Relationship/CRM Marketing: a marketing strategy designed to foster customer loyalty, interaction, and long-term relationships. It is designed to develop or reinforce a strong bond with existing customers by continuing a dialogue with them that provides the customer with information that matches their needs, problems, or interests.

# Problems faced by non-logged in publishers

## Serving relevant ads

For websites that do not require a user to log-in, comparatively little (or perhaps no) information is known about the person browsing. This makes it extremely challenging to show a relevant ad. In certain cases it may be possible to show “contextual ads” that are reflective of the surrounding content, but in other cases this might not be possible.

Search engines are a very special case, where contextual ads make a lot of sense. The search term already indicates a strong interest in a specific topic. For many use-cases though, “contextual ads” will have exceptionally little contextual information to work with. 

Today, publishers rely on 3rd party ad networks to serve relevant ads. These ad networks use 3rd party cookies to connect the person viewing the publisher’s website to some other source of information they have (perhaps from a logged-in website). 

If publishers can no longer rely on 3rd party ad networks, or if those networks themselves are unable to connect the user identity to any other source of information, publishers will struggle to show relevant ads.

Irrelevant ads generally perform poorly (people do not engage with them), which will lead to reduced publisher revenues.

## Running an auction

As mentioned above, most publishers rely on multiple 3rd party ad networks to serve relevant ads. One important use-case worth considering is how a publisher is to decide which ad network should serve an ad for a given opportunity.

Today, the industry is slowly converging on “real time bidding”. This is the optimal mechanism for ensuring fairness, equal access, and equitable treatment. In such a system, each ad network is asked to “bid” on an ad opportunity. The highest bidder wins, and is given the chance to show an ad. They must pay the amount they previously bid.

Today, this type of “bidding” arrangement can either be done client-side, or server-side. Server-side has many advantages, including reduced latency and less risk of fraud. Server-side bidding requires some piece of Ad-Tech (which is responsible for conducting the auction) to request a “bid” from multiple ad servers. It must send them some kind of information about this ad opportunity so that they know how much to bid. Simply sending the contextual data (e.g. the URL of the page requesting the ad) is not enough to make a bid. It’s important to know more about the person who will see the ad, to understand what ad to serve, and how much to bid. Today this is accomplished with various approaches that are inconsistent with the Webkit / Chrome privacy models.

# Affiliate Marketing

A lot of useful content on the web only exists because Affiliate Marketing provides the financial basis to support its creation.

Publisher websites provide “affiliate links” to merchant websites. If the person clicks through and eventually completes a purchase, the publisher receives a portion of the sale price. 

There are many similarities with display ads, but the main difference is the payment scheme. Instead of being paid per impression or per click, the publisher is only paid when there is a conversion. 

Returns complicate this significantly. If someone makes a purchase, but subsequently returns the item, the publisher should not be paid for that conversion. The challenge of reversing a payout for a reported conversion is not well solved for today.

Another concern is fraud. Fraudsters may attempt to utilize “click flooding” / “click spamming” to take credit for conversions they played no part in driving. Malicious browser extensions are also a big threat here. They have the power to intercept traffic and thereby take credit for conversions they did not drive.

# Enablers for first parties 

Section on use-cases which are not strictly advertising driven but will need consideration in terms of how they can be handled in the web-platform going forward as they are key enablers for publishers, advertiser as well as users.

## Federated Single Sign-on

Across all the proposals already published and the viewpoints shared by the browsers (Privacy Models), it is quite clear that 1-st parties in general will need to build more direct/explicit consumer relations to be able to sustain advertising supported business models in the open web. Federated Sign-on as a means of scalable authorization / authentication is a key enabler to that. It servers both site owners, simplifying consumer onboarding, as well as the users in terms of ease of use, transparency, security etc.

As noted in the WebID Proposal, identity federation has been standardized outside the web platform largely by just using basic primitives (re-directs/cookies). Anticipating changed behavior/constrained use of them, there is a need for a more explicit integration while still being compatible with the existing federation protocols.

## View-through Site Personalization

When a site/brand wants to customize the content of their web pages based off of the user having seen a particular ad or opened a particular email.  This is often about a consistent, more straight-forward user experience - so if a brand shows someone a particular product/sale, and then that user shows up at their site, the user doesn't have to dig around to find out what they're interested in.  This use case might be covered by the same methods as view-through attribution - but this particular use case does require it to be available in real-time.

## Click-through Site Personalization

When someone clicks a link to a website, that URL might encode information that can be used to personalize the experience. It might be a link to a particular offer or product. It might be a link that is specific to a particular marketing campaign. It might be personalized to the person themself.

One special call out relates to "Landing Page Personalization". In the case that the website is accessed by a click on an ad, the website owner may work with the Ad-Tech provider to customize the link based on data provided by the Ad-Tech provider, the website owner or both.

## On-site Sponsored Product

Another area of digital adverting is called sponsored products. Brands launch this type of campaigns in order to obtain preferential placements for their products on retailers' or big marketplaces' websites.
Contrary to the other use cases mentioned above, the entire process from printing to ad to the actual sales happens on the retailer's website (there is no redirecting outside of the retailer's domain).

## Search

Advertisers use services like Google Shopping to advertise their products directly in Search Engines' results.

## Ad Safety

Publishers usually consider that some ads contain inappropriate imagery, promote inappropriate products or messages they don't want to be associated with.
Each publisher needs to be able to maintain a "block-list" of specific brands, categories of brands, or specific pieces of ad creative that a given publisher would not want to appear on their site.   Block lists need to be able to update quickly, and affect both contextual and interest-based ad placements.

**Publishers need the list of all of the redirecting domains of the ads that were printed on their real estate and access to the visual rendering of each ad, so that they can review this list from the perspective of "Ad Safety".** This reporting is used to investigate reported misconducts and update the block-list.

## Malvertising Protection

Some ads may contain (or link to) malware. Publishers need a mechanism to block malware from being served to their sites' users and to take appropriate action against the source of the malware.  In the event that a malvertising campaign is discovered, publishers require mechanisms to block affected ads from appearing on their sites.

Publishers use third-party malvertising detection services to prevent malvertising from serving to their users.

In the event that an ad server, DSP, or other service is compromised and may have served malware, the blocking needs to apply to bundles that may already have been cached. 

## Pausing Advertising

Marketers sometimes choose to rapidly pause their ads in all media after an adverse news event such as a product recall or  airplane crash. Publishers of any web property on which the marketer's ad might appear will need to apply the pause to any ad placement on their sites.


## Advertisers exclusion

As a Publisher, get the ability to blacklist ads from a list of advertisers.

E.g. As owner of mylocalnews.com, I don't want to have any ad from any other newspaper on my site.

## Revenue management

As a Publisher, I want to:
- Have a daily detailed and accurate reporting of advertising revenues on my properties.
- Have the ability to investigate and understand variations in my advertising revenues.
- Have the ability to reconcile and investigate discrepancies between the reporting I get on my ad revenues and the revenues that will actually come from advertisers or ad networks.

## Support Directly Sold Ads

Publishers need to be able to fulfill directly sold ad placements. Many publishers often secure higher-revenue advertising based on the ability to deliver guaranteed impressions. Publishers need to balance short term optimization of revenue with longer-term strategic client relationships, which means serving lower priced ads ahead of higher priced ads in certain cases. Being able to control this without a time delay is critical to publishers meeting their contractual and strategic goals.  Publishers can currently make direct and real-time bidding (RTB) work together using a variety of techniques. For example the highest RTB bid should not always win. Publishers need the flexibility to be able to optimize their revenue and strategic goals by controlling which ads are delivered for given placements, which rely on configurable rules that compare direct sold ads, and indirect sold ads.

## Floor Rates

Publishers need to set floor rates (a minimum rate below which an ad will not be shown, even if there is no ad at a higher rate) based on several criteria. 

 * A publisher may not want an ad to serve at all, if it is below a minimum floor set for that page, section, or site.

 * A publisher may not want an ad from a certain brand, vertical or campaign unless it is above a minimum floor set for that brand, vertical, or campaign.  For example, a publisher may set a "floor for automotive." Publishers choose to protect their direct sales business, by not allowing marketers to undercut their pricing by way of other channels.

## Competitive exclusions

Publishers need to honor contractual terms preventing competitors from appearing together. If the contextual system serves an ad for one brand in one slot on the page, the decision process in other ad slots needs to know not to serve direct competitor ads. The publisher needs to control which brands are considered competitors. (A pair of brands might be considered direct competitors in one publisher niche, but not another.)

## Weighted bidding

The bid price set by the marketer is not necessarily the price at which it should compete in the auction. Publishers need to apply their own weighted prices, not necessarily the bid price. The \"price\" of a given ad is assigned by the buyer, but that price may not be what the publisher gets paid. In this case the publisher needs to apply an adjustment for auction purposes. Adjustment must be applied by the publisher in a 1st-party context. (Related issue: [Ad Pricing · Issue #14 · WICG/turtledove](https://github.com/WICG/turtledove/issues/14)) Some examples:

 * VAST errors: If a given ad is known to have VAST errors 20% of the time (for example, does not serve) then it needs to have its effective bid price reduced by 20%, by the publisher.

 * Post-bid brand safety blocking: If a given buyer has brand-safety controls that prevent an ad from serving on a page, the publisher ultimately does not get paid. Publishers can currently adjust for this by reducing the effective bids for impressions from a given buyer.

 * Reporting discrepancies: For many reasons, buyer and seller reporting (and third parties) does not match, and publishers need to be able to reduce (or increase) the bid price based on those discrepancies to ensure the ads compete at a price that is equal to what the publisher actually gets paid.


# User needs

## Why am I seeing this ad?
One user need is to be presented with an accurate explanation of why a particular ad was shown. This is especially important in use-cases like re-targeting when people want to understand how personal information is shared. When no explanation is provided, people may imagine innacurate theories as to why they are seeing a given ad.

## Opt-out of an advertiser, category or product
One user need is the ability to opt-out of all ads from a specific business. This could be for any reason, but examples might include a dislike for that particular company / brand, a lack of relevance, excessively repetitive ads from that business in the past, a prior bad experience with that business, or offensive ad imagery. 

The compexity in serving this user need comes from the difficulty in recognizing which ads promote which businesses. A given business may run ads through many channels, through many different ad agencies, and might promote a variety of apps and websites. All of this makes it very difficult to successfully satisfy this user need.

## Opt-out of a marketing service
As a user, upon request I want to easily be able to opt-out of ads provided by a specific marketing service.

## Seeing ads relevant to me
As a user, I prefer to see ads that are relevant to me.

## Smooth user experience
As a user, I do not want:
 - Ads to degrade my browsing experience, in term of navigation, e.g.: ads blocking access to content or slowing down page load. 
 - Ads to be annoying, e.g.: get to many times ads for the same product, or ads for products I already bought.
