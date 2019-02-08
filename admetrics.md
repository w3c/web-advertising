# Privacy protecting metrics for web audience measurement 

Accurate information about advertisement visibility is necessary for all the actors involved with advertising on the web. In situations where prior user consent is needed for personal data collection or terminal storage access, or where user agents implement tracking protection e.g. Safari's ITP, traditional cookie-based methods may not be available. User agents can gather this information easily so what is needed is a privacy protective way to record it at an external server responsible for “audience measurement”, i.e. in such a way that does not identify or "single-out" the user.

HTML elements, (e.g. div elements) could be declared with a new attribute associated with the visibility of the element. If the element is “viewable” by the user, defined by parameters to the attribute, an HTTP transaction could be triggered which cause an aggregation “count” to be incremented at a server responsible for metrics collection.

The data transportable in the HTTP transaction could be constrained so that the user can not be identified, i.e. they cannot be singled-out. No cookies or other identifying headers would be transmitted, and the target URL path would be restricted to a predefined and unmodifiable value.

For example, the following element would trigger a remote count to be incremented when at least 80% of the pixels of an advertisement (ignoring a 5px margin) is viewable for at least 5 seconds.
```
<div adm=“domain=metrics.audiencemeasurement.com;ad=AD-30045;trigger=1;margin=5px;threshold=0.8;after=5000“>

… advertisement content

</div>
```
When the trigger was activated an HTTP HEAD request would be sent.

```
HEAD https://metrics.example.com/.well-known/admetrics/AD-30045/1/ HTTP/1.1

Host: metrics.audiencemeasurement.com
Referer: publisher.com
```
An element can have multiple adm attributes so different triggers could be set for the same element, differentiated via the trigger parameter whose value restricted to a single decimal digit. The particular ad whose viewability is to be counted is indicated by the ```ad``` parameter. The ```ad``` parameter should be restricted in entropy, for example to 7 or 8 aphanumeric characters, to help mitigate fingerprinting attacks.

Cache headers could be set but there can be no response content.

The receiving server would simply increment the appropriate count, there being no cookies or other user identifying or linking information.


