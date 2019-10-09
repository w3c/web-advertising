# Browser API for Private Lift Measurement
Many browsers have already defined, or are in the process of defining new policies regarding cross-site tracking. As they take steps to curtail various types of tracking, there is also an awareness that measuring the effectiveness of ads is a legitimate use-case which should be supported.

The Safari team has published a proposal for “[Private Click Measurement](https://wicg.github.io/ad-click-attribution/index.html)” and the Chrome team has published an explainer for “[Conversion measurement](https://github.com/csharrison/conversion-measurement-api)”. These proposals have much in common. Among other similarities, both imply **“click through attribution”** as a new default conversion attribution methodology for the web.

## The downsides of click through attribution

There are known risks related to **click through attribution**. There are numerous and well documented cases of click spamming / click flooding fraud. The basic idea is simple: a publisher acting in bad faith can report as many clicks as possible in the hopes of getting credit for conversion events that were going to happen anyways.

Sometimes the clicks themselves are fraudulent (they never happened), and sometimes publishers create intentionally deceptive ad experiences intended to generate lots of accidental clicks. Either way, it results in a bad outcome: low quality (or fraudulent) publishers reporting inflated numbers of conversions to advertisers, that they did not cause.

This is bad for both people and high quality publishers. People suffer from bad ad experiences that are specifically designed to drive high click through rates. Accidentally clicking on ads is a frustrating user experience. Here are some examples of tactics that drive accidental clicks:
- Ads that pop up over the top of clickable content
- Ads that reflow the page as they load in, causing an ad to occupy the position formerly occupied by a clickable element
- Ads with huge clickable whitespace areas
- Ads with tiny close buttons that are hard to tap
- etc.

High quality publishers who do not engage in these types of tactics that drive mis-clicks suffer as well, because ads budgets start to flow away from their websites, towards sites with “higher performance” (as measured by click-through attributed conversion rates).

## How can we solve this?

We believe that the antidote is lift measurement. It’s a basic concept from science: measure a test and a control group. 

If “toasters.com” wants to measure the effectiveness of the ads they are running on some publisher (or set of publishers), here is how they can use lift to do so. People are randomly assigned to either the “Test Group” or the “Control Group”. People in the “Test Group” can see ads directing to their website and the “Control Group” sees no ads promoting their domain.

Next, we count the total number of conversions in both groups. If we observe a difference in the total number of conversions on “toasters.com”, there are only a few possible explanations:

1. It’s just **random chance**. To rule this out, we should check for statistical significance.
2. There is some kind of **bias / skew** between the individuals assigned to the “Test” versus the “Control” group. Random group assignment, combined with relatively large audiences should ensure this is not the case.  
3. This “Lift” in conversions was **caused by the ads** shown to the “Test Group”.

## Can we build new APIs to support private lift measurement?

We believe we can. The basic reason it should be possible is that lift measurement is, by design, an aggregate measurement. Lift is meaningless at an individual level.

The ultimate result of a lift study is just 4 running totals for both the test and control groups (8 numbers total):
- The total count of people in the test/control group
- The total count of conversions in the test/control group
- The total sum of the conversion value in the test/control group
- The total sum of squares of the conversion value in the test/control group (This is needed for calculating the variance to perform a t-test)

We don’t have a fully formed plan for how to achieve this type of measurement in a privacy preserving way, but we have some early ideas. We want to share these ideas in this early / raw stage in an effort to start a broader discussion about how to support this use-case with the other members of this working group.

## Limitations

Lift measurement is **not a replacement for attributed conversion measurement**, it is a complement to it. We are not suggesting that a lift API alone would be sufficient. We anticipate the future addition of other APIs (like the Safari and Chrome proposals for click-through attribution APIs) which we envision would also be used, at the same time, to measure the same ad campaigns. We have attempted to design these APIs in such a way that they **could be used in parallel** without negative privacy consequences.

What we have at this time is **just a first step**. One limitation of these two proposals we are sharing today is that **they only consider Browser => Same Browser user journeys**. For a list of other user journeys that are really important in digital advertising see: “[Common User Flows used in web advertising](https://github.com/w3c/web-advertising/blob/master/common-user-flows-in-web-advertising.md)”.

Obviously these are all crucially important flows to consider, and eventually find privacy-preserving ways of measuring. Again, these proposals are not a proposal for an end-state, they are just intended to share some ideas and get a conversation started. In the long term, (perhaps a few iterations into the future) we would love to see improved proposals that can measure all of these important user journeys.

Another issue is **fraudulent conversion reporting**. We do not touch on that at all in these proposals. Please see our work on “[Private Fraud Prevention](https://github.com/siyengar/private-fraud-prevention)” for some early thoughts about how we might address that set of concerns.

Finally, we do not address the case of the **complex first party** (e.g. [Oath](https://www.oath.com/our-brands/) owns both TechCrunch and HuffPost.). Perhaps Mike West’s proposal around “[First Party Sets](https://mikewest.github.io/first-party-sets/)” can help here.

# Proposed Solutions

We have two early-stage proposals for browser APIs which could implement *private lift measurement*. 

1. The first is for measurement on **a single first party publisher domain** 
   
   *Example: toasters.com runs ads on facebook.com and wants to know if they drove incremental conversions on their website.*
   
   See the proposal for a [Browser API for Private Lift Measurement - First Party proposal](https://github.com/w3c/web-advertising/blob/master/private-lift-measurement-first-party.md).
2. The second is for an ad network delivering ads across **multiple third party publisher domains**.
   
   *Example: toasters.com runs ads on Facebook’s ad-network “Audience Network” and wants to know if they drove incremental conversions on their website.*
   
   See the proposal for a [Browser API for Private Lift Measurement - Third Party proposal](https://github.com/w3c/web-advertising/blob/master/private-lift-measurement-third-party.md).

The **key idea** in both solutions is that the **random assignment to test or control group occurs in the browser**, rather than being controlled by the publisher or ad network. 

In both proposals, the publisher/ad-network supplies the browser with two ads. 
1. The first ad is within a study. It’s only shown to people assigned to the “Test Group”
2. The second ad is a backup, unrelated to the study, only shown to people assigned to the “Control Group”

For these proposals, we have adapted the anonymous conversion reporting approach used in Safari’s “[Private Click Measurement](https://wicg.github.io/ad-click-attribution/index.html)”, but it’s entirely possible that these 8 values could instead be measured using Chrome’s proposed “[Aggregate Reporting API](https://github.com/csharrison/aggregate-reporting-api)”. We are agnostic as to this implementation detail.

## Comparison Chart

|                                | First Party | Third Party |
| -------------------------------|:-----------:|:-----------:|
| Publisher/Ad-Network knows which ad is delivered | ✅ | ❌ |
| Publisher/Ad-Network can measure exact number of impressions of each ad served | ✅ | ❌ |
| Consistent Test/Control assignment across publishers with different eTLD+1 | ❌ | ✅ |
| Test/Control assignment need not change, even if someone clears their browser data | ❌ | ✅ |
| Publisher/Ad-Network supplies two ads, one that’s part of a study, and one that’s a “backup” | ✅ | ✅ |
| Publisher/Ad-Network does not learn anything about the activity of individual users on another site (e.g. Did they eventually produce a conversion event) | ✅ | ✅ |
| API cannot be abused to link user identity across different eTLD+1 sites | ✅ | ✅ |
| API cannot be abused to link a single user, on the same site, across an “Incognito” session and a logged-in session | ✅ | ✅ |

