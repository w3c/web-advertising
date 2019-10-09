# Browser API for Private Lift Measurement - Third Party

We propose a new set of browser based APIs and behaviors, to implement “Private Lift Measurement” to measure the effectiveness of ads **that run across a large number of eTLD+1**.

For context as to the motivations, limitations, and alternatives, see the "[Private Lift Measurement Conceptual Overview](https://github.com/w3c/web-advertising/blob/master/private-lift-measurement-conceptual-overview.md)"

## Proposal

- Allow a browser to consistently assign users to either a test or control group
  - Test/Control assignment depends on both the ad-network being measured (perhaps indicated via the `reportingdomain`), and the destination eTLD+1 that the studied ad links to.
- Allow the browser to enforce that decision by only showing the studied ad to people in the test group, suppressing the ad in the control group. 
- If the user produces a conversion event on the destination eTLD+1 being promoted by the studied ad, report back the conversion value with a test/control flag.
- The browser must hide the test/control decisions it makes from application code to prevent abuse of this API as a means to implementing cross-site tracking.

## Displaying Ads

We propose the publishing website an iframe (similar to Chrome’s [Conversion Measurement proposal](https://github.com/csharrison/conversion-measurement-api#permission-delegation)) The ad network provides two ads, an ad to show to people in the “test group”, and a “fallback ad” to show to people in the control group. These would be clearly marked, and both would be provided in the HTML served by the ad-network using the markdown within an <a> tag embedded in the IFrame.

```
<iframe ...>
<a testGroupPercentage=”50” reportingdomain=”https://ad-tech.com”>
  <testGroup href=”shop.example”>
    <div>
      <img src=”...” />
      <p>...</p>
      ...
    </div>
  </testGroup>
  <controlGroup href=”toasters.com”>
    <div>
	    <video src=”...” />
      <p>...</p>
      <img src=”...” />
      ...
    </div>
  </controlGroup>
</a>
</iframe>
```

The important thing is that it will be **the browser** that decides which of the two ads to show, the test or the control ad.

This can be achieved in a way that does not enable any form of cross-site tracking. We propose the following approach:

The ad display tag includes an attribute for “testGroupPercentage” instead of the “campaignID”  proposed by Private Click Measurement. This would take values from 0 to 100. To limit information entropy, browsers could consider introducing limits on this, for example, limiting the values to the range of 50 to 90, only supporting multiples of 5, etc.

The first time the browser needs to make a test/control decision about ads being shown by a given ad network (as identified by a unique `reportingdomain`), it just needs to generate (and store) a random identifier: `adnetwork_specific_random_id`. This same identifier will be retrieved by the browser from storage and used on all subsequent test/control decisions made in that browser for ads served by the same ad-network, (that is, ads using that same `reportingdomain`. To be clear, this means that the same `adnetwork_specific_random_id` would be used to make test/control decisions across many different websites that utilized the same ad-network. This random identifier must not be accessible to application code, in fact it must never be available in the DOM or sent to the server. 

We then use a keyed hashing function, such as blake2b, which is keyed on the `adnetwork_specific_random_id`. 

```
h = blake2b(key=adnetwork_specific_random_id)
is_test = h(destination_domain) % 100 < testGroupPercentage
```

By using a keyed hashing function, we accomplish the following design goals:
- Within a given ad-network, a given user’s likelihood of being in “test” is independent across destination domains (no correlation between tests, they do not influence one another)
- A given user’s likelihood of being in “test” is independent across ad-networks (no correlation between tests run across multiple ad-networks, they do not influence one another)
- It is consistent across all ad campaigns that direct to a given domain. (A given person will always be in either “test” or “control” with respect to ads served by that ad-network which are promoting a given domain)

## Privacy considerations

This `adnetwork_specific_random_id` presents a high risk of being utilized for cross-site tracking. Even if the identifier itself is not accessible to application code, any kind of leakage of information about this identifier could potentially be used for cross-site tracking. 

An example of the way this could be abused to enable two websites to join the same user across their two domains:

- Both websites retain the browser selection of test/control for every ad leading to a distinct set of destination domains, where the `reportingdomain` is constant. Each test group has a 50% size.
  - The websites can build a profile of destination-domain => test/control-assignment decisions over time, using their 1st party cookie.
- After building these profiles, suppose the two websites have the test/control assignment decision across the same set of 100 destination domains. 
  - Each website now has a 100 bit identifier for the user. It’s technically possible for there to be collisions (two users with 100 identical treatments) but the rate of collisions can be brought down to a vanishingly small number as the number of destination-domains => test/control-assignment mappings stored grows.

This implies that it must be **technically impossible** for the publisher to know which ad the person actually saw. This has a few implications:
- It must not be possible to use JavaScript APIs to inspect the content of an <a> tag with a “testGroupPercentage” attribute. For example, requesting the innerHTML, or children must either return both, or neither of the two ads.
- The width and height of the <a> tag must be the same regardless of which ad was shown. Either the ad must always resize to the maximum of the two, or the width and height must be specified in advance.
- We must be *very careful* about what type of HTML is allowed within the <testGroup> and <controlGroup> tags. For example, <script> tags probably must be banned. We do not want logging code to be able to report back to the publisher any kind of data which could reveal which ad was shown. Alternatively, we can allow script tags, but the execution of those scripts must not be influenced by whether or not the ad was selected.
- Although the ad must be interactive (for example, it should be clickable, and clicks should direct the viewer to different destinations), the publisher cannot know which ad was clicked. (It should be OK to know that *one* of the two ads was clicked, but not which)

## Issues for Publishers

It’s an unfortunate side effect that when using this API, ad-networks will not be able to report 100% accurate counts of impressions / clicks / video-views to advertisers when they use this API. This is an unavoidable consequence of the ad-network not knowing themself which ads were actually shown.

Our suggestion is to just count fractional impressions / clicks / video-views and rely on the law of large numbers to ensure that the estimated counts are approximately equal to the real numbers.

Specific example:
- Publisher provides ad ID 12345 as the test group and ad ID 67890 as control to the browser, with a 60% chance of test.
- JavaScript running on the publisher site deems an ad “viewable”. It reports an “impression”
- The publisher increments the number of impressions of ad ID 12345 by 0.6 and the number of impressions of ad 67890 by 0.4

Perhaps there is a better solution to be had here by utilizing Chrome’s proposed “[Aggregate Reporting API](https://github.com/csharrison/aggregate-reporting-api)”.

## Reporting Conversions 

Currently proposed approaches to “Private Click Measurement” have a number of similarities. They use anonymous requests, add random timing delays, and limit the amount of information contained in the report. We assume that similar approaches can be applied here. We propose only a few small changes to the contents of the fields contained in the report:

### Current PCM report:

| Key      | Value          |
|----------|----------------|
| From     | search.example |
| To       | shop.example   |
| Campaign | 55             |
| Convert  | 20             |

### Lift report

| Key      | Value          |
|----------|----------------|
| From     | search.example |
| To       | shop.example   |
| Is Test? | 1              |
| Convert  | 20             |

Instead of a 6-bit ([Safari proposal](https://wicg.github.io/ad-click-attribution/index.html)) or 64-bit ([Chrome proposal](https://github.com/csharrison/conversion-measurement-api)) “campaign ID” value, there would only be a single bit, indicating if the person who’s browser fired the conversion report was in test or control.

Just as in the Safari and Chrome proposals, the reports will be sent individually and with some time delay. This prevents linkage between different reports from the same browser, which if joined could be used as a high entropy identifier.

## Conversions from control group?

This implies that conversion are being reported from people who did not click on an ad (and in the case of the control group, who never even saw the ad). For this reason, an even higher standard of anonymity is required, thus the reduction in total entropy.

This implies another change from normal PCM: the browser needs to store more than just clicks, now it must store opportunities to see the ad.

The moment at which the browser parses the HTML for the ad, and makes the decision as to whether this person should be in the “test” or “control” group is the “opportunity”. This is distinct from an “impression” which usually connotes ideas about “viewability”. This means that some people in the “test” group might not actually have seen the ad (perhaps it never became viewable), which will cause a small amount of dilution in the test. This isn’t ideal, but it’s significantly simpler to implement, and so long as the ads are effective, and the average rate of viewability is high, there should still be an observable difference between the number of conversions in the test and control group.

## Other Considerations

In order to try to provide consistent test/control status for the same person across multiple devices they own, the browser can sync the `adnetwork_specific_random_id` from one device to another.

In order to measure conversion events where the ad-opportunity / ad-impression was on one browser, and the conversion was on another, not only the `adnetwork_specific_random_id` but also the locally stored “ad-opportunities requesting attribution” could be synced across devices belonging to the same user.

