# Privacy-Preserving Multi-Touch-Attribution and Cross-Publisher Lift Measurement

## The opportunity

Advertisers have many choices when they buy digital ads. Savvy advertisers run ads across multiple publishers, and attempt to maximize the overall efficacy of their total digital advertising budget. We believe there is an opportunity to help advertisers achieve this business need in a private way.

We believe that high quality web publishers are at risk of being under-valued by advertisers. If we can help advertisers accurately measure the true contribution that each publisher is driving for their business, we can simultaneously help advertisers make better decisions about where to allocate their digital advertising budgets, and ensure that each publisher receives their fair share. 

This in turn is good for the people who use the internet, because it ensures the websites they love receive their fair share of advertising budgets. When ads revenue flows to high-quality publishers, not those engaging in click-jacking or fraud, we can ensure we continue supporting future investment in high-quality content on the web.

## Concept #1: Path to conversion

Each conversion that occurs on an advertiser website is preceded by a history of touchpoints between that person and various advertisements that are shown across a number of different publisher websites. We will refer to this history of preceding interactions as the “path to conversion”

Here are two “paths to conversion” from “Alice” and “Bob” to make this more concrete:

![diagram showing Alice and Bob, each viewing and clicking ads across publishers A, B, and C prior to generating conversion events](https://github.com/w3c/web-advertising/blob/master/images/Path%20to%20conversion%20diagram.png)

When each publisher is only aware of the “path to conversion” on their own property, this leads to over-counting. To use the above example, this would mean:

- Publisher A takes credit for both conversions
  - Alice’s purchase is a “click-through” conversion
  - Bob’s is a “view-through” conversion
- Publisher B takes credit for both conversions as well
  - Both are “click-through” conversions
- Publisher C only takes credit for Alice’s conversion
  - Also a “click-through” conversion

The problem here is clear. The advertiser only actually sold 2 things, to 2 people, but the publishers they buy ads from are reporting a total of 4 “click-through” attributed conversions, and one “view-through” attributed conversion. 

It would be a mistake to write off the “view-through” conversions. Data from our lift studies show that ad impressions without clicks can be very important in driving purchase intent, and later click-through conversions simply would not have occurred had these previous impressions not been shown.

Based on all of this, how can we tell which publisher was actually responsible for driving these conversions? Where should the advertiser actually be investing their ad budget?

## Concept #2: Multi-Touch-Attribution (MTA)

Advertisers attempt to collect the full “path to conversion” for each purchase, and employ some methodology for deciding how to allocate credit. One common approach is “last click”. This approach awards 100% of the credit for a conversion to the last-click along that user journey. Other approaches are more complex, and often involve awarding partial credit to several publishers.

Whichever method is used to collect the “path to conversion” data, advertisers would ideally like to consider all the ad impressions and ad clicks from that person across all of their devices, not just one browser. (See our proposal for [“Cross-Browser Anonymous Conversion Reporting”](https://github.com/w3c/web-advertising/blob/master/cross-browser-anonymous-conversion-reporting.md)).

### How do advertisers obtain this “path to conversion” today?

One approach advertisers employ today, is to use “tracking tags” provided by a measurement company across all the publishers where they buy ads. These “tracking tags” rely on 3rd party cookies to tie together touchpoints from the same browser across multiple publishers. 

Another approach advertisers employ today, is to generate an “order id” or “transaction id” for each conversion on their website, and then ask each publisher to provide all of the impressions and clicks shown on that publisher that preceded the conversion with that “order id”. 

Both of these approaches are clearly in conflict with [Mozilla’s “Anti-Tracking policy](https://wiki.mozilla.org/Security/Anti_tracking_policy), [Safari’s “Tracking Prevention Policy”](https://webkit.org/tracking-prevention-policy/), and [Chrome’s proposed “Privacy Model for the Web”](https://github.com/michaelkleber/privacy-model).

Fortunately, the advertiser doesn’t actually need all of this data at this level of granularity, so there is the potential to solve this business need in a privacy-preserving way. 

## Proposal #1: Private MTA Evaluation

The advertiser’s objective is to find, and then evaluate a “multi-touch attribution model” that does a good job of approximating the true value provided by each publisher, and utilize the output of this model to assign partial credit to each publisher where they buy ads. 

There are a small set of relatively standard MTA models employed by advertisers today (e.g. last-touch, first-touch, equal-credit, time-decay). While we should begin by trying to support these approaches, we also want to try to support emerging standards that better align the allocation of credit with the incremental advertiser value generated across each publisher (see Proposal #2 below).

Assume for the moment that the advertiser has already decided upon an MTA algorithm. For the sake of argument, let’s suppose the advertiser had reason to believe that the best approach was a “time-decay” approach. Let’s imagine that for Alice’s “path to conversion”, evaluating this credit assignment function results in the following output:

- **Publisher A:** 0.12
- **Publisher B:** 0.40
- **Publisher C:** 0.48

If there was a privacy-preserving way to run this function on each conversion, across all users, then just sum up all the results, this would satisfy the business need. 

Imagine an advertiser has a total of 670 advertising driven conversions, evaluating an MTA algorithm over the granular “path to conversion” data to decide how to assign credit might result in something like this:

- **Publisher A:** 168 incremental conversions
- **Publisher B:** 429 incremental conversions
- **Publisher C:** 73 incremental conversions

If we can find a privacy-preserving way of evaluating this credit assignment function across all these conversions (and their associated “path to conversion” data), and summing the results, this would comply with the various anti-tracking / privacy-model documents referenced earlier.

## Privacy Risks

We need to consider the privacy of the output statistic as well as the evaluation. We can apply either a local or global differential privacy model to mitigate this risk. 

Given that global differential privacy generally gives greater utility, we suggest a limited set of approved functions with formal differential privacy guarantees at the function output level. This is probably viable as there are a limited number of standard MTA approaches. Perhaps these functions could take a set of weights, or tuning parameters (where we could provide sensible limits of the valid values for those weights).

In addition, if enabling arbitrary attributions functions is deemed necessary, we could employ local differential privacy on the input data, similar to that [proposed by Chrome for preserving privacy within conversion measurement](https://github.com/csharrison/conversion-measurement-api#privacy-considerations).

## Proposal #2: Private Cross-Publisher Lift

We think it’s important to help publishers determine which multi-touch-attribution function they should use. We think that the best approach is the one provided to us by science, that is controlled experimental trials. 

Previously, we shared a proposal for [“Private Lift Measurement”](https://github.com/w3c/web-advertising/blob/master/private-lift-measurement-conceptual-overview.md) with this group that provides a mechanism for measuring the “lift” driven by a single publisher or ad-network. We are now proposing an additional API for measuring “Cross publisher lift” which would help an advertiser understand the interaction effect when buying ads on two or more publishers. We think there is a place for both, and in all likelihood they will probably be used together in parallel. 

### How would “Cross Publisher Lift Measurement” work?

If the publisher is running ads across 3 channels, and they wanted to understand the interaction effects of doing so, this is the data they would need:

| Publisher A group | Publisher B group | Publisher C group | Total people in group | Total conversions from people in group | Sum of conversion value | Sum of squares of conversion value |
|-------------------|-------------------|-------------------|-----------------------|----------------------------------------|-------------------------|------------------------------------|
| Test | Test | Test | ... | ... | ... | ... |
| Test | Test | Control | | | | |
| Test | Test | Not in study | | | | |
| Test | Control | Test | | | | |
| Test | Control | Control | | | | |
| Test | Control | Not in study | | | | |
| Test | Not in study | Test | | | | |
| Test | Not in study | Control | | | | |
| Test | Not in study | Not in study | | | | |
| Control | Test | Test | | | | |
| Control | Test | Control | | | | |
| Control | Test | Not in study | | | | |
| Control | Control | Test | | | | |
| Control | Control | Control | | | | |
| Control | Control | Not in study | | | | |
| Control | Not in study | Test | | | | |
| Control | Not in study | Control | | | | |
| Control | Not in study | Not in study | | | | |
| Not in study | Test | Test | | | | |
| Not in study | Test | Control | | | | |
| Not in study | Test | Not in study | | | | |
| Not in study | Control | Test | | | | |
| Not in study | Control | Control | | | | |
| Not in study | Control | Not in study | | | | |
| Not in study | Not in study | Test | | | | |
| Not in study | Not in study | Control | | | | |
| Not in study | Not in study | Not in study | | | | |

With this table of data in hand, and with the ability to run candidate multi-touch-attribution functions over the corpus of conversion data that produced this report, one could, through trial and error, eventually converge upon a multi-touch-attribution function that does a good job of approximating the incremental conversions driven by each publisher, as evidenced by the report, including interaction effects.

Fortunately, we believe there is a simple way to generate this table of data in a private way. As you recall in our “Private Lift Measurement” proposal, publishers provide the browser with two ads, and the browser decides if the person should be assigned to the “test” or “control” group. 

The first trivial extension, is to consider that a person may or may not have ever been exposed to an ad opportunity prior to generating a conversion event (that is, their browser was never presented with two ads and asked to assign them to “test” or “control”). In this case, we consider the person as belonging to the “Not in study” group with regards to that publisher.

The second trivial extension, is to ask the advertiser to make a small change to the way they fire conversion events. When measuring N publishers with a cross-publisher lift study, instead of firing N conversion events each time a purchase is made on their website (one conversion event fired per publisher), the advertiser should only fire a single conversion event, but annotate all of the publishers being measured in this test so that the browser is aware. 

Then, with these two changes, the browser has all the necessary information to generate a private lift-measurement report. Just replace the single “is_test” bit from our previous proposal, with N key-value pairs, one per publisher, where there are only 3 possible values per publisher: “test”, “control”, and “not_in_study”.

The browser can then multicast the same anonymous lift-measurement report to each of the publishers the advertiser specified (and potentially the advertiser as well). This has the added benefit of providing 3rd party verification of the lift numbers each publisher presents to advertisers. They ought to see the exact same results reported by each of their publishers. This satisfies another business need that advertisers have.

## Privacy considerations

So with N publishers, where a person is in one of 3 states with regards to each publisher, that creates 3 ^ N cells in this table. 

If N is large, we may have a privacy problem on our hands, where an individual lift-report may contain too much entropy, and thus could potentially be tied back to the unique person who reported it. If N is 3 this should be fine, as there are only 27 groups. We actually think it doesn’t make sense to increase N beyond 3 for another reason, which is statistical power. [It’s very, very hard to measure interaction effects without huge amounts of data](https://statmodeling.stat.columbia.edu/2018/03/15/need-16-times-sample-size-estimate-interaction-estimate-main-effect/). 

If the advertiser is buying ads from more than 3 publishers, they can simply run single-publisher lift tests for the other publishers. If desired, they can sequentially rotate the set of publishers belonging to the cross-publisher lift study to understand the various interaction effects over time. We expect that this proposal, even if limited to 3, would provide sufficiently useful data for calibrating a good multi-touch-attribution model.

## A note about “publishers”

We have used the word “publisher” in the preceding section, but we want to be clear that this does not necessarily imply a single website. Our lift measurement proposal includes [a methodology for measuring the aggregate lift of an entire ad-network](https://github.com/w3c/web-advertising/blob/master/private-lift-measurement-third-party.md). This means that a limit of say 3 “publishers” in a cross-publisher lift test does not imply a limit of just 3 publisher websites. Assuming each of those 3 “publishers” was itself an entire ad network comprised of thousands or tens of thousands of websites, such a test could still enable an advertiser to reach a large portion of the web.

## Who will attempt to design multi-touch-attribution functions to match lift results?

Our assumption is that only a small minority of advertisers will have the technical expertise / inclination to go through the trial-and-error process of generating various candidate functions and comparing the results they generate to cross-publisher-lift results. Probably the most sophisticated advertisers will want to do this in-house, or hire someone to do this for them. We presume that the vast majority of advertisers will look to the publishers where they buy ads to provide this capability for them. 

Fortunately, we don’t think there is a privacy problem with enabling all parties (the advertiser, and each publisher) to either see the final lift report results, or run various multi-touch-attribution functions to get total conversion counts. 

## Implementation details

We have not provided any suggested implementation details for how much a system would enable the evaluation of functions on the total corpus of raw path-to-conversion data, aggregate the results, and provide K-anonymity guarantees. We choose not to do so, because this sounds extremely similar in nature to [Chrome’s proposed “Aggregated Reporting API”](https://github.com/csharrison/aggregate-reporting-api). We imagine that both systems would require some server-side infrastructure for querying, and each would maintain more granular data that the API would return. Both would have to ensure excellent privacy characteristics of this system, to ensure the operator(s) of such server-side infrastructure could not peek into the underlying data. We are hopeful that the cryptographers at Google working on this problem can find a solution to this problem as well.


