# Common User Flows used in web advertising


## Browser => Browser (Same Device)
### 1st party:
Ad is shown on a first-party website rendered within a fully-featured browser (on either Desktop or Mobile). Clicking the ad opens another website in the same browser. 

_<example: ad shown on m.facebook.com directing to adidas.com>_

**Conversion attribution:** Conversion event matched to user identity using 3rd party cookies, or 1st party cookies placed via link decoration
### 3rd party:
Same scenario except for third-party website. 

_<example: ad shown on washingtonpost.com, served by Google’s ad network, directing to adidas.com>_

**Conversion attribution:** Same as 1st-party

## Mobile Browser => Desktop Browser (Different Device)
### 1st party:
Ad is shown on a first-party website rendered within a fully-featured mobile browser. Clicking the ad opens another website in the same browser. The user might complete a conversion event at that time, in that mobile browser, but they might also return to the same site later, on a desktop browser to complete the conversion event. 

_<example: ad shown on m.facebook.com directing to royalcaribbean.com. Conversion occurs later from that person’s Desktop computer>_

**Conversion attribution:** The person must have signed into the publisher website on both their mobile and desktop browser. Conversion events are matched to the user identity using 3rd party cookies, or 1st party cookies placed via link decoration
### 3rd party:
Same scenario except for third-party website. 

_<example: ad shown on washingtonpost.com, served by Google’s ad network, directing to royalcaribbean.com>_

**Conversion attribution:** The person must have signed into a website operated by the party that operates the ad network on both their mobile and desktop browser. Conversion events are matched to the user identity using 3rd party cookies, or 1st party cookies placed via link decoration

## In-App-Browser => In-App-Browser
### 1st party:
Ad is shown on a first-party website rendered within an in-app-browser within a native mobile app (i.e. App is just a wrapper around a webview). Clicking the ad opens another website in the same in-app-browser. 

_<example: ad shown on Facebook app directing to adidas.com>_

**Conversion attribution:** There are a few options.
Use 3rd party cookies (or 1st party cookies with link decoration) that are scoped just to the in-app-browser.
Alternatively, it’s possible for native code in the containing app to communicate directly with JavaScript code running on the webpage within the webview. Unique identifiers could be injected into the webview (to be later sent back with conversion events) or conversion events could just report out directly into native code.

### 3rd party:
Same scenario except for third-party website. 

_<example: After clicking a link on the Facebook app, it opens a webpage in the In-App-Browser. Ads on that page are served by Google’s Ad Network. Clicking them redirects the Facebook In-App-Browser to adidas.com>_

**Conversion attribution:** It depends substantially on which ad network serves the ads. 
In the example above (ad network unrelated to app hosting the webview) it’s unclear how the Google ad network can match the user identity at the time the ad request is made. However, conversion events fired from subsequent webpages can be measured with either 3rd party cookies, or 1st party cookies and link-decoration.
If the ad network is operated by the same party that is operating the native app containing the webview (i.e. Facebook Audience Network serves the ad to a webpage rendered within the Facebook In-App-Browser) then, in addition to these cookie based solutions, it’s possible for native code in the containing app to communicate directly with JavaScript code running on the webpage within the webview. Unique identifiers could be injected into the webview (to be later sent back with conversion events) or conversion events could just report out directly into native code.

## Mobile Browser => Mobile App Store
### 1st party:
Ad is shown on a first-party mobile website. Clicking the ad opens to a particular application page in a mobile app store. 

_<example: ad shown on m.facebook.com directing people to install the Uber app>_

**Conversion attribution:** Conversion events are measured by asking the app developer to incorporate an SDK, that sends the IDFA / AAID back on first app launch. This is matched to user identity via the user’s IDFA / AAID, which is only known if they are also logged in to a native mobile app product provided by the same first party as operates the mobile website.
### 3rd party:
Same scenario except for a third-party mobile website. 

_<example: ad shown on nytimes.com served by Google’s Ad Network directing people to install the Uber app>_

**Conversion attribution:** App installs are reported by the native app store to the publisher. This is enabled by URL parameters embedded in the ad link.

## Mobile App => In-App-Browser
### 1st party:
Ad is shown in a first-party native app. Clicking the ad opens a website in an In-App-Browser. 

_<example: majority of ads shown on Facebook, Instagram, Tik-Tok, Pintrest, Twitter, Gmail, Youtube, etc.>_

**Conversion attribution:** A couple options:
The app can place cookies into the In-App-Browser using a few techniques. One of them is via a “cookie-drop redirect page”. Once this is done, conversions fired from pages loaded within the in-app-browser can be matched using 3rd party cookies. 
It’s also possible for native code in the containing app to communicate directly with JavaScript code running on the webpage within the webview. Unique identifiers could be injected into the webview (to be later sent back with conversion events) or conversion events could just report out directly into native code.
### 3rd party:
Same scenario except for a third-party native app. 

_<example: ad shown in CandyCrush app, served by an ad-network, directing to a mobile website>_

**Conversion attribution:** When the ad is served by the 3rd party ad network, the ad request contains the IDFA / AAID, which can be resolved to a particular user. When the ad link is clicked, it first takes them to a redirect link (hosted by the ad network) that drops some cookies in the In-App-Browser before redirecting onwards to the destination website. If a conversion then happens on the destination page, these 3rd party cookies are sent back to the ad network, which resolves them to the user.

## Mobile App => Mobile Browser
### 1st party:
Ad is shown in a first-party native app. Clicking the ad opens a website in the built in mobile browser. 

_<example: I cannot actually think of an example of an app that does this...>_

**Conversion attribution:** This requires understanding of user identity through IDFA/AAID in the native app, AND cookies in the built-in mobile browser (ideally pre-dropped through some other first party website interaction). Another mechanism used here is URL parameters (e.g. dclid) to pass on a form of identity to the built in web browser, which will result in a dropped first-party domain cookie. Conversion events then matched by later resolving user identity between original ad-click and event with cookie.

### 3rd party:
Same scenario except for a third-party native. 

_<example: ad shown in CBS Sports app, served by Google, directing to online.berklee.edu>_

**Conversion attribution:** Same as 1st-party.

## Mobile App => Mobile App Store
### 1st party:
Ad is shown in a first-party native app. Clicking the ad opens to a particular application page in a mobile app store. 

_<example: ad shown on Snapchat, opens to Deliveroo application page within Apple App Store>_

**Conversion attribution:** Conversion events measured by asking the app developer to incorporate an SDK, that sends the IDFA / AAID back on first app launch. This is matched to user identity via the user’s IDFA / AAID.

### 3rd party: 
Same scenario except for a third-party native app. 

_<example: ad shown on Plants Versus Zombies app, served by Facebook Audience Network, promoting another native app>_

**Conversion attribution:** Same as 1st-party.

## Mobile App => Mobile App (Deeplink)
### 1st party:
Ad is shown in a first-party native app. Clicking the ad opens another app directly via a deeplink to a specific product / item / page. 

_<example: ad shown within the Google Maps app, opens the Uber app with destination already plugged in>_

**Conversion attribution:** Conversion events are both based on identical IDFA/AAID in both apps.

### 3rd party:
Same scenario except for a third-party native app. 

_<example: ad shown in Angry Birds app, served by Google’s ad network, that opens a particular product in the Wish app>_

**Conversion attribution:** Same as 1st-party

## Mobile App => Same Mobile App (Deeplink)
### 1st party:
Ad is shown in a first-party native app. Clicking the ad redirects you to a deep location within the same app. 

_<example: promoted products within the Amazon App, brings you to the product within the same app>_

**Conversion attribution:** Conversion event reporting done entirely first party, using whatever mechanism available (IDFA, logged-in user info, app-scoped ID, etc)
### 3rd party:
No third-party equivalent
