# One-to-One Personalization

In the area of marketing, personalization typically means tailoring the ad content in order to meet the individual needs of each user.

As shown by many well-known examples in various sectors (web search, social/video platforms, online retail marketplaces, mobile app stores…), it can be a game-changer for both: user experience and effectiveness of the marketing activities. 

While user lists are not enough to satisfy this use case, there are other technical solutions that allow 1:1 personalization without affecting the users’ privacy.

## Personalization in RTB

Modern process of ad delivery is typically led by three decisions:

* **Targeting** – shall we consider this ad/campaign?
* **Bid valuation** – how much are we willing to pay for this?
* **Personalization** – what exact content shall we present in the ad?

Current technical solutions allow us to answer all three questions with a perfect granularity – a single impression and a single user.

However, the technology behind raises privacy concerns. To minimize them, one possible solution is to somehow group users instead of identifying them 1:1 – a concept called "users lists". Doing so for targeting and bid valuation wouldn’t necessarily hit hard effectiveness. Nor the user experience. Thus, this probably is one of the good, future directions.

But, the situation with personalization is very much different – preserving the possibility of one-to-one personalization is crucial for delivering to each individual user the ad content that will be useful and valuable for him or her.

## User’s perspective

If I’m really into fantasy books, I may end up on a user list 'fantasy-lovers'. And that’s fine for targeting, and that’s OK for bid valuation. But that’s not enough for personalization. A broadly targeted static ad showing me best selling books in the category is useless for me and just annoying. Only the ad showing me recommendations carefully tailored towards my individual taste, my reading history, my past reviews etc... makes sense to me.

The problem scales inversely with the size of the advertiser. Largest advertisers might have a big enough user base to be able to somewhat accurately personalize based on user lists (assuming reasonable size is allowed). However, the long tail of not-the-largest advertisers will be in a lost position – a campaign for local bookshop will be performing significantly worse than similar one from a large retailer. The only fair solution is 1:1 personalization.

The imprecise personalization negatively affects user satisfaction, ad effectiveness from the advertiser's perspective, and as a consequence publishers' revenue stream. The whole ecosystem is hurt.

## Privacy

One-to-one personalization within marketing is widely accepted by users. The main concern is that current technical solutions allow to leak information to many unrelated parties and this can be abused. This is not an issue in case of big platforms where a single domain serves as publisher, advertiser and a data source – all at once. So, the only question left is how this can be addressed in a more scattered ecosystem of a, so called, open internet.

Recent ideas can help with that – for instance, the concept of on-device auctions introduced in the [TURTLEDOVE](https://github.com/michaelkleber/turtledove). While the user lists cannot be used in this case, by slightly tweaking the proposal, it should be possible to guarantee user privacy by keeping the user data on the device, while still allowing 1:1 personalization.
