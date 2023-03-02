# The Use Cases of Online Advertising

Discussion Paper for the  
W3C Advertising Business Group

Contibutors:
* Wendell Baker <[wbaker@verizonmedia.com](mailto:wbaker@verizonmedia.com)\>  
* Michael Kleber <[kleber@google.com](mailto:kleber@google.com)\>

Draft, updated 2019-04-17

This document describes event sequences supporting modern online advertising. Many details are elided. There is a significant amount of tutorial literature available which can be used to fill in the details and omissions that are not necessarily present in this summary document.

## Dramatis Personae

The web advertising ecosystem is a collaboration among many parties.  Common designations like "third-party" can often be ambiguous when a single pageload involves interactions with servers run by many organizations with different roles, needs, and capabilities.  So we begin with a list of types of parties and what roles they play.

* **Publisher:**
  Publishers produce web pages that users visit.  Most of the time, you can think of the publisher as the first party (though aggregation and syndication sites can add a little confusion).
  
  Since we're discussing the web ads ecosystem, the publisher web pages we care about will generally have ads on them.  That means you can think of publishers as creating web pages containing _empty rectangles_ — rectangles which they want to sell to advertisers.  Since publishers sell empty rectangles, they are referred to as the _sell side_ or the _supply side_ of the market.  The empty rectangles themselves are sometimes referred to as _ad slots_ or _impressions_, or as _inventory_ when thinking of them as something being sold.
  
  Publishers generally have a lot of control over their content, but the amount of control they have over the advertisements on their pages varies a lot; see "Publisher Ad Platform" below.
  
* **Advertiser:**
  Advertisers want to show some message to people visiting publishers' web sites, and are willing to pay money to publishers to do so.  Their messages are the ads, also called _creatives_, and can include text, images, video, JavaScript, and so on — all the capabilities of a web page.  Creatives  usually fit into a standard-sized rectangle, meant to fill in one of the empty rectangles on a publisher page.
  
  Since advertisers buy rectangle-shaped real estate to put their ads in, they are referred to as the _buy side_ or the _demand side_ of the market.  When a specific ad creative appears on a user's load of a publisher web page, the event is called an _impression_.
  
  The advertiser might pay by the impression, or might only pay if a user actually clicks on the ad.  Many advertisers have some goal beyond the impression or click — they might want the user to sign up for their newsletter, buy something on their web site, etc.  Those later target events are called _conversions_.
  
* **Publisher Ad Platform:**
  A service used by a publisher to pick which ad to show in each ad slot on each page view, often implemented as a 3rd-party HTTP request.  The publisher may have made direct agreements with various advertisers, in which case the ad platform's job includes fulfilling those preexisting obligations.  The ad platform may also sell the publisher's inventory to a range of advertisers, either directly or through an exchange.
  
  Types of publisher ad platforms include:
  - _Ad networks_, which also work directly with advertisers;
  - _Ad exchanges_, which offer publisher inventory to a multiple demand sources in a real-time auction;
  - _Supply-Side Platforms_ or _SSPs_, which sell publisher inventory on multiple ad exchanges.
  
* **DSP** or **Demand-Side Platform:**
  A service used by an advertiser to buy inventory in real-time auctions, often across multiple ad exchanges.  DSPs engage in _programmatic buying:_ they evaluate each potential impression and decide how much to bid on behalf of their advertisers.  They often receive feedback from the advertiser about conversions, including the value of the conversion, which lets them pay attention to the advertier's return on investment (ROI).
  
* **Ad Servers:**
  Servers that host and serve the actual ad creative payload and serve it to the user's browser.  They can include both a CDN and a business reporting component: The ad server may perform some accounting and some verification that the ad really is being rendered on a publisher page desirable to the advertiser.


## Ad Call with Single Market

![Event Flow Diagram](https://w3c.github.io/web-advertising/OnlineAdvertisingParticipants/image1.png)

### Steps

1.  UserBrowser calls Publisher Web App (Web Site) asking for content
2.  Content is returned
3.  UserBrowser calls Publisher Content Server to assemble the page
4.  Content is returned
5.  UserBrowser calls the Ad Marketplace, some sort of publisher ad platform, and says “give me an ad”
6.  Creative Code is returned
7.  UserBrowser calls the Creative Payload Server
8.  Creative Payload is returned
9.  UserBrowser calls a Content Delivery Network to recover the payload (images).
10.  Payloads are returned.

### Background Activities

*   Counting of a Page Delivered occurs at the Publisher Web App (Web Site)
*   Counting of a Sale occurs at the Ad Market
*   Counting of a Page Viewed occurs a the Creative Payload service.

### Other Considerations

*   There may be many marketplaces (Steps 5 & 6) in a cascade or waterfall.
*   There may be many creative payload services (Steps 7 & 8), each providing a different business capability: fraud mitigation, visibility verification, conversion counting, and so forth.
*   The UserBrowser may solicit from multiple marketplaces simultaneously, resolving the winner in the consumer premises equipment.
*   Each marketplace may have a deeper structure with subcalls to other services and other marketplaces with which they trade.
