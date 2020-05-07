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

When an advertiser spends money to run an ad campaign they want to answer three very basic questions about it:
- How many people saw the ad?
- How many conversions happened as a result of running this ad campaign?
- Are you confident that the ad impressions and ad conversions are from real, authentic people?

Although they sound like simple questions, in practice answering them well is rather complex and difficult undertaking. The ads industry has endeavored to find good ways of answering these basic questions, and has developed a number of approaches to doing so, which is why there are more than 3 use-cases in this section.

- [Impression and Viewability Measurement](#impression-and-viewability-measurement)
  - [Viewability](#viewability)
  - [Invalid Traffic](#invalid-traffic)
  - [Third Party Verification](#third-party-verification)
- [Aggregate Conversion Measurement](#aggregate-conversion-measurement)
  - [Lift Measurement](#lift-measurement)
  - [Click-through and View-through attribution heuristics](#click-through-and-view-through-attribution-heuristics)
  - [Multi-touch attribution](#multi-touch-attribution)
  - [Cross Browser / Cross Device Measurement](#cross-browser--cross-device-measurement)
- [Fraud Prevention](#fraud-prevention)
  - [Conversion Fraud](#conversion-fraud)
  - [Click-Flooding](#click-flooding)
  - [Malicious Browser Extensions](#malicious-browser-extensions)
  - [Malicious Mobile Apps](#malicious-mobile-apps)
  - [Malicious browsers](#malicious-browsers)
  
| Use-case | Chrome | Safari | Community Proposals |
|----------|--------|--------|---------------------|
| [Impression and Viewability Measurement](#impression-and-viewability-measurement) | There should be no conflict with Chrome’s proposed “[Privacy Model for the Web](https://github.com/michaelkleber/privacy-model)”. First parties should still be capable of measuring that the ads displayed on their own properties entered the viewport, that images and video were loaded, how much time they spent in the viewport, etc. None of this requires joining up user identity across multiple domains. The only possible complication that might arise would be with "blind rendering" as described in proposals like "[TURTLEDOVE](https://github.com/michaelkleber/turtledove)" and “[PETREL](https://github.com/w3c/web-advertising/blob/master/PETREL.md)”. Here, the publisher would have restricted access to the ad being rendered and we will have to discuss the feasibility of "viewability" measurement. There is a discussion on this [GitHub issue](https://github.com/csharrison/aggregate-reporting-api/issues/10). | Similar answer as that for Chrome. This should not be in conflict with Webkit's [Tracking Prevention Policy](https://webkit.org/tracking-prevention-policy/) given the measurement is entirely within the scope of a single publisher website. | | 
| [Lift Measurement](#lift-measurement) | A [section of the Conversion Measurement with Aggregation proposal](https://github.com/WICG/conversion-measurement-api/blob/master/AGGREGATE.md#view-through-conversions) describes explicit support for view through conversions, which should be sufficient to support lift measurement using experiments on a first party site (e.g. when using a first-party identifier to decide which experiment branch a person is in). | No support | Facebook proposal for “[Private Lift Measurement](https://github.com/w3c/web-advertising/blob/master/private-lift-measurement-conceptual-overview.md)” |
| [Click-through attribution](#click-through-and-view-through-attribution-heuristics) | Proposal: “[Click Through Conversion-Measurement Event-level API](https://github.com/WICG/conversion-measurement-api)” | Proposal: “[Private Click Measurement API](https://github.com/WICG/ad-click-attribution)” |
| [View-through attribution](#click-through-and-view-through-attribution-heuristics) | A [section of the Conversion Measurement with Aggregation proposal](https://github.com/WICG/conversion-measurement-api/blob/master/AGGREGATE.md#view-through-conversions) describes explicit support for view through conversions. | No support | |
| [Multi-touch attribution](#multi-touch-attribution) | The [Event-level API](https://github.com/WICG/conversion-measurement-api/blob/master/README.md) proposal covers last-click attribution.  A section of the [Conversion Measurement with Aggregation](https://github.com/WICG/conversion-measurement-api/blob/master/AGGREGATE.md#multi-touch-attribution-mta) proposal describes support for some built-in multi-touch models, and acknowledges that measurement providers writing their own models would be good to support if technically feasible. | No support | Facebook proposal for “[Privacy-Preserving Multi-Touch-Attribution and Cross-Publisher Lift Measurement](https://github.com/w3c/web-advertising/blob/master/privacy_preserving_multi_touch_attribution_and_cross_publisher_lift_measurement.md)” |
| [Cross Browser / Cross Device Measurement](#cross-browser--cross-device-measurement) | No support | No support | Facebook proposal for “[Cross Browser Anonymous Conversion Reporting](https://github.com/w3c/web-advertising/blob/master/cross-browser-anonymous-conversion-reporting.md)” |
| [Conversion Fraud](#conversion-fraud) | A section of the [Multi-Browser Aggregation Service Explainer](https://github.com/WICG/conversion-measurement-api/blob/master/SERVICE.md#authenticating-inputs) describes using a service that includes an authentication mechanism. This use case is also being explored for the event level API in this [GitHub Issue](https://github.com/WICG/conversion-measurement-api/issues/13). | Confirmation this is a use-case Safari cares about addressing. In the process of collaboratively designing a solution on this [GitHub Issue](https://github.com/WICG/ad-click-attribution/issues/27). | Facebook proposal for “[Private Fraud Prevention](https://github.com/siyengar/private-fraud-prevention)” |
| [Click Flooding](#click-flooding) | No solutions yet for this problem. However, lift measurement could be a potential way of measuring this problem. This might be supported by the “[Aggregate Reporting API](https://github.com/csharrison/aggregate-reporting-api)”. | No solutions yet for this problem. | Facebook proposal for “[Private Lift Measurement](https://github.com/w3c/web-advertising/blob/master/private-lift-measurement-conceptual-overview.md)” a potential way of measuring this problem |
| [Malicious Browser Extensions](#malicious-browser-extensions) | No solutions yet for this problem. | No solutions yet for this problem. | |
| [Malicious Mobile Apps](#malicious-mobile-apps) | No solutions yet for this problem. | No solutions yet for this problem. | |
| [Malicious Browsers](#malicious-browsers) | No solutions yet for this problem. | No solutions yet for this problem. | |


## Specialized Advertiser Needs

These are use-cases that only apply to a (possibly large) subset of advertisers. 

- [Targeting](#targeting)
  - [Exclusion Targeting](#exclusion-targeting)
  - [Lookalike Targeting](#lookalike-targeting)
  - [Retargeting](#retargeting)
- [Brand Safety](#brand-safety)
- [Frequency](#frequency)
  - [Frequency Capping](#frequency-capping)
  - [Frequency Optimization](#frequency-optimization)
- [Businesses with Multiple Domains](#businesses-with-multiple-domains)
- [Ads directing to large marketplaces](#ads-directing-to-large-marketplaces)
  - [Collaborative ads](#collaborative-ads)
  - [Dynamic Ads](#dynamic-ads)
  - [Email Marketing](#email-marketing)
- [Real time spend management](#real-time-spend-management)


| Use-case | Chrome | Safari | Community Proposals |
|----------|--------|--------|---------------------|
| [Exclusion Targeting](#exclusion-targeting) | Acknowledgement that this is a valuable use-case and a link to Facebook’s PETREL proposal on this [GitHub Issue](https://github.com/michaelkleber/turtledove/issues/3) | No support | Facebook proposal for “[Private Exclusion Targeting Rendered Exclusively Locally (PETREL)](https://github.com/w3c/web-advertising/blob/master/PETREL.md)” |
| [Lookalike Targeting](#lookalike-targeting) | Might be possible to achieve limited support by leveraging “[Federated Learning of Cohorts (FLoC)](https://github.com/jkarlin/floc)”.  Might require an extension to TURTLEDOVE offering a new way to create interest groups, which Chrome indicated support for in [this issue](https://github.com/michaelkleber/turtledove/issues/26). | No support | Facebook proposal for "[Privacy Preserving Lookalike Audience Targeting](privacy_preserving_lookalike_audience_targeting.md)" |
| [Retargeting](#retargeting) | Proposal: "[Two Uncorrelated Requests, Then Locally-Executed Decision On Victory (TURTLEDOVE)](https://github.com/michaelkleber/turtledove)" | No support | |
| [Brand Safety](#brand-safety) | There should be no conflict with Chrome’s proposed “[Privacy Model for the Web](https://github.com/michaelkleber/privacy-model)”. First parties should still be aware of the URL where an ad is served, and the type of content on the page. Ad requests should include this type of contextual information, so that ad-servers can select appropriate ads for that context where there are no brand-safety concerns. The only possible complication that might arise would be with Chrome's "[TURTLEDOVE)](https://github.com/michaelkleber/turtledove)" proposal. One of the two uncorrelated ad-requests will NOT have any contextual information by design. As such, the ad-server will not be able to ensure that the ad-returned is eligible to be shown on that specific webpage. However, the Chrome team understands the importance of "Brand Safety" to advertisers, and the proposal includes [a specific approach to invalidate ads client-side](https://github.com/michaelkleber/turtledove#on-device-auction), prior to rendering, specifically to support this use-case. | Similar answer as that for Chrome. This should not be in conflict with Webkit's [Tracking Prevention Policy](https://webkit.org/tracking-prevention-policy/) given the measurement is entirely within the scope of a single publisher website. | |
| [Frequency Capping](#frequency-capping) / [Frequency Optimization](#frequency-optimization) | For ads served via TURTLEDOVE interest-group targeting, the proposal offers a way to [handle frequency capping on-device](https://github.com/michaelkleber/turtledove#on-device-auction).  For other types of targeting, while it will not be possible to enforce a hard "frequency-cap" across multiple websites, it might be possible to calibrate a target average frequency model. See discussion about how to do this on the explainer for the [Aggregate Reporting API](https://github.com/csharrison/aggregate-reporting-api#advanced-example-calibrating-a-frequency-capping-model). | No support | |
| [Businesses with Multiple Domains](#businesses-with-multiple-domains) | Proposal: "[First Party Sets](https://github.com/krgovind/first-party-sets/)" | Some discussion in this [GitHub issue](https://github.com/krgovind/first-party-sets/issues/6) indicates weak support for at least the country-specific eTLD use-case, but various concerns with the current "First Party Sets" proposal. | | 
| [Collaborative Ads](#collaborative-ads) / [Dynamic Ads](#dynamic-ads) | Some discussion in this [GitHub issue](https://github.com/WICG/conversion-measurement-api/issues/32). No solutions or even strong acknowledgement of the importance of this use-case yet. | Some discussion in this [GitHub issue](https://github.com/WICG/ad-click-attribution/issues/36). Good collaborative problem solving going on. No firm solution yet. | Facebook proposal for “[Conversion Filters](https://github.com/w3c/web-advertising/blob/master/conversion-filters.md)” |
| [Email Marketing](#email-marketing) | unclear | unclear | |
| [Real time spend management](#real-time-spend-management) | High latency in TURTLEDOVE as the javascript updates have unknown frequency, and reporting is delayed in the [Aggregate Reporting API](https://github.com/csharrison/aggregate-reporting-api#advanced-example-calibrating-a-frequency-capping-model) | Supported as ads are not done in opaque iframe and bidding is not done remotely | |



# Ad Network Needs

## Core Ad Network Needs

These are core essentials ad networks need to deliver value to advertisers.

- [Training ML Models](#training-ml-models)
  - [Click Through Rate (CTR) Model: P(click | impression)](#click-through-rate-ctr-model-pclick--impression)
  - [Conversion Rate (CVR) Model: P(conversion | click)](#conversion-rate-cvr-model-pconversion--click)
  - [Return on Ad Spend (ROAS) optimization](#return-on-ad-spend-roas-optimization)

| Use-case | Chrome | Safari | Community Proposals |
|----------|--------|--------|---------------------|
| [P(click \| impression)](#click-through-rate-ctr-model-pclick--impression) | There is no conflict with Chrome’s proposed “[Privacy Model for the Web](https://github.com/michaelkleber/privacy-model)”. Should be possible to use 1st party cookies to tie together multiple sessions from the same browser on the same website. | [isLoggedIn](https://github.com/WebKit/explainers/tree/master/IsLoggedIn) may potentially pose problems here for websites without login. Limited storage may make it hard to tie together multiple sessions from the same person. | |
| [P(conversion \| click)](#conversion-rate-cvr-model-pconversion--click) | This is a stated goal of Chrome’s “[Click Through Conversion-Measurement Event-level API](https://github.com/WICG/conversion-measurement-api)” | Not possible to train an ML model. | |
| [Return on Ad Spend (ROAS) optimization](#return-on-ad-spend-roas-optimization) | Not possible to train an ML model. However, It may be possible to use the “[Aggregate Reporting API](https://github.com/csharrison/aggregate-reporting-api)” to obtain average conversion value measurement for very coarse-grain buckets of people. This could be used to perform calibration at this coarse-grain bucketed level. | Not possible to train an ML model. | |

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

| Use-case | Chrome | Safari | Community Proposals |
|----------|--------|--------|---------------------|
| [Serving relevant ads (non-logged in publishers)](#serving-relevant-ads) | Proposal: “[Federated Learning of Cohorts (FloC)](https://github.com/jkarlin/floc)”. | No support | |
| [Running an auction (non-logged in publishers)](#running-an-auction) | The TURTLEDOVE proposal is built around a server-side auction for contextually-targeted ads, and then an in-browser auction for user interest targeting.  [This GitHub issue comment](https://github.com/michaelkleber/turtledove/issues/20#issuecomment-602800377) describes the RTB flow in some more detail. | No support | Verizon / Oath [write-up of this use-case](https://github.com/w3c/web-advertising/blob/master/rtb-use-case.md) |
| [Affiliate Marketing](#affiliate-marketing) | It may be possible to use a combination of the “[Click Through Conversion-Measurement Event-level API](https://github.com/WICG/conversion-measurement-api)” and the “[Aggregate Reporting API](https://github.com/csharrison/aggregate-reporting-api)” to get a first-order estimate of the number of the number of conversions driven by an Affiliate marketing link, but questions remain about “returns” and fraud. | Extremely limited support possible with the “[Private Click Measurement API](https://github.com/WICG/ad-click-attribution)”, but questions remain about “returns” and fraud. | |
| [Federated Single Sign-on](#federated-single-sign-on) | Yes - Proposal: “[WebID](https://github.com/samuelgoto/WebID)”| Generally supportive as per [Tracking Prevention Policy](https://webkit.org/tracking-prevention-policy/) " "*We consider certain user actions, such as logging in to multiple first party websites or apps using the same account, to be implied consent to identifying the user as having the same identity in these multiple places*". Policy also acknowledges unintended impacts to federated SSO setups. General discussion in the context of the [Storage Access API](https://github.com/privacycg/storage-access) which is related but not targeted for this use-case (non-goal)| | |
| [View-through Site Personalization](#view-through-site-personalization) | No support | No support | |
| [Click-through Site Personalization](#click-through-site-personalization) | There should not be any conflict with Chrome's proposed “[Privacy Model for the Web](https://github.com/michaelkleber/privacy-model)” unless this mechanism is used to pass through high-entropy identifiers which could be used to tie user identity across websites. See PING's [Privacy Threat Model](https://w3cping.github.io/privacy-threat-model/#goal-transfer-userid). | Similar to Chrome, this should not fall afoul of Webkit's [Tracking Prevention Policy](https://webkit.org/tracking-prevention-policy/) unless high-entropy identifiers are being used to perform "navigational tracking". See PING's [Privacy Threat Model](https://w3cping.github.io/privacy-threat-model/#goal-transfer-userid). | |
|[On-site Sponsored Product](#on-site-sponsored-product)| There should not be any conflict with Chrome's proposed “[Privacy Model for the Web](https://github.com/michaelkleber/privacy-model)” | Similar answer as that for Chrome. This should not be in conflict with Webkit's Tracking Prevention Policy given the measurement is entirely within the scope of a single publisher website. | |
| [Search](#search) | unclear, but relies a lot on contextual | unclear | |

# User Needs
| Use-case | Chrome | Safari | Community Proposals |
|----------|--------|--------|---------------------|
|[Why am I seeing this ad?](#why-am-i-seeing-this-ad)||||
|[Opt-out of an advertiser, category or product](#opt-out-of-an-advertiser-category-or-product)||||
|[Opt-out of a marketing service](#opt-out-of-a-marketing-service)||||
|[Seeing ads relevant to me](#seeing-ads-relevant-to-me)||||
|[Smooth user experience](#smooth-user-experience)||||


# Current Proposals and Browser Support

| Use-case | Chrome | Safari | Community Proposals |
|----------|--------|--------|---------------------|


# Aggregate Conversion Measurement

Advertisers need to know how many conversions happened as a result of their ad campaigns. 

## Lift Measurement

Ideally advertisers would like to know how many conversions were caused by their ad campaign (i.e. would not have happened were it not for this ad campaign). This is variously refered to as "causality" or "incrementality" measurement. The gold standard measurement approach to answer this question is “Lift Measurement”. This involves a “test group” who is shown ads, and a “control group” who is not shown ads. These groups should be randomly selected prior to running the test to ensure they are well balanced. 

The total number of raw conversion events is counted in both groups, and the difference between the two is called the “lift”. If this is a statistically significant value, and the test and control groups were selected randomly, the only explanation for the difference is the causal effect of the ads.

## Click-through and View-through attribution heuristics

The alternative approach is to use some heuristic to “attribute” conversions to ads. The two most popular heuristics are “click-through attribution” and “view-through attribution”. “Click-through attribution” gives credit to an ad if the person had previously clicked on the ad prior to the conversion event. “View-through attribution” gives credit to an ad if the person had previously seen the ad prior to the conversion event.

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

# Training ML Models

Ad selection is a very important task. When there are many ads to choose from, which one should be shown to which person? The most effective approach to-date has proven to be using machine learning algorithms to assist with ad selection. This produces more relevant ads for people, and more business outcomes for advertisers. This in turn increases publisher revenues. 

Machine learning models must be trained with sample data. They must be provided with both “positive” and “negative” training samples. There are a few key types of ML models to consider

## Click Through Rate (CTR) Model: P(click | impression)

The “CTR model” predicts the likelihood of a click given an impression. This can be done entirely with first party data. The publisher should be aware of which ads received were clicked, and which ads were not. These provide “positive” and “negative” training samples which can be used to train a machine learning model that predicts how likely someone is to click on an ad if it is shown to them.

An ad server which just utilizes a “CTR Model” will be able to optimize for the cheapest clicks. It will maximize the number of clicks for a given budget.

## Conversion Rate (CVR) Model: P(conversion | click)

Just optimizing for clicks may not lead to desirable results. Clicks are only a weak proxy for business value. Some clicks will result in conversions and business value generation, but others will not. Some clicks might be accidental, or could even be fraudulent. Advertisers do not want to pay for cheap clicks than seldom result in conversions, they want ads that actually create business value.

For this reason, another important use-case is to train “CVR Models”, that predict the likelihood of a conversion given a click. Training such a machine learning model will require “positive” and “negative” training samples as well. “Positive” samples are clicks which eventually resulted in a conversion. “Negative” samples are clicks which did not result in a conversion.

Bayesian probability tells us that we can compute the probability of a conversion given an impression by just multiplying these two predictions:

P(conversion | impression) = P(conversion | click) * P(click | impression)

## Return on Ad Spend (ROAS) optimization

Not all conversions are equally valuable. There are many advertisers who sell a wide variety of items. Imagine a marketplace where some conversions generate $1 in profit, and other conversions generate $1000 in profit. Such an advertiser does not want to maximize the total number of conversions, they want to maximize their total profit. To this end, it’s desirable to try to estimate the conversion value given a conversion.

We can combine all three predictions to estimate the conversion value given an impression

E(conversion-value | impression) = E(conversion-value | conversion) * P(conversion | click) * P(click | impression)

# Affiliate Marketing

A lot of useful content on the web only exists because Affiliate Marketing provides the financial basis to support its creation.

Publisher websites provide “affiliate links” to merchant websites. If the person clicks through and eventually completes a purchase, the publisher receives a portion of the sale price. 

There are many similarities with display ads, but the main difference is the payment scheme. Instead of being paid per impression or per click, the publisher is only paid when there is a conversion. 

Returns complicate this significantly. If someone makes a purchase, but subsequently returns the item, the publisher should not be paid for that conversion. The challenge of reversing a payout for a reported conversion is not well solved for today.

Another concern is fraud. Fraudsters may attempt to utilize “click flooding” / “click spamming” to take credit for conversions they played no part in driving. Malicious browser extensions are also a big threat here. They have the power to intercept traffic and thereby take credit for conversions they did not drive.

# Targeting

Targeting is the selection of a specific audience to see an ad. There are a few types of targeting that will be particularly affected by the loss of 3rd party cookies.

## Exclusion Targeting

A very common theme in user feedback about ads, is that people strongly dislike seeing ads for products they have already purchased, or mobile apps they already have installed. Such ads are not just an annoyance to people, they also drive no value for advertisers. As such, a common targeting use-case is “exclusion targeting”, which aims to prevent people from seeing ads for products / apps they already have. This is implemented in much the same way as retargeting ads, by keeping track of the browsers / devices who have fired a particular conversion event, and preventing particular ads from being shown to them.

## Lookalike Targeting

A common problem all businesses face is finding new customers. One highly effective approach is to try to find people who are similar to the business’s existing customers. These people are significantly more likely to be interested in the business’s products than a randomly selected person. This use-case is especially critical for small businesses that do not have large ad budgets, and cannot afford to spend money for a long period of time on broadly targeted ads until the system learns which type of people are most likely to engage. Small businesses need to ensure none of their limited marketing budget goes to waste. 

One common way this is achieved today is with 3rd party cookies. Before starting to run ads, a website owner can instrument their website with code that generates a conversion event each time a customer engages with their website. In this way, an ad-tech platform can learn about the aggregate properties of the current customers of the website, and utilize this information to generate a “Lookalike Audience” of people “similar” to them. Ads can then be targeted to these “similar” people, who are more likely to find this website relevant.

## Retargeting

A common practice in the industry today is to run ads that are shown to previous visitors of a website. While there is a lot of negative sentiment related to “ads that follow you around the internet”, this capability forms a significant fraction of digital advertising.

# Impression and Viewability Measurement

Advertisers want to know how many times people saw their ads. One of the most basic metrics ad-servers report to advertisers is the total number of "impressions" served. These counts must be accurate as they are integral to reporting and billing.

There are a few critically important aspects of impression counting that advertisers care about:

- "Viewability": was the ad actually visible to the user
- "Invalid Traffic": was the ad viewed by an actual human (as opposed to a bot / script)
- "Third Party Verification": are the impression counts validated by a neutral third party that will vouch for their accuracy

## Viewability

Sometimes, an ad is returned from an ad-server, but never actually enters the viewport. Sometimes an ad might enter the viewport, but only for a few milliseconds. Sometimes an ad might enter the viewport, but fail to load any images or video before it leaves again. Sometimes there might be other elements drawn over the top of the ad. These cases illustrate "non-viewable" ad impressions. Advertisers only want to pay for "viewable ad impressions". The reasoning is simple, an ad cannot possibly have an effect on someone's future purchasing behavior if they never saw it in the first place!

Over the years, the industry has developed standards to define what consititues a "Viewable ad impression". These are different for image and video ads, for the mobile and desktop web, and in-app. Industry bodies like the IAB and the MRC regularly audit measurement vendors (ad servers, ad verification vendors) to see if they are compliant with these standards.

## Invalid Traffic

Advertisers want to show ads to humans, not to bots and scripts. The industry has devoted a great deal of time and energy towards identifying invalid traffic (IVT), so that it can be filtered out of impression reports.

## Third-Party Verification

When a measurement vendor (ad server, publisher, etc) reports a certain number of impressions (presumably viewable impressions, with invalid traffic filtered out), how is the advertiser to know if this number is accurate? Advertisers would prefer not to just accept these numbers on faith. For this reason, the industry has established a number of independent, third-party measurement vendors that run their own verification scripts to provide advertisers with the peace of mind and confidence that the number of impressions they are being billed for are accurate, and measured in a consistent way across all of their ad buys.

# Brand Safety

Some web-pages may contain graphic imagery, or might discuss controversial topics, or include news about disaster or tragedy. Advertisers care a great deal about the types of concepts that people will associate with their brand. For this reason, advertisers might not feel comfortable having their advertisements run alongside these types of content.

For this reason, the industry has developed a lot of "Brand Safety" controls. Advertisers commonly provide ad-servers with either "whitelists" of the websites where they are happy to display their ads, or "blacklists" of websites where they are not happy to have their ads shown. Some ad-servers also support "keyword blocking", where the actual text on the page is crawled, and if certain keywords are present, the ad should not be shown anywhere on the page. 

Another important part of this story is transparency. Once an ad campaign has been delivered, advertisers want a breakdown of all of the apps and websites where their ads were shown, so that they can review this list from the perspective of "Brand Safety". This is important to validate that their "Brand Safety" configuration was respected, and also to give them a better idea of where their ad was actually delivered (from the perspective of the adajcent content on the page).

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

## Email marketing

Many companies utilize email as a mechanism for re-engaging with their customers. JavaScript isn't used in email, but tracking pixels are to track things like open rates, etc.

## Real time spend management

Advertisers have very tight control over the budget they spend on the various online marketing channels they run simultaneously. They demand the ability to start / stop / increase / decrease the spend on a campaign with low latency.

This proves to be necessary for ramping-up the campaigns (budgets are increased little by little in order to minimize the risk of any campaign setup error, etc.), peak seasons (strong budget increase during sales periods) or inversely exceptional event management (E.g. shut down all travel campaigns related to a country where a catastrophe just happened), or on a more daily basis, channels and campaigns fine-tuning (increase the budgets for the campaigns best performing and vice versa).

## On-site Sponsored Product

Another area of digital adverting is called sponsored products. Brands launch this type of campaigns in order to obtain preferential placements for their products on retailers' or big marketplaces' websites.
Contrary to the other use cases mentioned above, the entire process from printing to ad to the actual sales happens on the retailer's website (there is no redirecting outside of the retailer's domain).

## Search

Advertisers use services like Google Shopping to advertise their products directly in Search Engines' results.

## Audience Selling

Some actors on the internet are able to build custom audiences, revealing strong user intent.  A business model is to sell these audiences to advertisers/brands for targetting purposes.

## CRM Targeting

Relationship/CRM Marketing: a marketing strategy designed to foster customer loyalty, interaction, and long-term relationships. It is designed to develop or reinforce a strong bond with existing customers by continuing a dialogue with them that provides the customer with information that matches their needs, problems, or interests

# User needs
## Why am I seeing this ad?
As a user, upon request I want a clear answer on why am I seeing a particular ad.
## Opt-out of an advertiser, category or product
As a user, upon request I want to easily be able to opt-out of ads related to an advertiser, a category of products or a specific product.
## Opt-out of a marketing service
As a user, upon request I want to easily be able to opt-out of ads provided by a specific marketing service.
## Seeing ads relevant to me
As a user, I prefer to see ads that are relevant to me.
## Smooth user experience
As a user, I do not want:
 - Ads to degrade my browsing experience, in term of navigation, e.g.: ads blocking access to content or slowing down page load. 
 - Ads to be annoying, e.g.: get to many times ads for the same product, or ads for products I already bought.

