# Conversion Filters Proposal

Many ads shown on publisher websites direct people to large e-Commerce websites that sell a wide variety of unrelated products. Think of Amazon, Walmart, Target, Wish, etc. 

Both Webkit’s “Private Click Measurement” proposal, as well as Chrome’s “Conversion Measurement” proposal currently suggest that **_all_** conversion events on a given domain could match up with **_any_** ad click that directed a browser to that domain. This poses a significant problem for such commerce websites. It would actually be desirable to collect **_less_** information here.

## Use-Case 1: Collaborative Ads
Many producers do not sell directly to consumers via their own website. Instead, they sell their goods in stores and through large e-Commerce websites. As an example, let’s imagine a producer ShaveCo that manufactures shaving supplies. They sell these shaving supplies on e-Commerce platform MegaStore’s domain megastore.com. If they want to run ads promoting their shaving supplies, the destination of the ads would be megastore.com. 

In their current form, the private click measurement APIs would not tell ShaveCo how many people purchased shaving products after clicking on their ads, it would tell them how many people purchased **_anything at all_** on megastore.com after seeing their ads. Since people commonly buy a wide variety of products on these large e-Commerce websites, it’s very likely that these APIs will be counting totally unrelated transactions for things produced by other producers, in totally different categories! 

This information would be interesting to MegaStore (the other participant in this collaborative ads campaign) but not useful to ShaveCo. ShaveCo is only interested in conversion events that they consider relevant to a subset of the products available on megastore.com - their shaving supplies.

## Use-Case 2: Dynamic Product Ads
The large e-Commerce website also wants to run their own ads promoting their website. They might run ads promoting a particular set of products (e.g. School Supplies in a “Back to School” campaign). While it might be interesting to know how many people ever bought anything at all on their website after clicking on an ad, it is also valuable to ask “how many people bought **_school-supplies_** after clicking on the ad promoting school supplies?”. They may additionally wish to know “How often was the exact product advertised purchased as a result of the advertisement?”

## Proposal

Both conversion measurement APIs propose the addition of new attributes to the <a> tag representing the ad in order to invoke the API. We propose the addition of another optional attribute, which would specify some type of “filter” for the set of conversions this particular ad wants to count.

We are not suggesting any specific protocol for how these “filters” would be implemented, but here is a short list of common use-cases that will drive value:

- “Only count conversions where the product_id is 12345”
- “Do not count conversions where the product_id is 12345”
- “Only count conversions where the product is one of [12345, 23456, 34567, …]”
- “Only count conversions which occur on the sub-domain electronics.megastore.com”
- “Only count conversions that occur within the directory megastore.com/store/electronics”
- “Only count conversions where the product category is ‘school-supplies’”
- “Only count count conversions where the producer is ‘ShaveCo’”

Both conversion measurement APIs propose the introduction of certain conversion metadata which would be associated with the conversion event. The Webkit proposal also suggests a “priority” attribute. We propose that advertisers can additionally specify various meta-data about the conversion event (e.g. Producer: “ShaveCo”, product_id: “12345”, category: “Shaving Supplies”).

When the browser looks into the storage of clicks requesting attribution, it could compare the metadata on the conversion with the filter specified on that ad click. These would either “match” or “not-match”. There are two possible approaches that come to mind for what to do in the event that they do-not-match.

Do not attribute this conversion event to this click. Continue checking the other clicks that requested attribution, and if none match, do not generate an anonymous conversion report at all.
Attribute the conversion to the click, but set the first “conversion-metadata” bit to zero to indicate “did not match the specified filter”.

## Privacy Considerations

This proposal does not rely on any change to the total entropy contained in anonymous conversion reports, or how the bits are distributed. Platform vendors have already taken stances about how many bits to allow for representing both campaign_id as well as conversion_metadata, and this proposal should not affect those choices. Since this does not propose to change the bit entropy in conversion reports, we do not believe this proposal meaningfully impacts user privacy.

What this does do is allow websites with complex advertising campaigns and large product catalogs to more effectively utilize those bits of entropy to support common use cases.
