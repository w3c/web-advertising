# Privacy protecting metrics for web audience measurement 

Mike O'Neill, February 2019

©2019, Baycloud Systems Ltd. All rights reserved.

The online advertisment ecosystem needs accurate information about how often advertisements are interacted with,
and when they are viewed. Advertisers need to know where and how often their ads are showing to real people, 
and publishers need to be paid when their properties deliver ads.

In situations where prior user consent is needed for personal data collection or terminal storage access, 
e.g. where laws such as the EU's GDPR and ePrivacy Directive apply,
or where user agents implement tracking protection e.g. Safari's ITP, 
traditional cookie-based methods may not be available. 

Even where they are available, the industry has been plagued by fraud where dedicated bot farms or remotely controlled co-opted devices 
create false ad viewing records, and lack of reliable metrics allow ads to appear alongside brand damaging content. 

### Definitions

**Metrics Database**. A database of aggregated statistics derived from **Ad Metrics Messages** and guaranteed to contain no personal data about individuals.

**Ad Metrics Messages**. Data items assembled into a URL query component indicating countable events to be aggregated into a **Metrics Database**, 
delivered over a secure HTTP connection with no cookies or other user identifying headers.
 
**Metrics Server**. Server{s} that receives ad metrics reporting messages from user agents, 
asks a **Validation Server** if they are from a properly installed browser instance, 
and if they are increment the appropriate counts in an aggregated **Metrics Database**.

**Unique Browser Identification Database**. A Database updated by the browser provider's installation and update processes. 
It contains an entry containing the **Unique Browser ID** of every properly installed browser instance. 
If browser providers suspect that a particular string is associated with a bot or fraudulent user its entry is removed.

**Unique Browser ID**. Unique string to be embedded into each properly installed browser instance. 
Its existence and/or value is never reported to external software i.e. by a JavaScript API.

**Validation Server**. Server that supports an authenticated API to allow contracted **Metrics Servers** to determine
if the **Ad Metrics Messages** it has received are from a properly installed user agent,
by referring to its **Unique Browser Identification Database**.

### Browser based ad metrics gathering

Browsers can collect the metrics required by advertisers relatively easily,
and are also in the best position to determine if real people are seeing or interacting with them.
The proportion of pixels in view can be measured to trigger viewability events, and user interaction or conversion can be detected
over arbitrary periods of time.  

What is needed is a privacy protective and secure way to communicate this information, 
making aggregated statistics derived from it available for recording
by advertisers and external “audience measurement” servers, in such a way that the user can not be identified or "singled-out".

The risk to privacy only occurs when data related to individuals is collected and processed.
As long as the metrics data is aggregated in counts relating to a big enough number of people,
and it is impossible to extract data related to the component individuals, the risk to their privacy is insignificant.

In addition the protocol used to communicate the data must allow the receiving server (or servers) to detect with some certainty 
whether the data relates to a real human being, i.e. to detect ad fraud.
Because the protocol is purpose designed it should be possible to deliver fraud detection capability with at least an 
order of magnitude improvement over what is attained at present.


### Ads identified in markup

HTML elements, (e.g. div elements) containing advertising content would be declared within an `ad` attribute identifying it as such to the user agent. 
This attribute informs the user agent about what user interaction events such as visibility are to be measured, and lists the metrics servers
where event data should be sent. 

Metrics servers are defined by their domain origin, the user agent appending to it a fixed path `.well-known/admetrics` and a query string 
to convey the minimum level of event information required to increment the appropriate data count at the metrics server. 
The domain origin should be a top-level domain i.e. arbitrary CNAMEs (subdomains) would not be allowed.

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
No cookies, or any other identifying headers such as Basic Authentication or client certificates, would be transmitted. 
To stop data being incorporated into URLs the target path component is restricted to a predefined and unmodifiable value in the IANA defined `.well-known` space.

For example, a site with origin `example.com` would mark an element to trigger a remote count to be incremented when at least 80% of the pixels of 
an advertisement identified by `id=A45`
is actually viewable by a real user for at least 5 seconds (ignoring a 5px margin).

```
<div ad=“recordDomain=audiencemeasurement.com;id=A45;triggerIype=viewable;triggerKey=42;margin=5px;threshold=0.8;after=5000“>

… advertisement content

</div>
```

When the view event is triggered parameters identifying the ad and its trigger are queued for transmission
along with the current date and time, and perhaps other commercially relevant values such as the `origin` of the top-level or parent frame, 
but ensuring that there is insufficient entropy in the combined data to track the user. 
In addition proof that the originator is a legitimate browser operated by a real human user is included via the `browser` parameter, 
described below.

An HTTP HEAD request would be sent, i.e. no request or response entity, the user agent ensuring that no cookies or other identifying headers are included.

```
HEAD https://audiencemeasurement.com/.well-known/admetrics?id=A45;triggerKey=42;origin=example.com;browser=a3d24b56789 HTTP/1.1

Host: audiencemeasurement.com
```

An element can have multiple `ad` attributes so different triggers could be set for the same element, 
differentiated via the `triggerKey` parameter whose value is restricted to 2 decimal digits. 
The particular ad whose viewability is to be counted is indicated by the `ad` parameter. 
The `id` parameter should be restricted in entropy, for example to 3 alphanumeric characters, 
but have, combined with the domain origin of the top-level context, sufficient granularity to identify a particular advertisement.
There can be multiple `domain` parameters so that metrics information can be sent to multiple destinations,
which could include the entity ultimately paying for the ad.


The receiving server would check the `browser` parameter and, if it is validated, 
simply increment the appropriate count in an aggregated metrics table, 
there being no cookies or other user identifying or linking information.

The `browser` parameter value is a string resulting from encrypting a 
message with a suitable asymmetric cryptographic function using a public key associated with the browser version
or embedded in the browser executable. 
The message contains a current time-stamp plus a string which is unique to each browser instance, created when the browser is installed or updated.
The private key used to decrypt the message is not accessible to the metrics server, so the unique browser instance string
is invisible to it. The message is encrypted afresh, with a different time-stamp, before every upload to the metrics server.

This cyphertext string will be different for every transaction because the underlying time-stamp will be different, 
so cannot be used by metrics servers for fingerprinting (they cannot decrypt the cyphertext), 
and the ad information is not sent to the instance validation servers.
 

The receiving server at some point will send a copy of the `browser` parameter 
via a secure REST transaction to an instance validation server (IVS) managed by the browser provider, 
which will decrypt the cyphertext using its secret private key, 
and respond with a single boolean indicating whether or not the string is properly associated with an installed browser instance.
If the response is `false` the event is silently discarded by the metrics server. 
No other information is passed to the instance validation server.
The browser provider's server should keep a tally of the id strings to detect if the same id is being reported by a suspicious number of event instances,
and mark that id as bad and always respond with `false` to metrics server requests. 

### Browser human/bot discrimination

The test to distinguish a human user from a robot would be done at install time, 
whenever the browser executable is updated, or at other times determined by the browser provider. 
An external service with suitable privacy guarantees could also be employed.
The browser installation or update process would be responsible for creating a unique instance string 
to be stenographically embedded within the user agent's executable image, 
and for retaining a copy in a **Unique Browser Identification Database** managed by the browser provider, 
and accessed only by the **Validation Server**.  
The unique string would be assigned at random and be of sufficient length to mitigate against a brute force attack.
There would also be an embedded string representing the browser provider's public key.

**Metrics Servers** would check the cyphertext as it is received in the `browser` parameter 
of each received ad event by delivering it to **Validation Servers** managed by the browser provider via an authenticated and secure HTTP transaction, 
which would respond with a single boolean value indicating the validity of the enclosed instance-unique string. 
Only the **Validation Server**, i.e. provided by the browser provider, 
would have access to the private key and would never return or receive any personal data,
other than that this instance-unique value is properly associated with an install or update event. **Validation Servers** would 
only respond to properly recognised and authenticated **Metrics Servers**.

Browser providers could also execute proprietary processes to detect illicit users of their installation or validation processes, 
e.g. monitoring the IP source address of its initiators.
This would not need to be standardised as long as no inter-working is envisioned. 
If illicit use is detected a unique string would still be returned, 
but it would not be recorded as valid in the provider's database. 
This would avoid alerting the illicit user that their fraud technique had been recognised.

No other information would be passed between the **Metrics Servers** and browser provider managed instance **Validation Servers**, 
ensuring the user cannot be tracked.

### Recording intermediate results

Browsers would have to record audience categorisation data or intermediate events 
in order to detect complex user interaction with advertisements such as conversion. 
To maintain the commitment not to communicate personal data this internal state should
be inaccessible to external APIs such as JavaScript.

Advertisement markup, e.g. further parameters to the `ad` attribute would then allow for metrics events to set internal boolean state variables, 
and define conditional statements so that **Ad Metrics Messages** can be created with input from previously recorded state.

### Signaling conversion from sites/browsing contexts to user agents.

Audience categorisation data, conversion events and other ad related data often needs to be communicated from a visited site,
or from an associated browsing context. This could be done via an HTTP cookie which is deleted by the user agent as soon as it has detected it.
The cookie could have a prefix name of __ADSignal, and the data could be the data to be communicated. 
This technique could align well with user consent mechanisms whereby cookies 
from all but specific origin are inhibited, only detectable when the user has agreed.



### Prior Art
*   John Wilander has proposed, and Apple's Safari and Mozilla's Firefox browsers have implemented, or are in the process of implementing, sophisticated controls over tracking cookies. "[Intelligent Tracking Prevention 2.2](https://webkit.org/blog/8828/intelligent-tracking-prevention-2-2/)"
*   Mike West has proposed that embedded resources should have to explicitly declare cross-origin cookies. "[Incrementally Better Cookies](https://mikewest.github.io/cookie-incrementalism/draft-west-cookie-incrementalism.html)"
*   Also, he has proposed a mechanism which allows HTTP servers to maintain stateful sessions with user agents, 
which could ultimately replace cookies. 
"[HTTP State Tokens](https://mikewest.github.io/http-state-tokens/draft-west-http-state-tokens.html)"
*   The Accessible Platform Architectures Working Group has published a paper on human/bot discrimination techniques. "[Inaccessibility of CAPTCHA](https://w3c.github.io/apa/captcha/)" 
  



 





 

