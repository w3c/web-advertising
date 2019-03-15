# The Use Cases of Online Advertising

Discussion Paper for the  
W3C Advertising Business Group

Wendell Baker <[wbaker@verizonmedia.com](mailto:wbaker@verizonmedia.com)\>  
Initial draft, 2019-03-05

This document describes event sequences supporting modern online advertising. Many details are elided. There is a significant amount of tutorial literature available which can be used to fill in the details and omissions that are necessarily present in this summary document.

Ad Call with Single Market
==========================

![Event Flow Diagram](https://w3c.github.io/web-advertising/UseCasesofOnlineAdvertising/image1.png)

### Steps

1.  UserBrowser calls Publisher Web App (Web Site) asking for content
2.  Content is returned
3.  UserBrowser calls Publisher Content Server to assemble the page
4.  Content is returned
5.  UserBrowser calls the Ad Marketplace “give me an ad”
6.  Creative Code is returned
7.  UserBrowser calls the Creative Payload Server
8.  Creative Payload is returned
9.  UserBrowser calls a Content Delivery Network to recover the payload (images).
10.  Payloads are returned.

### Background Activities

*   Counting jof a Page Delivered occurs at the Publisher Web App (Web Site)
*   Counting of a Sale occurs at the Ad Market
*   Counting of a Page Viewed occurs a the Creative Payload service.

### Other Considerations

*   There may be many marketplaces (Steps 5 & 6) in a cascade or waterfall.
*   There may be many creative payload services (Steps 7 & 8), each providing a different business capability: fraud mitigation, visibility verification, conversion counting, and so forth.
*   The UserBrowser may solicit from multiple marketplaces simultaneously, resolving the winner in the consumer premises equipment.
*   Each marketplace may have a deeper structure with subcalls to other services and other marketplaces with whch they trade.