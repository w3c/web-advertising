# Advertising Use Cases

This document provides an overview of key advertising use cases that depend on cross-site data sharing.

- [Aggregate Conversion Measurement](#aggregate-conversion-measurement)
  - [Lift Measurement](#lift-measurement)
  - [Click-through attribution](#click-through-and-view-through-attribution-heuristics)
  - [View-through attribution](#click-through-and-view-through-attribution-heuristics)
  - [Multi-touch attribution](#multi-touch-attribution)
  - [Cross Browser / Cross Device Measurement](#cross-browser--cross-device-measurement)
- [Fraud Prevention](#fraud-prevention)
  - [Conversion Fraud](#conversion-fraud)
  - [Click Flooding](#click-flooding)
  - [Malicious Browser Extensions](#malicious-browser-extensions)
  - [Malicious Mobile Apps](#malicious-mobile-apps)
  - [Malicious Browsers](#malicious-browsers)
- [Training ML models](#training-ml-models)
  - [P(click \| impression)](#click-through-rate-ctr-model-pclick--impression)
  - [P(conversion \| click)](#conversion-rate-cvr-model-pconversion--click)
  - [Return on Ad Spend (ROAS) optimization](#return-on-ad-spend-roas-optimization)
- [Affiliate Marketing](#affiliate-marketing)
- [Targeting](#targeting)
  - [Exclusion Targeting](#exclusion-targeting)
  - [Lookalike Targeting](#lookalike-targeting)
  - [Retargeting](#retargeting)
- [Ads directing to large marketplaces](#ads-directing-to-large-marketplaces)
  - [Collaborative Ads](#collaborative-ads)
  - [Dynamic Ads](#dynamic-ads)
- [Problems faced by non-logged in publishers](#problems-faced-by-non-logged-in-publishers)
  - [Serving relevant ads](#serving-relevant-ads)
  - [Running an auction](#running-an-auction)



# Current Proposals and Browser Support

| Use-case | Chrome | Safari | Community Proposals |
|----------|--------|--------|---------------------|
| [Lift Measurement](#lift-measurement) | Discussion on this [GitHub Issue](https://github.com/csharrison/aggregate-reporting-api/issues/2) suggests that this use-case should be supported by the “[Aggregate Reporting API](https://github.com/csharrison/aggregate-reporting-api)” | No support | Facebook proposal for “[Private Lift Measurement](https://github.com/w3c/web-advertising/blob/master/private-lift-measurement-conceptual-overview.md)” |
| [Click-through attribution](#click-through-and-view-through-attribution-heuristics) | Proposal: “[Click Through Conversion-Measurement Event-level API](https://github.com/WICG/conversion-measurement-api)” | Proposal: “[Private Click Measurement API](https://github.com/WICG/ad-click-attribution)” |
| [View-through attribution](#click-through-and-view-through-attribution-heuristics) | Should be supported by the “[Aggregate Reporting API](https://github.com/csharrison/aggregate-reporting-api)” | No support | |
| [Multi-touch attribution](#multi-touch-attribution) | No support | No support | Facebook proposal for “[Privacy-Preserving Multi-Touch-Attribution and Cross-Publisher Lift Measurement](https://github.com/w3c/web-advertising/blob/master/privacy_preserving_multi_touch_attribution_and_cross_publisher_lift_measurement.md)” |
| [Cross Browser / Cross Device Measurement](#cross-browser--cross-device-measurement) | No support | No support | Facebook proposal for “[Cross Browser Anonymous Conversion Reporting](https://github.com/w3c/web-advertising/blob/master/cross-browser-anonymous-conversion-reporting.md)” |
| [Conversion Fraud](#conversion-fraud) | No solution proposed yet, but an acknowledgement that this is an important threat worth considering in a [GitHub Issue](https://github.com/WICG/conversion-measurement-api/issues/13) | Confirmation this is a use-case Safari cares about addressing. In the process of collaboratively designing a solution on this [GitHub Issue](https://github.com/WICG/ad-click-attribution/issues/27). | Facebook proposal for “[Private Fraud Prevention](https://github.com/siyengar/private-fraud-prevention)” |
| [Click Flooding](#click-flooding) | No solutions yet for this problem. However, lift measurement could be a potential way of measuring this problem. This might be supported by the “[Aggregate Reporting API](https://github.com/csharrison/aggregate-reporting-api)”. | No solutions yet for this problem. | Facebook proposal for “[Private Lift Measurement](https://github.com/w3c/web-advertising/blob/master/private-lift-measurement-conceptual-overview.md)” a potential way of measuring this problem |
| [Malicious Browser Extensions](#malicious-browser-extensions) | No solutions yet for this problem. | No solutions yet for this problem. | |
| [Malicious Mobile Apps](#malicious-mobile-apps) | No solutions yet for this problem. | No solutions yet for this problem. | |
| [Malicious Browsers](#malicious-browsers) | No solutions yet for this problem. | No solutions yet for this problem. | |
| [P(click \| impression)](#click-through-rate-ctr-model-pclick--impression) | There is no conflict with Chrome’s proposed “[Privacy Model for the Web](https://github.com/michaelkleber/privacy-model)”. Should be possible to use 1st party cookies to tie together multiple sessions from the same browser on the same website. | [isLoggedIn](https://github.com/WebKit/explainers/tree/master/IsLoggedIn) may potentially pose problems here for websites without login. Limited storage may make it hard to tie together multiple sessions from the same person. | |
| [P(conversion \| click)](#conversion-rate-cvr-model-pconversion--click) | This is a stated goal of Chrome’s “[Click Through Conversion-Measurement Event-level API](https://github.com/WICG/conversion-measurement-api)” | Not possible to train an ML model. | |
| [Return on Ad Spend (ROAS) optimization](#return-on-ad-spend-roas-optimization) | Not possible to train an ML model. However, It may be possible to use the “[Aggregate Reporting API](https://github.com/csharrison/aggregate-reporting-api)” to obtain average conversion value measurement for very coarse-grain buckets of people. This could be used to perform calibration at this coarse-grain bucketed level. | Not possible to train an ML model. | |
| [Affiliate Marketing](#affiliate-marketing) | It may be possible to use a combination of the “[Click Through Conversion-Measurement Event-level API](https://github.com/WICG/conversion-measurement-api)” and the “[Aggregate Reporting API](https://github.com/csharrison/aggregate-reporting-api)” to get a first-order estimate of the number of the number of conversions driven by an Affiliate marketing link, but questions remain about “returns” and fraud. | Extremely limited support possible with the “[Private Click Measurement API](https://github.com/WICG/ad-click-attribution)”, but questions remain about “returns” and fraud. | |
| [Exclusion Targeting](#exclusion-targeting) | Acknowledgement that this is a valuable use-case and a link to Facebook’s PETREL proposal on this [GitHub Issue](https://github.com/michaelkleber/turtledove/issues/3) | No support | Facebook proposal for “[Private Exclusion Targeting Rendered Exclusively Locally (PETREL)](https://github.com/w3c/web-advertising/blob/master/PETREL.md)” |
| [Lookalike Targeting](#lookalike-targeting) | Might be possible to achieve limited support by leveraging “[Federated Learning of Cohorts (FloC)](https://github.com/jkarlin/floc)”. | No support | |
| [Retargeting](#retargeting) | Proposal: "[Two Uncorrelated Requests, Then Locally-Executed Decision On Victory (TURTLEDOVE)](https://github.com/michaelkleber/turtledove)" | No support | |
| [Collaborative Ads](#collaborative-ads) / [Dynamic Ads](#dynamic-ads) | Some discussion in this [GitHub issue](https://github.com/WICG/conversion-measurement-api/issues/32). No solutions or even strong acknowledgement of the importance of this use-case yet. | Some discussion in this [GitHub issue](https://github.com/WICG/ad-click-attribution/issues/36). Good collaborative problem solving going on. No firm solution yet. | Facebook proposal for “[Conversion Filters](https://github.com/w3c/web-advertising/blob/master/conversion-filters.md)” |
| [Serving relevant ads (non-logged in publishers)](#serving-relevant-ads) | Proposal: “[Federated Learning of Cohorts (FloC)](https://github.com/jkarlin/floc)”. | No support | |
| [Running an auction (non-logged in publishers)](#running-an-auction) | No support | No support | Verizon / Oath [write-up of this use-case](https://github.com/w3c/web-advertising/blob/master/rtb-use-case.md) |

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
