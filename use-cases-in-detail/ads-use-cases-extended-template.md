---
title: A Use Case In Detail
date: 2020-06-01 10:05:00 -05:00
neededBy:
- Advertiser
- Ad Network
- Publisher
- User
proposedBrowserSupportFrom:
- Chrome
- Safari
- Firefox
- Edge
communityProposalsFrom:
- Facebook
- Criteo
- Foobar
proposalsAddressingUseCase:
- BIRDNAME
currentlyAddressed: true
consideredResolvedBy:
- Foobar
userFlowsAddressed:
- browser-same-device
authors: 
- Aram Zucker-Scharff (The Washington Post)
excerpt: The first paragraph should provide a summary of the use case and its context, starting with the content of the excerpt in the metadata.
metrics:
- Abstract 
isBasedOn: https://github.com/w3c/web-advertising/blob/main/ads-use-cases-template.md
---
# A Use Case in Detail

Use case detail documents should [be authored in Github-flavored Markdown](https://guides.github.com/features/mastering-markdown/).

The first paragraph should provide a summary of the use case and its context, starting with the content of the excerpt in the metadata.

## What is the use case?

Expand and give detail on the use case, what is the use case exactly? What is going on in this use case that can be defined, especially as a technical process. This may include subtitled areas giving chunks of information in more detail. 

### Metadata That lives here

If you define this use case as specific to a user flow then you should explain how it works within the user flow specified in the metadata as a `h2` or `#` header on the [Common User Flows document](https://github.com/w3c/web-advertising/blob/main/common-user-flows-in-web-advertising.md#browser--browser-same-device). 

If you define this use case as having specific metrics it relates to in the metadata header, explain the metric here and how it applies. 

## Why is it important to preserve this use case?

Describe why this feature is critical to effective web advertising relative to who it is needed by as follows: 

### Advertiser

Advertisers need to be able to clearly define use cases in order to have them addressed by various proposals clearly. 

### Ad Network

Ad Networks need to be able to clearly define use cases in order to have them addressed by various proposals clearly. 

### Publisher

Publishers need to be able to clearly define use cases in order to have them addressed by various proposals clearly. 

### User

Users need to be able to clearly define use cases in order to have them addressed by various proposals clearly. 

## How is it functionally achieved today?

Describe how this functionality is achieved today

## Proposed Support  

If there are any entries in this category, then the `currentlyAddressed` metadata should be considered `true` and proposal names should be listed in `proposalsAddressingUseCase`

### Browser Proposed Support  

Where this use case is addressed by a browser proposal list it here in the following format:

#### Browser Name

##### Proposal Name

How does this proposal address the use case in detail and/or link to external explanations and assets.

### Community Proposed Support

Where this use case is addressed by a community proposal list it here in the following format:

#### Community Member Name

##### Proposal Name

How does this proposal address the use case in detail and/or link to external explanations and assets.

## Considered Resolved By

If any member of this group considers this use case resolved by a proposal, they can detail how and why here in the following format:

### Community Member Name/Entity "Foobar"

#### BIRDNAME

Why I, representative of Foobar Inc, consider this to be resolved by BIRDNAME and how. 
