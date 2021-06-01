---
title: Impression and Viewability Measurement
date: 2020-06-01 10:05:00 -05:00
neededBy:
- Advertiser
- Publisher
proposedBrowserSupportFrom:
- Chrome
- Safari
- Edge
communityProposalsFrom:
- Facebook
proposalsAddressingUseCase:
- TURTLEDOVE
- PARAKEET
- PETREL
currentlyAddressed: true
authors: 
- Ben Savage (Facebook)
- Aram Zucker-Scharff (The Washington Post)
excerpt: Advertisers want to know how many times people saw their ads. One of the most basic metrics ad-servers report to advertisers is the total number of "impressions" served. 
	These counts must be accurate as they are integral to reporting and billing.
metrics:
- Viewability
- IVT
- Verification 
isBasedOn: https://github.com/w3c/web-advertising/blob/main/support_for_advertising_use_cases.md#impression-and-viewability-measurement
---
# Impression and Viewability Measurement

Advertisers want to know how many times people saw their ads. One of the most basic metrics ad-servers report to advertisers is the total number of "impressions" served. These counts must be accurate as they are integral to reporting and billing.

## What is the use case?

There are a few critically important aspects of impression counting that advertisers care about:

- "Viewability": was the ad actually visible to the user
- "Invalid Traffic" (IVT): was the ad viewed by an actual human (as opposed to a bot / script)
- "Third Party Verification": are the impression counts validated by a neutral third party that will vouch for their accuracy

### User Flows

This operates primarily in the context the use views an ad and is not concerned with the user's transition from site to site except in how it allows us to accomplish the measurement. 

### Viewability

Sometimes, an ad is returned from an ad-server, but never actually enters the viewport. Sometimes an ad might enter the viewport, but only for a few milliseconds. Sometimes an ad might enter the viewport, but fail to load any images or video before it leaves again. Sometimes there might be other elements drawn over the top of the ad. These cases illustrate "non-viewable" ad impressions. Advertisers only want to pay for "viewable ad impressions". The reasoning is simple, an ad cannot possibly have an effect on someone's future purchasing behavior if they never saw it in the first place!

Over the years, the industry has developed standards to define what constitutes a "Viewable ad impression". These are different for image and video ads, for the mobile and desktop web, and in-app. Industry bodies like the IAB and the MRC regularly audit measurement vendors (ad servers, ad verification vendors) to see if they are compliant with these standards.

### Invalid Traffic

Advertisers want to show ads to humans, not to bots and scripts. The industry has devoted a great deal of time and energy towards identifying invalid traffic (IVT), so that it can be filtered out of impression reports.

### Third-Party Verification

When a measurement vendor (ad server, publisher, etc) reports a certain number of impressions (presumably viewable impressions, with invalid traffic filtered out), how is the advertiser to know if this number is accurate? Advertisers would prefer not to just accept these numbers on faith. For this reason, the industry has established a number of independent, third-party measurement vendors that run their own verification scripts to provide advertisers with the peace of mind and confidence that the number of impressions they are being billed for are accurate, and measured in a consistent way across all of their ad buys.

## Why is it important to preserve this use case?

### Advertisers

By knowing humans have seen ads advertisers can decide if they want to pay out for an impression and if that impression has high value. Advertisers also do not want to rely on publishers to self-attest that their measurements are valid and would like to be able to bring their own technology or vendor to independently verify these metrics. 

### Publishers

Viewability and IVT are very important to publishers in order to show that their audience is real and engaged with the content and therefore the displayed ads. High viewability correlates to the ability to charge higher CPMs because it is a clear indicator of quality. Site-wide measurement of IVT allows publishers to show they are taking care to have quality audiences which also gives them the capability to raise the cost of their ad space. 

## How is it functionally achieved today?

Viewability is generally tracked in browser through three distinct methods. Different vendors may have different definitions of "viewable", with some looking for multiple seconds 100% in viewport. The generally accepted standard measure for display ads as defined by the IAB is at least one full second 50% in the viewport. Viewability measurement for other formats, [including video ads on video players](https://docs.google.com/spreadsheets/d/1kS4vtA0sGeqHRyTzwLSpmsFmN1bN6xyZj51hZYBeqrA/edit?usp=sharing), is notably without a commonly accepted measure. 

*Publisher-based* display viewability measurement is often accomplished with the use of vendors like Moat or IAS and it scans ad positions relative to the browser viewport and measures time spent in viewport. It measures viewability and then combines that viewability measurement with things like an ad ID or query to create reports on real time viewability of specific ads. It also takes viewability measures on specific sites and ad positions, averages them over time, and attaches that information as a target-able key value on the ad request. 

*Ad-system* display viewability is accomplished by scripts inserted by the ad server or SSP, often both. It is usually inserted at a privileged position of execution on the publisher site or at a high level in the ad (often a combination of both to allow cross-frame communication). The most common example of this type of viewability measurement is [Google's Active View](https://support.google.com/google-ads/answer/6085471?hl=en). It is almost entirely used for reporting and accounting of 1 to 1 measurement of ad impressions.

*Ad-based* display viewability is a measurement script or pixel that exists only within the frame-context of the ad itself and attempts to measure viewability by operating within the ad frame or by attempting to crawl out of the iFrame and is almost entirely used for reporting and accounting of 1 to 1 measurement of ad impressions.

## Proposed Support

### Browser Proposed Support  

#### Chrome

There should be no conflict with Chrome’s proposed “[Privacy Model for the Web](https://github.com/michaelkleber/privacy-model)”. First parties should still be capable of measuring that the ads displayed on their own properties entered the viewport, that images and video were loaded, how much time they spent in the viewport, etc. None of this requires joining up user identity across multiple domains. 

##### TURTLEDOVE

There is a possible complication that might arise would be with "blind rendering" as described in proposals like "[TURTLEDOVE](https://github.com/michaelkleber/turtledove)" Here, the publisher would have restricted access to the ad being rendered and we will have to discuss the feasibility of "viewability" measurement. There is a discussion [on this GitHub issue](https://github.com/csharrison/aggregate-reporting-api/issues/10).

#### Safari

This should not be in conflict with Webkit's [Tracking Prevention Policy](https://webkit.org/tracking-prevention-policy/) given the measurement is entirely within the scope of a single publisher website.

#### Edge

##### PARAKEET

[PARAKEET proposes](https://github.com/WICG/privacy-preserving-ads/blob/main/Parakeet.md#contextual-information-and-anonymization) to include contextual signals of which viewability could be one. 

### Community Proposed Support

#### Facebook

##### PETREL

A lack of [viewability could be added as an exclusion group, maybe](https://github.com/w3c/web-advertising/blob/main/PETREL.md#adding-a-user-to-an-exclusion-group)? 

## Considered Resolved By
