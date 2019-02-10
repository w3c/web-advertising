# Privacy protecting metrics for web audience measurement 

The online advertisment ecosystem needs accurate information about advertisement visibility. 
Advertisers need to know where and how often their ads are showing to real people, 
and publishers need to be paid when their properties deliver ads.

In situations where prior user consent is needed for personal data collection or terminal storage access,
or where user agents implement tracking protection e.g. Safari's ITP, 
traditional cookie-based methods may not be available. 

Even where they are available, the industry has been plagued by fraud where dedicated bot farms or remotely controlled co-opted devices 
create false ad viewing records, and lack of reliable metrics allow ads to appear alongside brand damaging content. 

User agents can gather information about ad viewability relatively easily, 
and are also in the best position to determine if real people are seeing them.
What is needed is a privacy protective and secure way to communicate this information, making it available for recording
by advertisers, publishers and external “audience measurement” servers, in such a way that does not identify or "single-out" the user.

### Data encrypted using stenographic secret key
The proposal is that user agent executables contain hidden bit-strings to be used as a semi-private encryption key, 
usually only accessible to internal code, i.e. the code entry points are designed not to be externally discoverable easily. 
Of course a determined adversary will eventually discover it, 
so the user agent should periodically update its code image, including at least new values for the encrypting bit-strings,
as often as is practicable.
The associated decryption keys are also semi-private in that they are only communicated to companies who have identified themselves 
to the user-agent developer, and are updated over a secure channel whenever the user agent is upgraded.
 
Each encryption key has a short id, perhaps calculated using a one-way hash function, which is transmitted in clear text
allowing the receiver to identify the correct decryption key if it has it. 
The receiver always accepts the messages silently, with no indication given if the cypher text cannot be decrypted, 
making it harder for an adversary to reverse engineer the user agent to identify the secret encryption key.

HTML elements, (e.g. div elements) could be declared with a new attribute associated with the visibility of the element. 
If the element is “viewable” by the user, defined by parameters to the attribute, 
an HTTP transaction could be triggered which cause an aggregation “count” to be incremented at a server responsible for metrics collection.

The data transported by the HTTP request could be constrained so that the user can not be identified, 
i.e. they cannot be singled-out. 
No cookies or other identifying headers would be transmitted, 
and the target URL path would be restricted to a predefined and unmodifiable value.

For example, the following element would trigger a remote count to be incremented when at least 80% of the pixels of 
an advertisement identified by `AD-30045`
(ignoring a 5px margin) is viewable for at least 5 seconds.

```
<div adm=“domain=metrics.audiencemeasurement.com;key=3a42;ad=AD-30045;trigger=1;margin=5px;threshold=0.8;after=5000“>

… advertisement content

</div>
```

When the trigger was activated the parameters identifying the ad and its trigger are added by the user agent to a digest which also 
includes the current date and time, and perhaps other values relevant for commerce such as the `origin` of the parent frame. 

The digest is then encrypted using the currently active secret key, 
say for this example this results in the bit-string `a3d24b56789`. 
This string will be different for every transaction because it includes an updated time value, so cannot be used by any servers for fingerprinting.
The `ad` parameter should be restricted in entropy, for example to 7 or 8 alphanumeric characters, 
to help mitigate fingerprinting by bad actors who have illicitly obtained decryption keys. 

The receiving server can sanity check the decrypted message, ensuring it has a valid time value etc. If it fails it is silently discarded.

An HTTP HEAD request would be sent including the key identifier and the encrypted payload. 
The user agent ensures that no cookies or other identifying headers are sent.

```
HEAD https://metrics.example.com/.well-known/admetrics/?key=3a42;payload=a3d24b56789 HTTP/1.1

Host: metrics.audiencemeasurement.com
```
An element can have multiple adm attributes so different triggers could be set for the same element, 
differentiated via the trigger parameter whose value is restricted to a single decimal digit. 
The particular ad whose viewability is to be counted is indicated by the `ad` parameter. 
There can be different `domain` parameters so that metrics information can be sent to multiple destinations,
which could include the entity ultimately paying for the ad.
 


The receiving server would simply increment the appropriate count, 
there being no cookies or other user identifying or linking information.


