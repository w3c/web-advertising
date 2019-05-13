# Privacy protecting metrics for web audience measurement 

Mike O'Neill, February 2019

©2019, Baycloud Systems Ltd. All rights reserved.

The online advertisment ecosystem needs accurate information about how often advertisements are interacted with,
and when they are viewed. Advertisers need to know where and how often their ads are showing to real people, 
and publishers need to be paid when their properties deliver ads.

In situations where prior user consent is needed for personal data collection or terminal storage access,
or where user agents implement tracking protection e.g. Safari's ITP, 
traditional cookie-based methods may not be available. 

Even where they are available, the industry has been plagued by fraud where dedicated bot farms or remotely controlled co-opted devices 
create false ad viewing records, and lack of reliable metrics allow ads to appear alongside brand damaging content. 


### Browser based ad metrics gathering

Browsers can collect the metrics required by advertisers relatively easily,
and are also in the best position to determine if real people are seeing or interacting with them.
The proportion of pixels in view can be measured to trigger viewability events, and user interaction or conversion can be detected
over arbitrary periods of time.  

The risk to privacy only occurs when data related to particular individuals is communicated. 
As long as the data is provably inaccessible to others, it makes little difference to the individual that data about them is held within their browser.

What is needed is a privacy protective and secure way to communicate this information, 
making aggregated statistics derived from it available for recording
by advertisers and external “audience measurement” servers, in such a way that the user can not be identified or "singled-out".

In addition the protocol used to communicate the data must allow the receiving server (or servers) to detect with some certainty 
whether the data relates to a real human being, i.e. to detect ad fraud.
Because the protocol is purpose designed it should be possible to deliver fraud detection capability with at least an 
order of magnitude improvement over what is attained at present.

### Ads identified in markup

HTML elements, (e.g. div elements) containing advertising content would be declared within an `ad` attribute identifying it as such to the user agent. 
This attribute informs the user agent about what user interaction events such as visibility are to be measured, and to what metrics servers
event data should be sent. 

Metrics servers are defined by their domain origin, the user agent appending to it a fixed path `.well-known/admetrics` and a query string 
to convey the minimum level of event information required to increment the appropriate data count at the metrics server.

Browser providers may choose to add a reference to a controlled or externally recognised metrics 
server to act as an overall check of the validity of the aggregated metrics.

User agents ensure that the target URL is formed by limiting the entropy delivered within it, 
and that no cookie values or other user identifying information is sent, 
ensuring that the data to be transported cannot be used to track an individual user. 
The user agent would queue the metrics data associated with an event for later transmission.
At appropriate intervals, 
perhaps sometime after the interactions the data refers to, 
the events in the store are processed, filtered, and assembled into HTTP HEAD requests to be sent to the appropriate servers.

The data transported by the HTTP request must be restricted so that the user can not be identified, 
i.e. they cannot be singled-out. 
No cookies, or any other identifying headers such as Basic Authentication or client certificates, would be transmitted, 
and the target URL path would be restricted to a predefined and unmodifiable value in the IANA defined `.well-known` space.

For example, a site with origin `example.com` would mark an element to trigger a remote count to be incremented when at least 80% of the pixels of 
an advertisement identified by `id=30045`
is actually viewable by a real user for at least 5 seconds (ignoring a 5px margin).

```
<div ad=“domain=audiencemeasurement.com;id=30045;triggertype=viewable;triggerKey=42;margin=5px;threshold=0.8;after=5000“>

… advertisement content

</div>
```

When the view event is triggered parameters identifying the ad and its trigger are queued for transmission
along with the current date and time, and perhaps other commercially relevant values such as the `origin` of the top-level or parent frame, 
but ensuring that their is insufficient entropy in the combined data to track the user. 
In addition proof that the originator is a legitimate browser operated by a real human user is included via the `browser` parameter, 
described below.

```
HEAD https://audiencemeasurement.com/.well-known/admetrics?triggerKey=42;origin=example.com;browser=a3d24b56789 HTTP/1.1

Host: metrics.audiencemeasurement.com
```

An element can have multiple `ad` attributes so different triggers could be set for the same element, 
differentiated via the `triggerKey` parameter whose value is restricted to 2 decimal digits. 
The particular ad whose viewability is to be counted is indicated by the `ad` parameter. 
The `id` parameter should be restricted in entropy, for example to 7 or 8 alphanumeric characters, 
but have, combined with the domain origin of the top-level context, sufficient granularity to identify a particular advertisement.
There can be multiple `domain` parameters so that metrics information can be sent to multiple destinations,
which could include the entity ultimately paying for the ad.


The receiving server would check the `browser` parameter and, if it is validated, 
simply increment the appropriate count in an aggregated metrics table, 
there being no cookies or other user identifying or linking information.

The `browser` parameter value is a string resulting from encrypting a 
message with a suitable asymmetric cryptographic function using a public key associated with the browser version
or embedded in the browser executable.
The message contains a current time-stamp and an arbitrary string guaranteed to be unique to every browser instance.
This cyphertext string will be different for every transaction because the underlying time-stamp will be different, 
so cannot be used by servers for fingerprinting.
 

The receiving server at some point will send a copy of the `browser` parameter 
via a secure REST transaction to a server managed by the browser provider, 
which will decrypt the cyphertext using its private key, 
and respond with a single boolean indicating whether or not the string is properly associated with an installed browser instance.
If the response is `false` the event is silently discarded. 
The browser provider's server should keep a tally of the id strings to detect if the same id is being reported by a suspicious number of event instances,
and mark that id as bad and always respond with `false` to metrics server requests. 

An HTTP HEAD request would be sent, the user agent ensuring that no cookies or other identifying headers are included.




### Browser installation sets a stenographic instance id and a public key

The browser installation process will be responsible for creating a unique instance string 
to be stenographically contained within the user agent's executable image, 
and for retaining a copy in a database managed by the browser provider.
 
The instance-unique value would be calculated when the browser is installed or updated, 
and the browser provider would retain a record of its use. 
It would be assigned at random and be of sufficient length to mitigate against a brute force attack.
The would also be an embedded string representing the browser provider's public key.
Metrics servers would deliver the cyphertext as it is received in the `browser` parameter
of each received ad event to servers managed by the browser provider via a secure REST transaction, 
who would then return a single boolean value indicating the validity of the enclosed instance-unique string.

Browser providers could also execute propitiatory processes to detect illicit users of their installation or validation processes, e.g. monitoring the IP source address of its initiators.
This would not need to be standardised as long as no interworking is envisioned. 
If illicit use is detected a unique string would still be returned, 
but it would not be recorded as valid in the provider's database. 
This would avoid alerting the illicit user that their fraud technique had been recognised.

No other information would be passed between the metrics and browser provider validation servers, ensuring the user cannot be tracked.

## Prior Art
*   John Wilander has proposed, and Apple's Safari and Mozilla's Firefox browsers have implemented, or are in the process of implementing, sophisticated controls over tracking cookies. "[Intelligent Tracking Prevention 2.2](https://webkit.org/blog/8828/intelligent-tracking-prevention-2-2/)" 

 





 

