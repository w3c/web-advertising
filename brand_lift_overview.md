# How of Brand Lift Measurement Works (And What'll Be Required to Support It)

## Introduction

Advertisers work extensively with measurement vendors to measure
the "lift" caused by brand advertising campaigns. The deprecation of 3rd party cookies in Chrome will break several key
 pieces of the current flow and thus disable essentially all current measurement of brand lift by neutral third 
 parties on the web.

Any discussion of new browser APIs or API features to support cookieless brand lift measurement will (hopefully) be 
more fruitful when grounded in a concrete, shared understanding of how brand lift measurement currently works. 
 
To initiate and facilitate such a discussion, this document covers:
 
 1. What brand lift measurement is and why it's important to the economic health of the open web;
 2. Why brand lift measurement needs to be conducted by neutral third party vendors;
 3. How brand lift measurement currently works, with an emphasis on which parts of the process will be broken by
 the upcoming deprecation of third party cookies;
 4. Why there are no viable alternatives to the current system (and therefore why new APIs will likely be needed
 to preserve the brand lift measurement use case).
 
 This document is intended to be read after the discussion of
 brand advertising and brand lift measurement in [Advertising 101](advertising101.md) and 
 after the discussion of the brand lift measurement use case in the "[Support for
 Advertising Use Cases](support_for_advertising_use_cases.md)" document. Those documents
 more extensively explain brand advertising, brand lift measurement, and why they're 
 important to the web ecosystem. 

## Background

As discussed in the [Advertising 101](advertising101.md) document in this respository, all advertising aims to 
achieve some outcome. Common outcomes are clicks, 
online purchases, offline purchases, and increased favorability towards brands. Advertising that aims to drive immediate 
outcomes (i.e. "conversions") is referred to as "direct response" advertising, whereas advertising that aims to drive
outcomes that are delayed, indirect, or subjective is referred to as "brand advertising". (Please see "[Advertising Outcomes](advertising_outcomes.md)"
for a more complete overview of the types of outcomes advertising aims to drive).

The proposals in Google's ["Privacy Sandbox"](https://www.chromium.org/Home/chromium-privacy/privacy-sandbox) provide
advertisers with several strong options for measuring the effectiveness of direct response advertising in driving conversions. 
Facebook's proposal for "Private Lift Measurement" provides additional options for rigorous statistical measurement of
the impact of advertising on conversions.

However, no proposals currently under discussion will allow the measurement of brand advertising by neutral third parties 
(as is the current standard practice). Measuring lift from brand advertising involves several additional challenges beyond what's
required to measure lift in conversions.

### Why Brand Lift Measurement Matters

In essentially all cases, advertisers view advertising as an investment and calibrate how much money to spend
on advertising based on the size/strength of the outcomes they expect that spending to drive. Most businesses
face constant pressure to increase how efficiently they deploy capital and so marketing budgets are under constant 
pressure to justify themselves. Advertising channels where efficacy and ROI are difficult to measure face substantial
 headwinds in drawing advertising spending, even though there is no *a priori* reason to assume that easy-to-measure
 channels are actually any more effective.
 
Any channels that are made more difficult (or impossible) to measure by the the deprecation of 3rd party
 cookies will see an outflow of marketing dollars (towards channels which remain easier to measure). As a result,
 publishers who rely on newly-difficult-to-measure channels will suffer economic harm, while publishers who control
 easier-to-measure channels will benefit. Brand advertising is particularly common on premium, independent publishers
 both large (such as the New York Times) and small (such as community newspapers). If measuring brand advertising 
 ceases to be viable, independent journalism will be deeply hurt.
   
### Why Measurement Requires Third Parties

Most large brand advertising campaigns are conducted across many channels at once. It's not uncommon for a single campaign
to involve banner ads run on dozens of publishers, video ads run on dozens of different publishers, television ads,
ads run inside of native apps, and ads run on social platforms.

The brand's goal is to know how well the campaign worked overall and whether it was a judicious use of money. Knowing how
well individual channels performed is important, but it's primarily important in the context of how different channels
performed compared to each other and compared to the overall campaign. So the brand needs a measurement vendor who can measure
as much of the campaign as possible, across as many different channels as possible, and report consolidated results that
make an apples-to-apples comparision between different channels. If results between channels are incommensurable, e.g.
because there are (even slight) differences in the time period covered or the definition of the KPI's then the
results become much less useful. 

Moreover, each of these channels charges for advertising space. If the brand simply asks "Website XYZ, could you please
 tell me how effective ads I run on your website are, so I can decide whether to give you less money?" then it is understandable that the self-reported efficacy numbers the
brand receives will be viewed as less than fully reliable.

Third party measurement companies are the only ones in a position to provide neutral, reliable reporting. They can 
measure as many channels as the campaign runs on, and can align measurement so results across channels are presented in
the same units and can be directly compared (i.e. "apples-to-apples"). Moreover they have the proper incentives
to provide fair and neutral analysis - they serve as agents of the advertiser and have no incentive to favor one 
channel/publisher over another. Please the section on [1st party measurement](#1st party measurement) below for a deeper
discussion of why measurement requires third parties.

## A Worked Example

### The Parties

Suppose ShoeCo is launching a new line of custom-fitted basketball shoes, JumpJetz, designed to promote higher jumping. 

The product is brand new, and the special fittings it requires mean it can only be sold in physical stores (not online).
 JumpJetz received rave reviews in focus groups, but ShoeCo knows that no one will think to buy it at the mall unless they learn it exists and learn
about its unique benefits. So ShoeCo works with AdAgencyCo to design and conduct a massive national brand advertising
campaign introducing North America to JumpJetz.

Initial market research suggests that basketball shoes are of particular interest to people under the age of 45. So the 
ad agency recommends targeting the campaign towards news outlets focused on sports and on outlets catering to Millenials
 and GenZ. They also suggest targeting towards people who have recently read content about basketball or basketball shoes.
 
 The brand and the agency agree that because JumpJetz are brand new, the KPI for this campaign will be simply "awareness",
 i.e. the percentage of Americans who are at least aware that JumpJetz exists and are a brand of basketball shoes.
 Because the KPI (awareness) is subjective/attitudinal and does not involve any observable online behavior, they agree
 that the KPI will be measured by surveys, with "lift" being defined as an increase in self-reported awareness among 
 people who saw the ad, compared to self-reported awareness among people who did not see the ad. 
 
The agency develops a "media plan" which documents their intent to run advertising across 10 different basketball related
 publications and channels. Some of these channels charge materially more for the same number of ad impressions, so
 the brand and the agency agree that they'll make a special effort to keep track of which channels are particularly effective
 and to rebalance spending during the campaign, in line with observed ROI.
 
 At this point, the brand or the agency contracts with a **3rd party measurement vendor** such as Kantar, Nielsen, or Survata
 who help them set up and conduct measurement and will report on the ongoing and ultimate performance of each channel
  in the campaign.
  
The vendor's core responsibility is to locate and survey a sufficiently large number of "exposed" respondents (who have seen
  the JumpJetz ad campaign) and "control" respondents (who have not). They must collect a large enough "sample" to allow
   drawing valid statistical conclusions about the efficacy of the campaign. 
   Typical successful brand advertising campaigns have effect sizes in the 1-10% range,
  meaning that hundreds of exposed and control respondents are necessary to muster the "[power](https://en.wikipedia.org/wiki/Power_of_a_test)"
   to detect statistically significant
  lift. And if advertisers are interested in "slicing-and-dicing" lift by channels (as they usually are), they must collect
  a sufficient sample size for each channel individually. Thus the required overall sample size increases significantly.
  
### Challenges of Measurement
  
  Surveys are expensive to conduct. Most people don't enjoy taking surveys (and the ones who do tend to not be representative
  of the general population). Unless surveys are extremely brief (e.g. ~1 question), most survey takers must be somehow 
  remunerated to respond, and the longer surveys are the more remuneration is required. So for measurement to be economically viable, vendors must
keep surveys as short as possible and recruit only as many respondents as are strictly necessary.

Brand lfit KPI's are inherently idiosyncratic and specific to a particular campaign -- i.e. JumpJetz is interested in 
increased awareness of JumpJetz specifically, not of basketball shoes in general. So the brand lift vendor needs to locate
people who have seen a JumpJetz advertisement and give them a survey that asks questions specifically about JumpJetz.

### The Measurement Process

Different vendors have different methods of locating, surveying, and remunerating respondents but they all begin by 
integrating with the advertiser's (or sometimes publishers') ad servers to track exposure to ad campaigns by placing 
tracking pixels inside of the advertising unit (usually an iframe). When the ad renders inside a browser, the tracking pixel
makes a request to the vendor's servers, affording the vendor the opportunity to place their 3rd party cookie in the browser.
When the ad server composes the HTML of the ad, it fills in "macros" in the URL which allow passing along metadata about the
exposure, primarily "creative ID's" specifiying which creative asset was shown and "placement ID" specifying which website
the ad was shown on (and often other contextual information such as whether the ad loaded in a display vs. video slot). 
The vendor records this exposure, keyed off their own cookie ID and an identifier for the particular campaign being measured. 
If browser already has a cookie from the vendor, the vendor will increment a counter rather than dropping a new cookie.

In order to actually survey people, the vendor needs these people to later visit a different domain where the actual
surveys are being conducted (and to elect to actually take a survey). The vast, vast majority of people will never make 
their way to a survey domain and so will not actually be surveyed. 

In Survata's case, we maintain relationships with a wide network of "4th party" publishers who have agreed to host code
that produces what we call a "surveywall". Functionally similar to a paywall, the surveywall asks site visitors
 to take a short survey before proceeding. This gives the user the explicit option to voluntarily trade a small amount 
 of their time, attention, and opinions for the publisher's valuable content. 
If the user agrees to take a survey, our surveywall code checks for the presence of a Survata cookie and, if found, sends the cookie ID to our servers. We 
cross reference our tracking data to see if which (if any) ad campaigns the user's browser has been exposed to. (**This 
"check the cookie and select a survey" step is probably the single most crucial step in the entire measurement process.**
 Without our 3rd party cookie and the recorded ad exposure information attached to it, we would have no way of knowing which, if any, survey 
  is suitable to give to this respondent).

When we recognize that a user visiting our surveywall has seen the JumpJetz campaign, we give them a brief survey asking questions 
such as "When you think of basketball shoes, what brands come to mind?" or "which of the following brands of basketball shoes
are you aware of? (Nike, Reebok, JumpJetz)". Our servers record the survey answers and when the survey is complete the 
user is given access to the publisher's content. If this user hasn't seen the JumpJetz campaign, we may give them a 
different survey for a different campaign they have seen, or we may collect them as an unexposed "control" response.

Survata does not collect (nor would we want to collect) any
personally identifiable information (e.g. we do not ask for emails in our surveys). The only information we collect 
is 1) survey responses themselves and 2) the exposure history, keyed to a randomly generated cookie ID, which we only use
for survey qualification and post-hoc aggregation of results by channel. 
 
Survata charges the advertisers for measurement, and pays a portion
of the fee we collect through to the publisher for each survey response they produce. (This flow of money helps fund
a section of the web which provides content which is too valuable to be funded by advertising, but in many cases
not valuable enough to meet minimum charge size thresholds for credit card payments). 

### Providing the Results
Once we've collected enough responses for a campaign, we begin a statistical analysis of the results. 

We report our aggregated results to brands and agencies, and use the exposure metadata 
(placement and creative IDs) to break the impact down by the publisher where the ad ran, the different creatives in the 
campaign, the ad format (e.g. video vs. banner), and any other recorded attributes of interest.

Crucially, **the "end product" of brand lift measurement is inherently statistical and aggregated**. The final reporting
contain results like "Awareness increased by 4% on average among everyone who saw the ad." and "Video ads produced a 
greater increase in awareness (7%) than banner ads (3%)." The particular survey answers
and ad exposure history of particular respondents is of no interest to brands or agencies. The 
**only** reason exposure history is collected at all is so that surveys can be correctly targeted to relevant respondents,
and so that aggregate performance conclusions can be drawn (e.g. that JumpJetz ads run on ESPN were more effective than the ads run on Vox).

# Conclusion
If browsers can provide API's that allow targeting surveys to exposed/control respondents and allow post-hoc aggregations 
and analysis by channel, then measurement vendors (or at least Survata) would be more than happy to abandon any reliance
on third party cookies or cross-site tracking.

# Appendices

If, by now, you've accepted the premise that 3rd party, neutral brand lift measurement needs to be preserved then you likely
don't need to read any further.

The remainder of this document goes into detail about the consequences (to the web) of breaking brand lift measurement,
then discusses potential current alternative methods of brand lift measurement that don't require 3rd party cookies (and why those alternatives are not viable),
and then discusses potential unintended consequences whereby well-intentioned efforts to preserve user privacy might
inadvertently promote the growth of a new competing ecosystem with substantially worse privacy characteristics.

## Implications of the loss of third party cookies
 
As discussed above, measurement vendors use (their own) third party cookies to record particular browsers' histories
of exposure to the ad campaigns they're measuring. They need this history for one crucial reason - when they're
later given an opportunity to survey the users of those browsers, they need to know which survey to give to which person
(and whether that person should be counted as an "exposed" or a "control" in the statistical analysis). This step
is unfortunately indispensable to the entire third party measurement process. There is no viable way to measure the 
efficacy of most types of brand advertising without surveys, and there is currently no way to conduct the necessary
surveys without third party cookies.

In addition to "survey qualification", the exposure metadata that is transmitted along with each impression (keyed
off the cookie ID) is extremely useful for allowing advertisers to understand which parts of their campaigns are
under or over-performing. For example, it allows them to understand the relative efficacy of different channels
(e.g. one publisher vs. another, or banner ads vs. video ads).
 
If the measurement vendors' cookies are blocked by the browser (and alternative APIs are not provided), there will be 
no viable way for advertisers to measure the efficacy of their campaigns or to calculate any form of meaningful return
on advertising spending. As discussed above, channels which become unmeasurable are likely to wither as advertising
spending flees to channels that remain measurable.
  
Brand advertisers currently spends heavily on the open web, but they use other channels as well. In particular, brand advertising
also happens within "owned and operated" platforms like Facebook, as well as within mobile apps and through connected and
"[addressable](https://simpli.fi/addressable-tv-vs-addressable-ott-ctv-bringing-precision-to-addressable-advertising-in-2020/)" television. Most measurement
vendors also measure some or all of these channels in addition to measuring the web.

Without effective 3rd party brand measurement options, brands advertisers will be forced to choose between conducting unmeasurable 
advertising on the open web vs. easily measurable advertising in mobile apps, on CTV, or in walled garden social platforms. 
 Many advertisers will choose to pull their (very material) spending from the open
web and double down on apps, TV, and platforms. Journalists and others producing content for the open web are already
struggling enormously to remain economically viable. If their advertising revenue is slashed, then the only types of 
content that will remain economically to produce on the open web will be content that draws such enormous audiences that
it can survive off low, un-targeted CPM's, or content that it is narrow and specific to "high-value" contexts where contextual 
advertising is valuable. Huge
swaths of the freely available information that makes the web so wonderful do not meet either criterion and will be unlikely to survive.
 
## Potential alternatives to the current system
 
 Although using neutral third parties is the predominant method of measuring brand advertising, it is not strictly the 
 only option. This section discusses some alternatives (and explains why they do not satisfy the goal of adequately
 supporting a healthy, private web).
 
### 1st party measurement
 
 Even if third party cookies disappear, first party cookies are presumably safe. So one option is to require that the 
 domains where advertising is served also conduct (or at least host) the surveys used to measure that advertising. This 
 may sound attractive, since it supports the goal of never requiring one domain to know about activities on other domains.
 And in fact 1st party measurement does happen today -- Facebook, Twitter, YouTube (and most likely others) all 
 currently insert brand lift surveys into their content feeds and are thus able to estimate brand lift effects among people
 exposed to ad campaigns on their sites.
 
 This system has several problems:
 
 1. Platforms are only able to estimate effects within their own channels, but most brand advertising campaigns are 
 conducted across many channels simultaneously. 1st party vendors do not adhere to common standard for how effects
 are measured or reported, meaning that advertisers are left to piece together self-reported effects across multiple channels
 which are often provided in incommensurable units or over incompatible time periods.
 2. 1st party measurement is only technically feasible for very large publishers:
    1.  In-platform surveys require substantial scale. Brand advertising campaigns tend to be spread widely across
     many different publishers. It is already difficult for brand lift vendors to collect enough respondents for campaigns
     with less than ~10m impressions. Even when they can, they are often only able to get an "overall" read that consolidates 
     across publishers. If each publisher must measure itself individually, many publishers will not serve enough ad impressions
     to be able to yield enough survey responses for a meaningful read. Many smaller ad campaigns will become completely
     impossible to measure. And advertisers will start having a perverse incentive to concentrate their ad spending 
     across fewer publishers (in hopes of getting to "reportable" sample on at least some of them). Larger publishers
     are more likely than small ones to survive this consolidation.
    2. In-platform surveys require substantial repeat traffic -- a large number of people who 
    saw an ad at T0 must return to the platform (and agree to take a survey) within a sufficiently short time window 
    to allow measuring the brand effects before they wear off (many brand advertisers believe effects fade dramatically by T+30 days). 
    Repeat traffic is not a problem for high-usage sites like YouTube but is a substantial problem for sites providing 
    e.g. long-form journalism, which visitors might only visit a few times per year. 
    3. Building and operating an in-site survey system is a substantial technical challenge. 
    Facebook can afford to employ a team of engineers and survey statisticians to develop and operate a 1st party measurement system,
     but the large majority of longer-tail publishers cannot. 
 3. No matter how well-intentioned the platforms conducting 1st party measurement are, there is a **strong** business 
 incentive not to provide fair, neutral measurement (especially if publishers are relieved of any possibility of "replication"
 by neutral third parties). Advertisers are actively searching for sites with low ad performance so they can cut spending
 on those sites. It is injudicious to rely on publishers to "grade their own homework" when giving themselves a poor 
 grade **directly** leads to a substantial loss of revenue.
 
 First party measurement is useful in itself and it can and should continue to complement neutral, third party measurement. But it cannot
 replace third party measurement. Moreover, the insurmountable obstacles to replacing third party measurement are social and economic,
 not technical. Therefore, it is highly unlikely that *any* technological innovation could make first party measurement
 a self-sufficient option.
 
### Spray-and-pray surveys
 
 As discussed above, vendors use third party cookies for selecting which survey to show to which people (and for categorizing
 respondents as "exposed" vs "control"). Strictly speaking, it's not technologically necessary to select specific surveys 
 for specific people. It *is* necessary to ask questions that are idiosyncratic to the brands being measured, but in theory
 vendors could survey a large number of completely random people and ask them questions like "Have you heard of JumpJetz sneakers?"
 regardless of whether they're expected to have seen a JumpJetz ad. Vendors would still need a way of separating exposed
 vs control respondents, but this could be done by asking "opportunity to see" type questions such as "did you visit ESPN.com
 in the last 7 days".
 
 While such a "spray-and-pray" system would have excellent privacy characteristics (it would not collect any information
 at all on respondents beyond what they explicitly volunteer), it suffers from enormous economic problems.
 
 Particularly, a spray-and-pray system would have to **enormously** increase the volume of survey responses collected. Survata
 measures many ad campaigns that serve ~10 million or fewer total ad impressions. Some of these impressions go to the same devices, so
 the total number of unique devices reached might be as low as ~5 million. For a USA-only ad campaign, that would
  mean that only approximately 1.5% of the US population would have been exposed to an ad. This would mean that to collect an
  equivalent number of respondents as one would collect via targeted surveys, one would need to collect ~64 times as many respondents.
  I.e. to collect 10,000 exposed respondents, one would need to conduct 640,000 surveys (i.e. roughly the entire population of
  Portland). As discussed above, survey responses are expensive to collect so this increase in scale would multiply the cost of 
  measuring a campaign by 64 times (or probably more, because with such enormous sample sizes required, the marginal respondent
  would grow more and more expensive to acquire). These increased costs would most likely totally swamp any economic benefit advertisers could
  hope to derive from measuring their campaigns (and so they'd simply opt instead to advertise through channels which are
  economical to measure).
  
Even putting aside raw collection costs, "opportunity to see" metrics have substantial statistical problems. The primary
issues are that 1) people have poor memories and 2) many people who have the opportunity to see an ad will nevertheless
not see it *especially* when ads are highly targeted as online ads typically are. The result is that many people will 
report not having had the opportunity to see an ad even though they did (thus being incorrectly marked as a "control"),
and many people who report having had the opportunity will be incorrect or will otherwise not have seen it (thus being
incorrectly marked as an "exposed"). 

"Muddying the waters" by including many mislabled respondents has the primary effect of diluting observed effects, leading
to an understatement (potentially a substantial understatement) of the effectiveness of brand advertising on the open web.
These effects can be adjusted for *if* statisticians know the "ground truth" misclassification rates, but there is no way
to assess those rates without 3rd party cookies. Thus advertising on the open web would appear materially less effective
than advertising through other channels (e.g. apps) that are still able to accurately measure undiluted lift.

For both economic and statistical reasons, spray-and-pray surveying is not a viable alternative. If brand measurement is
to continue (which is necessary for brand advertising on the open web in general to continue) it must remain possible to
target surveys to the appropriate potential respondents.  
 
## Further technical details
 
 This document has already gotten very long, but please file an issue or contact george@survata.com if you'd like any additional
 details about any topics in this document.