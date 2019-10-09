# Browser API for Private Lift Measurement - First Party

We propose a new set of browser based APIs and behaviors, to implement “Private Lift Measurement” to measure the effectiveness of ads **that run on a single eTLD+1**.

For context as to the motivations, limitations, and alternatives, see the "[Private Lift Measurement Conceptual Overview](https://github.com/w3c/web-advertising/blob/master/private-lift-measurement-conceptual-overview.md)"

## Proposal

- Allow a browser to consistently assign users to either a test or control group
  - Test/Control assignment depends on both the publisher eTLD+1 where the ad is shown, and the destination eTLD+1 that the studied ad links to.
- Allow the browser to enforce that decision by only showing the studied ad to people in the test group, suppressing the ad in the control group. 
- If the user produces a conversion event on the destination eTLD+1 being promoted by the studied ad, report back the conversion value with a test/control flag.

## Displaying Ads

In the markdown on the page, within the <a> tag, we propose the publisher provide two ads, an ad to show to people in the “test group”, and a “fallback ad” to show to people in the control group. These would be clearly marked, and both would be provided in the HTML.

```
<a testGroupPercentage=”50”>
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
```

The important thing is that it will be **the browser** that decides which ad to show, the test or the control ad. 

This can be achieved in a way that does not enable any form of cross-site tracking. We propose the following approach:

The ad display tag includes an attribute for the “testGroupPercentage” instead of the “campaignID” proposed by Private Click Measurement. This would take values from 0 to 100. To limit information entropy, browsers could consider introducing limits on this, for example, limiting the values to the range of 50 to 90, only supporting multiples of 5, etc.

The first time the browser needs to make a test/control decision about ads being shown on a given website, it just needs to generate (and store) a random identifier: `domain_specific_random_id`. This same identifier will be retrieved by the browser from storage and used on all subsequent test/control decisions made in that browser for that website. This random identifier need not be accessible to application code, in fact it will never be available in the DOM or sent to the server. Think of it as similar to the browser automatically setting a first-party cookie for the domain that’s initialized to a random value, but in a way that application code cannot read or write to it.

We then use a keyed hashing function, such as blake2b, which is keyed on the `domain_specific_random_id`. 

```
h = blake2b(key=domain_specific_random_id)
is_test = h(destination_domain) % 100 < testGroupPercentage
```

By using a keyed hashing function, we accomplish the following design goals:
- A given user’s likelihood of being in “test” is independent across destination domains (no correlation between tests, they do not influence one another)
- For ads served on this publisher domain, it is consistent across all ad campaigns that direct to a given destination domain. (A given person will always be in either “test” or “control” with respect to ads promoting a given destination domain)

## Privacy considerations

This domain specific random ID probably needs to be reset if someone clears their cookies for a given domain. Otherwise it could be used to link the same user across two sessions when their expectations are that the website cannot link the two visits. This is unfortunate for the quality of lift measurement (they will change test/control group each time they reset their cookies), but it seems unavoidable.

This domain specific random ID probably cannot be used normally for users in “Incognito Mode” or “Private Browsing”. We recommend just using a random value for those cases. Otherwise this could potentially be used to link two visits from the same person, one in “Incognito mode” the other not.

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

This implies that conversions are being reported from people who did not click on an ad (and in the case of the control group, who never even saw the ad). For this reason, an even higher standard of anonymity is required, thus the reduction in total entropy.

This implies another change from normal PCM: the browser needs to store more than just clicks, now it must store opportunities to see the ad.

The moment at which the browser parses the HTML for the ad, and makes the decision as to whether this person should be in the “test” or “control” group is the “opportunity”. This is distinct from an “impression” which usually connotes ideas about “viewability”. This means that some people in the “test” group might not actually have seen the ad (perhaps it never became viewable), which will cause a small amount of dilution in the test. This isn’t ideal, but it’s significantly simpler to implement, and so long as the ads are effective, and the average rate of viewability is high, there should still be an observable difference between the number of conversions in the test and control group.

## Other Considerations

In order to try to provide consistent test/control status for the same person across multiple devices they own, the browser can potentially sync the `domain_specific_random_id` from one device to another.

In order to measure conversion events where the ad-opportunity / ad-impression was on one browser, and the conversion was on another, not only the `domain_specific_random_id` but also the locally stored “ad-opportunities requesting attribution” could be synced across devices belonging to the same user.

In the case of the **complex first party** (e.g. [Oath](https://www.oath.com/our-brands/) owns both TechCrunch and HuffPost.) perhaps if the user is logged-in to multiple sites, and there is already a shared user-identity, it could be possible to utilize the same `domain_specific_random_id` across these sites. Perhaps we can design a mechanism for a publisher to declare multiple domains as being part of the same experimental group, similar to the proposal put forward to allow multiple domains to [share a single cookie relationship](https://github.com/mikewest/cookie-samesite-firstparty).
