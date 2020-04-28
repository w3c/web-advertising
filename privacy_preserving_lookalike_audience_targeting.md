# Privacy Preserving Lookalike Audience Targeting

A common problem all businesses face is finding new customers. One highly effective approach is to try to find people who are similar to the business’s existing customers. These people are significantly more likely to be interested in the business’s products than a randomly selected person. This use-case is especially critical for small businesses that do not have large ad budgets, and cannot afford to spend money for a long period of time on broadly targeted ads until the system learns which type of people are most likely to engage. Small businesses need to ensure none of their limited marketing budget goes to waste.

This proposal aims to find a mechanism for achieving these goals that is not at odds with Webkit’s “[Tracking Prevention Policy](https://webkit.org/tracking-prevention-policy/)”, or Chrome’s proposed “[Privacy Model for the Web](https://github.com/michaelkleber/privacy-model)”. This seems like it should be possible, because we only need to learn about aggregate properties of a large group of people; we do not need to track specific individuals. 

# An Observation

It seems that one could potentially serve this use-case with just a minor extension to Google Chrome’s proposed “[Aggregated Reporting API](https://github.com/csharrison/aggregate-reporting-api)”. The current proposal [only contemplates using integers within a bounded range as inputs](https://github.com/WICG/conversion-measurement-api/blob/master/SERVICE.md#input). If we could just extend this to support vectors of numbers, we think a privacy-preserving solution to “Lookalike Targeting” might be within reach.

# Background Context: Embedding Vectors

If you are not familiar with embeddings, let this serve as a brief introduction. If you are already familiar with the concept, you can just skip down to the next section.

For any type of input (e.g. Text, or an Image, or an ad, or a user of a website) we can imagine a neural network of some width and height that is trained to predict a binary classification. 

![diagram showing a multi-layer neural network feeding into a single node at the top](https://github.com/w3c/web-advertising/blob/master/images/Privacy%20Preserving%20Lookalike%20Audience%20Targeting%201.png)

If the activation of this final prediction node is over some threshold (say 0.5) we will predict category “true”, otherwise we will predict “false”.

Now, once we have trained such a network on a vast number of training examples, the interesting innovation is to just remove the final “prediction node” and look at the activation over the neurons in the final layer.

![diagram showing a multi-layer neural network feeding without a single node at the top, instead showing floating point numbers representing activation levels in the final layer of neurons](https://github.com/w3c/web-advertising/blob/master/images/Privacy%20Preserving%20Lookalike%20Audience%20Targeting%202.png)

Any particular input will result in some type of activation across the neurons in this final layer. We can think of this as a vector of floating point numbers. In the illustration above, the vector would be 8 long, and have the value: <0.2, -0.7, 0.3, 0.9, -0.8, -0.1, 0.0, 0.1>

Now, due to the way that neural networks work, “similar” inputs will result in similar activation levels for neurons in this final layer. We can think of each neuron in the final layer as a sort of “concept”, and the collection of these “concepts” taken together as some type of conceptual representation of the input. As such, we can think of an embedding as a “thought vector in concept space” to draw from Geoffrey Hinton.

If we can compute the “embedding” for two items, we can now compute how “similar” they are. We can just compute the cosine of the angle between the two vectors. Two vectors that are pointing in exactly the same direction will have an angle of 0° between them, and the cosine of 0° is one. Two vectors that are orthogonal to one another will have an angle of 90° between them, the cosine of which is zero.

Computationally, this is very efficient to do. All we need to do is take the dot-product of the two vectors, and divide by the magnitude of each vector. See the Wikipedia entry on “[Cosine Similarity](https://en.wikipedia.org/wiki/Cosine_similarity#Definition)”.

![mathematical formula for cosine similarity: similarity = cos(theta) = (A dot B) / (magnitude(A) * magnitude(B))](https://github.com/w3c/web-advertising/blob/master/images/Cosine%20Similarity.png)

# High Level Proposal

We assume that a publisher website is able to collect first-party data about its own users. From this first-party data, we presume the publisher is able to train a neural network which can be used to produce an “embedding vector” for each user, that somehow represents “similarity” between these users in a way that has value to the publisher. One potential approach might be to train an embedding based on ad-clicks, such that users who click on similar ads have similar embeddings. This is just one example, and not necessarily an optimal strategy.

## Step 1:

When a person visits the publisher website, we send the person’s embedding to the “write-only per-origin data store” described in Chrome’s proposed “[Aggregated Reporting API](https://github.com/csharrison/aggregate-reporting-api)”. This has the desired property of being private to the origin (no other domains can read from it), and of not enabling user-tracking (it is write-only).

Again, this will require a minor adjustment to [the current proposal](https://github.com/WICG/conversion-measurement-api/blob/master/SERVICE.md#input) to ensure that vectors of numbers can be supported as a primitive, not just integers within a bounded range.

## Step 2:

When this person later visits the advertiser’s website in this browser, some 3rd party JavaScript provided by the publisher should be invoked to flush this data to a server-side aggregation service. Again, this should already be supported by Chrome’s proposed “[Aggregated Reporting API](https://github.com/csharrison/aggregate-reporting-api)”. 

## Step 3:

This server-side aggregation service would, over a period of time, receive embedding vectors for all of the visitors of the advertiser website who were previously assigned an embedding vector by the publisher. The server-side aggregation service would aggregate all of these embedding vectors together in some way (Sum? Average? Sum and then normalize the vector length?).

Again, this may require a small extension to the current API proposal to ensure that vector sums / vector averages / normalized vector sums are supported by the server-side aggregation service. 

## Step 4:

The publisher could then request this aggregated vector via some kind of API. The server-side aggregation service would add some small amount of noise to the aggregation to ensure a target ε-differential privacy. With the exception of supporting vector primitives, this is exactly what is described in [Chrome’s current explainer](https://github.com/WICG/conversion-measurement-api/blob/master/AGGREGATE.md#final-private-output). 

## Step 5:

The publisher would at this point have learned something about the aggregated properties of the visitors of the advertiser website. It could utilize this aggregated data to enable “Lookalike Targeting” on the publisher website. It could compute the similarity of each user to the aggregated embedding vector, and set some kind of minimum similarity threshold before showing an ad.

# A more advanced approach: train a logistic regression model

While summing up the embedding vectors is simple to describe, and might have some utility, we think that we could get much more utility for the same value of ε-differential privacy if we could extend Chrome’s proposed “[Aggregated Reporting API](https://github.com/csharrison/aggregate-reporting-api)” to support logistic regression.

In Step 3, the aggregation servers would still receive the embedding vectors of users who visited the advertiser website. These would all be “positive” training samples. We could randomly select browsers that did *not* visit this website, and use their embeddings as “negative” training samples. This would result in a table of data like this:

| vec[0] | vec[1] | vec[2] | vec[3] | vec[4] | vec[5] | vec[6] | vec[7] | label |
|--------|--------|--------|--------|--------|--------|--------|--------|-------|
| 0.2 | -0.7 | 0.3 | 0.9 | -0.8 | -0.1 | 0.0 | 0.1 | 1 |
| 0.8 | -0.1 | 0.5 | -0.2 | -0.6 | 0.9 | -0.4 | -0.3 | 0 |
| -0.2 | -0.9 | 0.4 | 0.6 | -0.2 | 0.3 | 0.2 | -0.1 | 1 |

Now we would need the aggregation server to train a logistic regression on this data for each advertiser’s website. The result of this training data would still be a single vector of weights with the same length as the embedding vectors provided. It would still need some small amount of noise added to it to provide a given ε-differential privacy. 

If Chrome is open to this approach, we think it would be quite promising. While logistic regression is more complex than simple sums, it does lend itself quite well to MPC and should be possible to compute in a relatively performant manner.

## Privacy Considerations

The user embedding computed by the publisher is private information. It is a derivative of private user data and must be treated with great care. We must assess the privacy characteristics of Chrome’s proposed “[Aggregated Reporting API](https://github.com/csharrison/aggregate-reporting-api)” from this perspective. 

Who will have access to read this embedding? Will malicious browser extensions or malicious mobile apps be able to read it? Once it is sent to the aggregation service, will any helper node be able to read it? 

It is also important to consider the privacy of the aggregate result computed from the embedding vectors of many users. Can the aggregation servers read this aggregate value before or after the addition of differential noise? Could a malicious helper node exfiltrate this trained model and from it learn information about the advertiser and/or publisher?

Given the answers to the above questions, is it necessary for the publisher to add differential noise to these embedding vectors to protect user privacy? Hopefully not, as the addition of noise at the input level would significantly reduce utility.

## Offline analysis

This is just a proposal. At this time, we do not know how much utility this approach would provide. We would need to do a significant amount of work to test this. To do this type of offline analysis, we need a simplified model of how global differential privacy would be added, and a target value of ε so that we can simulate it. 


