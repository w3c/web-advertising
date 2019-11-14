# Summary

Real Time Bidding or RTB is a standard which establishes an open marketplace for online advertising.  A popular version of this standard is [OpenRTB](https://www.iab.com/wp-content/uploads/2016/03/OpenRTB-API-Specification-Version-2-5-FINAL.pdf).  The existence of this marketplace is critical to the advertising industry as it provides publishers a way to obtain fair value for their inventory and advertisers a way to improve the effectiveness of their advertising either by cost or effectiveness optimization.

This use case is heavily dependant upon anonymous user-level tracking but it need not be.  At the end of the day, these exchanges do not exist to track users; they exist to enable a value exchange between publishers and advertisers.  The user-level tracking which exists today is merely a means to an end.  If browsers are willing to act as a trust agent in this context, it is conceivable that an open advertising market could be created which is not dependant upon user-level cross-site tracking.

# Why is it important to preserve this use case?

RTB provides an open marketplace for publishers to more effectively monetize their content and for advertisers to optimize their ad budgets.  Open marketplaces ensure that participants (in this case publishers and advertisers) provide fair value for what they're purchasing or recieve fair value for what they offer.  While in it's current form, RTB is dependant upon user-level tracking, the core value prop of an open advertising marketplace does not necessarily need to be dependant upon user-level tracking.

# How is it functionally achieved today?

1. [The use cases of online advertising explainer](https://github.com/w3c/web-advertising/blob/master/UseCasesofOnlineAdvertising.md) describes many of the participants in this marketplace but does not discuss the RTB marketplaces which enable those exchanges
1. High-level flow
    1. User visits publisher property foo.com
    1. Publisher makes a bid request to an ad exchange.  Bid request includes details about the ad placement as well as details about the user.
    1. Ad exchange forwards the request to multiple DSPs
    1. DSPs, using information from the publisher and of the user will place a bid to show their ad.  The bid can come from any advertiser active on the DSP at the time of the bid.
    1. After some time, the ad exchange will take the bids it has received and select a winner based on exchange and publisher bidding rules
    1. The winning ad is sent to the user's browser and loads from the user's browser
    1. The ad server which served the ad records this using data from the browser
    1. In the ad itself, there are often additional third party trackers which enable verification to the advertiser

# What are the requirements of this use case?

1. Publishers
    1. Need a way to list inventory on an exchange
    1. Need a way to prove the value of their inventory without user-level tracking
    1. Need a way to segment premium inventory from non-premium inventory
1. Advertisers
    1. Need to be convinced that they received ad placements they paid for on the exchange
    1. Need proof of the value of the ad placement they paid for
        1. Needs to minimize and eliminate fraud where possible
        1. Needs to know at what rate an ad they paid for was seen
        1. In the case of streaming content ads (video, audio), the advertiser needs to know if the ads were fully or partially consumed
    1. Needs optimization levers to improve bidding intelligence.
1. Exchanges
    1. Needs a way to list publisher inventory
    1. Needs a way to enable marketplace rules
1. Marketplace
    1. Needs to know that transactions are accurate
    1. Needs to know that fraud is low or non-existent (along with a way to eject fraudulent actors)
    1. Needs to know value delivered from ads served
       
