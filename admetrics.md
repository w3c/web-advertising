# Privacy protecting metrics for web audience measurement 

Accurate information about advertisement visibility is necessary for all the actors involved with advertising on the web. In situations where prior user consent is needed for personal data collection (or terminal storage access), traditional cookie-based methods may not be available. User agents can gather this information easily so all that is needed is a privacy protective way to record it at an “audience measurement” server.

HTML elements, (e.g. div elements) could be declared with a new attribute associated with the visibility of the element. If the element is “viewable” by the user, defined by parameters to the attribute, an HTTP transaction could be triggered which cause an aggregation “count” to be incremented at a server responsible for metrics collection.

The data transportable in the HTTP transaction is constrained so that the user can not be identified, i.e. they cannot be “singled-out”. No cookies or other identifying headers would be transmitted, and the target Url path would be restricted to a predefined and unmodifiable value.

For example, the following element would trigger a remote count to be incremented when at least 80% of the pixels of an advertisement (ignoring a 5px margin) is viewable for at least 5 seconds.
```
<div adm=“domain=metrics.audiencemeasurement.com;trigger=1;margin=5px;threshold=0.8;after=5000“>

… advertisement content

</div>
```

When the trigger was activated an HTTP HEAD request would be sent.

```
HEAD https://metrics.example.com/.well-known/admetrics/1/ HTTP/1.1

Host: metrics. audiencemeasurement.com

Referer: publisher.com

```
An element can have multiple adm attributes so different triggers could be set for the same element (up to 10). The triggers would be differentiated via the trigger parameter which can only contain a single digit decimal number.

Cache headers could be set but there can be no response content.

The receiving server would simply increment the appropriate count, there being no cookies or other user identifying or linking information.
