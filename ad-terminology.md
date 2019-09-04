#TARGETING:

A set of restrictions upon the set of people who are eligible to see a given ad. These are supplied by the advertiser in an effort to avoid showing ads to people for whom the ad would be irrelevant, and thus wasted spend.

Examples:
- ONLY show this ad to people over the age of 21
- DO NOT show this ad to people who already bought this item
- ONLY show this ad to iOS users
- ONLY show this ad to people who are within 50km of this store
- ONLY show this ad to people who live in the EU

#RANKING:

There are frequently over 10,000 ads eligible to show a given person (i.e. where the person meets the targeting restrictions). Ranking is the process of deciding which of these ads to show.

#THE AUCTION:

Auctions provide a highly efficient mechanism for allocating resources. Facebook operates an internal "auction" to determine which of the tens of thousands of ads to show to a given person. Each advertiser "bids" on the opportunity to show their ad to the person, and the highest bidder wins the ad opportunity. Facebook runs a "second price auction", meaning that the winner only needs to pay the price of the second highest bidder. These "bids" are generated internally by Facebook on behalf of the advertiser. They are not shared externally. While advertisers can set a "maximum bid", most do not, and rely on Facebook to make good choices on their behalf.

#OPTIMIZATION GOAL:

This is the measurable objective the advertiser would like to maximize.

Examples:
- Link Clicks: Get as many clicks as possible
- Website Conversions: Get as many of a particular valuable event to happen as possible. Advertiser must provide data each time this event happens.
- Mobile App Installs: Get as many people to install a mobile app as possible. Advertiser must report each time a mobile app activation (first time opened) happens
- Mobile App Events: Get as many of a particular action taken within a mobile app to happen as possible. Advertiser must provide data each time this event happens.

#ATTRIBUTION WINDOW:

A set of criteria an advertiser can select to determine which conversions ought to be attributed to a given campaign.

Examples:
- 1 day click-through only. Only attribute conversions which occur within 24 hours of a click.
- 28 day click-through + 1 day view through. Attributed conversions which occur with 28 days of a click, or 1 day of a view.
- 7 day click-through + 7 day view-through.


#ATTRIBUTED CONVERSION:

A conversion which is considered to have been "caused" by an advertising campaign according to the attribution window configured by the advertiser. All attribution methodologies suffer from a variety of flaws. Most importantly, attributed conversions are frequently not a good measure of *incremental* attributions. See “Lift Measurement” below. See “Data Driven Attribution” as well.


#ORGANIC CONVERSION:

Any conversions which can not be attributed to advertising campaigns are deemed "organic" conversions.

#TOTAL CONVERSIONS:

The total number of conversions to occur, regardless of attribution window settings.

#AVERAGE COST PER OPTIMIZED CONVERSION:
This is the total budget spent on an advertising campaign, divided by the number of attributed conversions attributed to said campaign.

Examples:
- If $100 are spent on an advertising campaign optimizing for "Website conversions", which generates 20 of this particular outcome, the average cost per optimized conversion would be $5.00

#MARGINAL COST PER OPTIMIZED CONVERSION:
The cost of acquiring the *next* optimized conversion event. This value increases as the total number of conversions increases.

Examples:
- The $100 budget campaign which generated 20 website conversions, might have a "marginal cost" of $7, meaning that the advertiser would need to spend $7 more dollars to acquire the next conversion.
- <<Show marginal cost curve>> y = sqrt(x)

#MARGINAL PROFIT:
The profit an advertiser makes from the "next" optimized conversion event. This includes all costs, including the cost of producing goods, as well as the costs of advertising.

Examples:
- It costs $300.00 to manufacture a drone (including buying raw materials, paying rent, electricity, salaries, taxes, etc. If the marginal cost per optimized conversion is $120 (that is, the cost of acquiring the next drone sale via advertising is $120), and the sale price of the drone is $500, then the marginal profit would be $80.
- A rational advertiser would stop spending at the point that their marginal profit drops to $0.00


AD IMPRESSION:
When an ad is shown to a person. There are various standards (e.g. MRC) for what constitutes an "impression".

Examples:
- Perhaps 75% of the ad pixels must be in the viewport for at least 2 seconds.
- For video ads, perhaps 2 continuous seconds of the video must be played, and must be at least 50% in the viewport during that time.
REACH:
The "reach" of an advertising campaign is the total number of *unique* people who ever are shown an ad impression from this campaign.

#AD CLICK:
Any click, anywhere on the ad, including the "like" button, the "report" button, or even whitespace.

#LINK CLICK:
A click on an ad which results in a navigation to the advertiser specified destination (be it a webpage, or an app store, or a mobile app via a deep link)

#AD OPPORTUNITY:
When a person is shown an ad, and a particular ad is eligible to be shown to them (e.g. they match the targeting rules), and this ad is *considered* at the time of ad selection. Even if the ad is eventually not shown for some reason (perhaps the person is in the control group of a lift study, or another ad has a higher bid) there was still an "opportunity"

#LIFT TEST:
A lift test, is a test versus control experiment, where members of the control group are not eligible to see ads from a given campaign. The raw (pre-attribution) count of conversions is measured for both the test and control groups and compared.

Example:
- Imagine a lift test, with a 20% control group.
- Imagine that the advertiser targets an entire city, population 10 million. So there are 8 million people in the test group who are eligible to see the ad, and 2 million people in the control group who are not eligible to see the ad.
- Unless the campaign has a very, very large budget, and runs a very, very long time, it's not going to reach all 8 million people in the test group. Let's imagine that only 5 million people in the test group ever actually see the ad.
- There is no counterfactual group of people in the control group that can be compared to the "reached" group.
- There *is* a counterfactual group if we consider opportunity logging. Let's imagine that 6 million people in the test group had an "ad opportunity" logged, while 1.5 million people in the control group had an "ad opportunity" logged.
- We can count the total number of conversions from this set of 6 million people who were in the test group, and compare it with the total number of conversions from the set of 1.5 million people in the control group who also had an opportunity. Let's imagine that there were 1 million conversions in the test group, and 200,000 conversions in the control group. This is regardless of any kind of attribution rules.
- These are not equal sized groups (we chose a 20% control group, not a 50% control group) so we need to scale things up to make them comparable. As such, we need to scale up the control group by a factor of 4.
- The scaled conversions are now 1 million, versus 800,000.
- The "lift" is 200,000 conversions. As the test and control group were randomly selected prior to the campaign starting to run, the only possible explanation for this "lift" is either random variation, or that the ads causally produced this uplift in the number of conversions.
- Facebook calculates the probability that such a difference could be observed purely due to random chance (the p-score) and shares this with the advertiser. If the chance is very small - the lift is deemed "statistically significant".

#BALANCED LIFT TEST:

Facebook constantly compares the composition of people in the test and control groups who had an "ad opportunity" to look for systematic bias. Given that the groups were selected randomly, before the campaign began, there ought not be any difference between these populations across any axis (e.g. age, gender, operating system, etc.) however, by introducing the idea of "opportunity logging" its possible that feedback loops exist which could cause the groups to go out of balance (e.g. the test group has 60% men, while the control group has 40% men). Any time such a discrepancy is observed, Facebook takes action to understand the root cause of this imbalance, and will fix whatever bugs, or feedback loops led to this imbalance. For the lift test results to be considered useful, the groups must be well balanced.

#LIFT DILUTION:

In the previous example, the lift of 200,000 conversions was computed by comparing the group of 6 million people in the test group who had an ad opportunity with the 1.5 million people in the control group who had an impression. This lift is "diluted", because only 5 million people in the test group actually ever saw an ad. As such, this lift of 200,000 would have been ever larger had we beed able to compare just the group of 5 million people who were reached with an equivalent set of people in the control group. Facebook continues to try to push "opportunity logging" later and later into our multi-stage ranking system to reduce the amount of lift dilution. However, advertisers still need to be careful. If they target, for example, the entire United States, with a budget of $100,000, they will likely experience a very large amount of lift dilution, probably having many, many more people having had an "opportunity" than were actually reached. As a result, running of lift tests usually requires careful consideration of the targeting spec, the budget, and the schedule of an ad campaign.

#DATA-DRIVEN ATTRIBUTION:

An attribution methodology which is found to best reflect the number of "incremental" conversions as measured by lift tests across a very large corpus of lift tests.

#ESTIMATED LIKELIHOOD OF A CONVERSION:

How does "ranking" work? How does Facebook generate "bids" on behalf of advertisers in the internal auction that it runs to decide which of the tens of thousands of eligible ads to show to people? Facebook generates a prediction of the likelihood of a conversion given an impression. It then uses this prediction to scale the advertiser bid.

Example:
- If an advertiser is willing to pay $100 per website conversion, and Facebook predicts that showing an ad to person A has a 1% chance of leading to a conversion, Facebook will bid 0.01 * $100 = $1.00
- If another person is estimated to be just 1/10th as likely to convert (i.e. has a 0.1% chance of an impression resulting in a conversion), Facebook will bid 1/10th as much: 0.001 * $100 = $0.10

#CALIBRATION:

The ads ranking system is "well calibrated" if the total number of estimated conversions is roughly equal to the actual number of attributed optimized conversions. If the system is poorly calibrated, this leads to poor advertiser value, with a systematic bias either bidding too high, or too low. A well calibrated system maximized value created.

| Onsite User Features |             |              |                                    |      | Offsite User Features        |      | Onsite Ad Features             |               |      | Offsite Ad Features                  |      | Label                                     |
|----------------------|-------------|--------------|------------------------------------|------|------------------------------|------|--------------------------------|---------------|------|--------------------------------------|------|-------------------------------------------|
| User age             | User gender | User country | User historical click through rate | etc. | Frequently Accessed Websites | etc. | Ad average CTR in this country | Ad x-out rate | etc. | Ad average conversion rate (offsite) | etc. | Did this impression lead to a conversion? |
| 25-34                | M           | USA          | 0.01                               |      | <64-float embedding vector>  |      | 0.02                           | 0.001         |      | 0.0058                               |      | 1                                         |
| 35-44                | F           | UK           | 0.04                               |      | <64-float embedding vector>  |      | 0.05                           | 0.002         |      | 0.0072                               |      | 0                                         |
| 35-44                | F           | USA          | 0.02                               |      | <64-float embedding vector>  |      | 0.02                           | 0.001         |      | 0.0091                               |      | 0                                         |
| 45-54                | M           | CA           | 0.01                               |      | <64-float embedding vector>  |      | 0.03                           | 0.003         |      | 0.0037                               |      | 1                                         |
| 13-24                | M           | IN           | 0.03                               |      | <64-float embedding vector>  |      | 0.1                            | 0.001         |      | 0.0094                               |      | 0                                         |
| 25-35                | F           | ID           | 0.01                               |      | <64-float embedding vector>  |      | 0.08                           | 0.0001        |      | 0.0066                               |      | 1                                         |
| 35-44                | M           | IN           | 0.02                               |      | <64-float embedding vector>  |      | 0.1                            | 0.0002        |      | 0.0018                               |      | 0                                         |
| 35-44                | M           | USA          | 0.05                               |      | <64-float embedding vector>  |      | 0.02                           | 0.0003        |      | 0.0025                               |      | 0                                         |
| 45-55                | F           | IE           | 0.01                               |      | <64-float embedding vector>  |      | 0.06                           | 0.0001        |      | 0.0058                               |      | 1                                         |
| 13-25                | F           | USA          | 0.02                               |      | <64-float embedding vector>  |      | 0.02                           | 0.002         |      | 0.0072                               |      | 1                                         |
| 25-36                | M           | UK           | 0.04                               |      | <64-float embedding vector>  |      | 0.05                           | 0.002         |      | 0.0091                               |      | 0                                         |
| 35-44                | M           | USA          | 0.029636364                        |      | <64-float embedding vector>  |      | 0.02                           | 0.0007        |      | 0.0037                               |      | 1                                         |
| 35-44                | F           | CA           | 0.030636364                        |      | <64-float embedding vector>  |      | 0.03                           | 0.001         |      | 0.0094                               |      | 0                                         |
| 45-56                | F           | IN           | 0.031636364                        |      | <64-float embedding vector>  |      | 0.1                            | 0.002         |      | 0.0066                               |      | 0                                         |
| 13-26                | M           | ID           | 0.032636364                        |      | <64-float embedding vector>  |      | 0.08                           | 0.001         |      | 0.0018                               |      | 1                                         |
| 25-37                | M           | IN           | 0.033636364                        |      | <64-float embedding vector>  |      | 0.1                            | 0.003         |      | 0.0025                               |      | 0                                         |
| 35-44                | F           | USA          | 0.034636364                        |      | <64-float embedding vector>  |      | 0.02                           | 0.001         |      | 0.0061                               |      | 1                                         |


#FEATURES and LABELS:

When training an ML model, there is an important distinction between "features" and "labels". Features are inputs to the final prediction system. They are also available at model training time. When training a model, the system looks for correlations between these input features, and the actual outcome - or "label". Each row of training data has a "label". These are usually a binary 0 or 1 indicating that there either was or was not a conversion.

#USER FEATURES:

These are features that describe something about the person who is viewing the ad.

#AD FEATURES:

These are features that describe something about the ad itself.

#ONSITE FEATURES:

Onsite features are pieces of information that are available at the time or ranking (as well as present in each row of training data) that come from data about onsite activity.

Example:
- The age / gender a person has entered into their Facebook profile would be an "onsite user feature"
- The historical click-through rate of an ad would be an "onsite ad feature"

#OFFSITE FEATURES:

Offsite features are pieces of information that are available at the time of ranking (as well as present in each row of training data) that come from data about offsite activity.

Example:
- Data about a person's historical interactions with other apps / websites would be "offsite user features"
- Data about the historical conversion rate of a "website conversions" ad would be an "offsite ad feature"

#ONSITE LABELS:

Onsite labels indicate if a given impression led to a conversion on Facebook itself.

Examples:
- For "Link Click" optimized ads, as the click happens on Facebook itself, the conversion label is measured "onsite"
- "Video View" optimized ads also have "onsite" labels, since the "video view" event (e.g. reaching 2 continuous seconds of play while the video is at least 50% in the viewport) happens onsite

#OFFSITE LABELS:

Offsite labels indicate if a conversion did or did not happen, when the conversion occurs on another app or website.

Examples:
- Website conversion ads. Since the "website conversion" happens on another website, the 0 or 1 label that indicates if the ad impression eventually led to an optimized conversion is "offsite"
- Mobile app install / mobile app event ads are the same.

#DENSE FEATURES:

Features which can be represented as a numerical value that has some meaning.

Examples:
- A historical click-through-rate can be represented as a floating point value between 0 and 1
- The optimization goal of an ad can be represented as an enum. An integer value will suffice.

#SPARSE FEATURES:

Features which indicate a particular item out of an extremely large set of possibilities. These are usually represented by a vector of many (e.g. 64) floating point values. This vector of floats is usually an "embedding" which is learned through back-propagation in a neural network. In this way, "similar" entities are vectors which point in a "similar direction" in N-space. Multiple entities can be combined through "pooling". Sometimes this is as simple as just adding the vectors together.

Examples:
- The set of Facebook pages a person follows (onsite)
- The set of ads a person has clicked on in the past (onsite)
- The set of websites a person has visited in the past (offsite)

#CLICK QUALITY:

A complex, ever evolving set of measurements that attempt to capture ideas like:
- How much value was created as a result of a click
- How intentional was a click
- How good / bad of a user-experience was associated with a click

#TIME TO RETURN:

One of multiple approaches to measuring click quality. Record the time at the moment a link-click occurs. This link-click results in a navigation to an advertiser website, or app-store, or app via a deep link. Once the person has finished interacting with the destination and returns to their prior context, record the time. The difference between these two timestamps is the "time to return". It is very common on the open-web, for people to click on ads accidentally. When they do so, they tend to return very quickly to their original context. When a person returns to their prior context after interacting with the advertiser destination for an extremely short period of time (e.g. under 2 seconds), it can be argued that no value was created for the advertiser, that the click was unintentional, and any "conversion" events attributed to this click are not causal - and are likely just stealing credit that is not due. For these reasons, Audience Network (Facebook's ad network) has ignored such clicks, and not attributed conversions to them for the last few years of operation.  Before this system was implemented, many years ago, over 1/3 of all clicks on the ad network had a "time to return" of less than 2 seconds.

#CLICKGUARD:

One approach used to improve click quality. An unclickable mask over the top of an ad which prevents clicks. Audience Network (Facebook's ad network) has a clickguard that makes ads un-clickable for the first 1-second after they become viewable. The rationale is that the human reaction time is around 680 milliseconds, so clicks that happen in roughly that timeframe (or less) could not possibly be intentional, but instead are likely unintentional.
