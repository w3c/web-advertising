# PETREL

* [Motivation](#motivation)
* [Proposal Overview](#proposal-overview)
* [API Example Flow](#api-example-flow)
    * [Adding a User to an Exclusion Group](#adding-a-user-to-an-exclusion-group)
    * [Displaying an Ad to a User](#displaying-an-ad-to-a-user)
* [Design Considerations](#design-considerations)
    * [Overlap with TURTLEDOVE](#overlap-with-turtledove)
    * [Possible Attacks](#possible-attacks)
* [Extensions](#extensions)

## Motivation

One of the most common among user complaints about digital advertising is that ads promoting a specific product continues to be delivered even after the person has purchased that item. To address this issue, advertising platforms developed exclusion targeting: an advertiser will notify us of a purchase, identity is determined from a 3rd party cookie, and that person is excluded from seeing an ad for that product in the future. While the removal of 3rd party cookies removes the ability to perform exclusion targeting under its current design, we believe that this can be achieved in a privacy preserving manner.

This proposal, Private Exclusion Targeting Rendered Exclusively Locally ("PETREL") leverages the idea of blind ad rendering for exclusion targeting, following some of the same ideas proposed in [TURTLEDOVE](https://github.com/michaelkleber/turtledove). In fact, we think this is an even simpler model that could act as an incremental step towards adoption of TURTLEDOVE.

## Proposal Overview

* We propose utilizing the same "interest group" concept introduced by TURTLEDOVE, but with the purpose of preventing an ad from being displayed.
    * To recap, this involves the advertiser website calling an API which invites the browser to join a group belonging to that advertiser, with some name.
    * Browsers should have privacy settings to control membership in these groups so that users may modify it at their discretion.
* When a conversion happens within a browser, the advertiser can store the relevant `product_id` privately in the browser as an "exclusion group", analogous to the TURTLEDOVE "interest group".
* An ad placement can leverage PETREL by providing an ordered list of potential ads which each individually include a `product_id`.
    * The browser locally and privately decides to display the first ad that does not contain a `product_id` belonging to an exclusion group it has stored.
* As with TURTLEDOVE, blind rendering is used to ensure the page cannot learn which ad was shown to prevent leaking data about the user. This mitigates revealing 3rd party data to the 1st party context.

## API Example Flow

This section provides a pseudo-code example for how this API could work, end to end. We've provided this as a concrete example, but the details and design of an implementation are left open.

### Adding a User to an Exclusion Group

Suppose that I visit [shop.example](http://shop.example/) and make a purchase for some product. In the backend of [shop.example](http://shop.example), they use `product_id="7546c0903c23"` to identify this product. When this happens, [shop.example](http://shop.example) decides that they would like to stop serving me ads for that product for 90 days.

```
var exclusionGroup = {'owner' : 'shop.example',
                      'product_id' : '7546c0903c23',
                      'readers' : ['publisher.example',
                      'publisher2.example']
 };
navigator.joinAdExclusionGroup(exclusionGroup, 90 * kSecsPerDay);
```

### Displaying an Ad to a User

Suppose that [shop.example](http://shop.example) has purchased ads from [publisher.example](http://publisher-1.example/) (or from some ad network running in the first-party context of [publisher.example](http://publisher.example)). Further suppose that when I visit [publisher.example](http://publisher.example), an ad for `product_id="7546c0903c23"` wins an auction for an impression.

The goal of this proposal is to be able prevent the display that ad, but do so in a manner that does not allow [publisher.example](http://publisher.example) to learn that I had been excluded (and thus converted for that `product_id`). In order to do so, [publisher.example](http://publisher.example) provides multiple ads to the browser, which can be referenced against the existing local AdExclusionGroup data. Note that we've also included one "fallback ad" that does not include a `product_id`; this would be shown if the person is excluded from all other ads provided.

```
<a exclusion="true">
  <a href="shop.example" product_id="7546c0903c23" campaign_id=42>
    <div>
      <img src="..." />
      <p>...</p>
      ...
    </div>
  </a>
  <a href="shop2.example" product_id="1f7f7c1dfbae" campaign_id=13>
    <div>
      <img src="..." />
      <p>...</p>
      ...
    </div>
  </a>
  ...
  <a href="toasters.com" campaign_id=51>
    <div>
      <video src="..." />
      <p>...</p>
      ...
    </div>
  </a>
</a>
```

Now, suppose that the browser's AdExclusionGroup data looks like the following, and the page load is happening on Jan 10, 2020:

```
{
    "shop.example": {
        "7546c0903c23": {"expiry": "2020-02-15T00:00:00Z", ...},
        "a6e8cecba12a": {"expiry": "2020-01-13T00:00:00Z", ...},
    },
    ...,
}
```

In this case, the second ad to [shop2.example](http://shop2.example) would be rendered by the browser, which *is determined by the browser*. The publisher learns nothing about which ad was rendered at this particular time, preventing them from learning the state of this particular browsers secure purchase storage.

## Design Considerations

### Overlap with TURTLEDOVE

Our proposal relies on many of the existing design elements in TURTLEDOVE, specifically

* [Browsers Joining Interest Groups](https://github.com/michaelkleber/turtledove#browsers-joining-interest-groups) (Exclusion Groups, in this proposal)
    * The only small modification is noted above, but the same concerns apply.
* [Rendering the Winning Ad](https://github.com/michaelkleber/turtledove#rendering-the-winning-ad)
    * "Winning Ad" means something slightly different in this context, but the concerns around hiding which ad is displayed apply.
* [Frequency Capping, Budgets, Metrics, and Reporting](https://github.com/michaelkleber/turtledove#frequency-capping-budgets-metrics-and-reporting)
    * All but Frequency Capping also apply here. We also believe that the Aggregate Reporting API is appropriate, and that it should enforce differential privacy.
    * See Possible Attacks below for one specific concern for reporting.
* [User Interface Controls](https://github.com/michaelkleber/turtledove#frequency-capping-budgets-metrics-and-reporting)
    * These controls should also apply to Exclusion Groups

### Possible Attacks

The premise of the proposal is to prevent the publisher from learning that a specific person has purchased a specific `product_id`. We leverage the idea of blind rendering to assure this at the time of ad display.

We must also consider this at the time of reporting. We see two possible attacks by which a malicious publisher could use a reporting mechanism in order to learn about a specific user's purchase.

The first is by generating a unique `product_id` for each user and product pair. Then, even through the Aggregate Reporting API with differential privacy applied, the appearance of a specific `product_id` would confirm membership. (False negatives would be easy to create in that API, but false positives would be nearly impossible.)

The second is a more subtle attack, but with the same objective. Instead of using a unique `product_id` for each user and product pair, we use a unique URL for the second "fallback" ad. In this case, seeing that URL in the final report would confirm membership. (Again, false negatives would be easy to create in that API, but false positives would be nearly impossible.)

There are two potential solutions here: limit the entropy of the reporting unit (`product_id` and destination URL) or ensure that the Aggregate Reporting API enforce both of the following:

* Some reasonable minimum number of impressions simply for being included, with a k-anonymity guarantee. This would prevent simply the appearance of a domain in the report at all as leaking private data.
* Ensure differential privacy on the aggregate counts that meet this threshold, to give provable privacy guarantees.

We believe the latter is more useful and practical. Limiting the entropy on `product_id` raises challenges as different advertisers have many orders of magnitude differences in the size of their inventory, and limiting the entropy of destination domains is similarly problematic.

## Extensions

This idea can be extended to provide more general exclusion targeting system:

* Users may be given the opportunity to add themself to an exclusion group related to an ad at the time the ad is being served, for products they have not purchased but wish to exclude for other reasons.
* Exclusion groups may exist at arbitrary levels of granularity with an ad being part of multiple groups. e.g. separate groups for the specific product, the product category, and the advertiser.
* Exclusion group membership for ads served by a third party:
    * Browsers would be able to exclude ads across the entire ad network without specifying publishers.
    * Publishers using a third party ad network would not have to run code in a first party context.
