# Transparency via a Machine-readable Server Identity and Purpose Descriptor.

Mike O'Neill, Febuary 2019

©2019, Baycloud Systems Ltd. All rights reserved.

Web pages often contain many, sometimes hundreds, of elements that initiate transactions with servers other 
than those managed by the top-level website. 
These "third-party" servers can collect personal data, 
link it to data from other sources, and the user is usually completely unaware of this.

Unfortunately, there is no recognised standard way for web servers to declare this information
i.e. to deliver information that allow users to identify the entities, 
what their purpose(s) for data collection are (if any), 
who they share it with, how long they keep it etc.

There is increasing legal pressure around the world for websites to declare their use of data collection procedures, 
explain how they intend to use the data, or what their legal basis is. 
In some jurisdictions users have to be offered the right to have their previously collected data deleted, 
while in others prior consent is needed before data is collected.

In addition user agents have implemented procedures that by default restrict the ability of embedded sub-resources
to access cookies. Some of these sub-resources may be managed by the same entity managing the top-level site, 
or have previously been given explicit consent by the user. A machine-readable mechanism to record this could be useful.

The following is a possible JSON encoding that can deliver the required machine-readable information
so that a user agent can make it accessible by the user in an standardised and easily digestible way, 
and to act on user specified preferences.
 
The information would be obtained by sending a secure HTTP GET to the resource `/.well-known/privacy-declaration` relative to any origin. 
For example the data declaration for the domain [www.bigco.com](http://www.bigco.com) 
would be at <https://www.bigco.com/.well-known/privacy-declaration/> and return a JSON document with the Content-Type 
"application/privacy-declaration+json". 
Alternatively the objects defined here could be incorporated in an Origin Policy manifest "[Origin Policy](https://wicg.github.io/origin-policy/)" to minimise 
the number of round-trips required when accessing a resource.

User agents or script could automatically parse the information as a JavaScript object at a standard location 
e.g. navigator.privacyDeclaration, which could then be used to display human-readable information to users. 
First-party sites could ensure this was always available by using an open source JavaScript library, 
and to support this the privacy-declaration resource should support CORS 
([Cross-origin resource sharing](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing)), 
so it can be accessed via the appropriate cross-origin fetch or XHR.
 
JavaScript can examine the JSON encoded for the first-party then use the *otherParties* and *sameParties* 
arrays to fetch the correct privacy-declaration JSON resources from them (made possible because the third-party resources are CORS enabled).
The *sameParties* set of domains could 
identify sub-resources which can be trusted as "first-party" because they are managed by the entity that manages the top-level site. 
User agents can check that each origin in a set are referenced by the
other origins by their own privacy-declaration resource, i.e. that they all contain exactly the same "sameParties" set. 
It may be possible for top-level or parent documents to host external privacy-declarations as bundles of "[Signed HTTP Exchanges](https://tools.ietf.org/html/draft-yasskin-http-origin-signed-responses)", 
which would avoid user agents having to make extra round-trips to get them. See @mikewest's proposal for this in "[First-Party Sets](https://github.com/mikewest/first-party-sets)".

Other methods are possible to ensure that domains are related, for example there could be a link to information in TLS certificate or domain name registrar's whois entry.
There is also ongoing discussion about using DNS records to associate relatedness between domain names.

The privacy-declaration resource could be dynamically generated so that some properties could reflect different user agent states derived from the incoming HTTP Request.
For example, the server would examine incoming cookies or other headers in order to calculate the correct value of the "consented" property,
or the length of time before consent expires.

In future there should be some standardisation on a low-entropy request header signal, which could be an existing header in widespread use like DNT,
or a specific cookie name such at the IAB EU's `euconsent`. Another avenue could maybe be explored by extending the cookie "prefix" options
described in "[Cookies: HTTP State Management Mechanism draft-ietf-httpbis-rfc6265bis-02](https://tools.ietf.org/html/draft-ietf-httpbis-rfc6265bis-02#page-14)". 


### Root properties ###

| **Property**  | **Type**                       | **Description**                                                                                                          |
| ------------- | ------------------------------ | ------------------------------------------------------------------------------------------------------------------------ |
| name          | String                         | Recognisable & unique entity name e.g. "Google Inc."                                                                     |
| policy        | String(Uri)                    | Human readable HTML page explaining the entity’s privacy policy                                                          |
| storagePolicy | String(Uri)                    | Human readable HTML page explaining the terminal storage policy                                                          |
| about         | String(Uri)                    | Human readable HTML page describing the entity                                                                           |
| deleteData    | String(Uri)                    | A HTTP POST will cause all user agent data for this origin to be deleted, e.g. Clear-Site-Data header could be returned  |
| mayCollect    | Boolean                        | "false" declares that no data is collected, "true" if it may be collected                                                |
| mayShare      | Boolean                        | "false" declares no data will be shared with other entities                                                              |
| mayCombine    | Boolean                        | "false" declares that data is not combined or linked with data from other sources                                        |
| purposes      | Array of *PurposeType* Objects | Lists all the purpose for which data is collected                                                                        |
| storage       | Array of *StorageType* Objects | Lists the terminal storage items that may be utilised                                                                    |
| otherParties  | Array of Strings               | Lists the third-party domains of embedded resources that may appear on this page                                         |
| sameParties   | Array of Strings               | Lists the first-party domains of embedded resources, i.e. those managed by the same entity, that may appear on this page |

The user can give their agreement for zero or more *purposes*. 
The *purposeType* Object for a particular purpose includes a Boolean *consented* which can be dynamically derived from the incoming HTTP request headers (e.g. cookies).

The *storage* objects are linked to the specific purposes which they are designed to implement. 
This gives user agents fine grained ability to restrict storage use to the purposes a user has agreed to.

A browser, browser extension or script executing in the top-level browsing context can use the 
*otherParties* and *sameParties* array to fetch the Descriptors for those domain origins 
(by fetching the resource at https://{*domain name*}/.well-known/privacy-declaration.

### StorageType Object properties ###

| **Property** | **Type**         | **Description**                                                                                                                                       |
| ------------ | ---------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- |
| type         | String           | Storage Type, one of "cookie", "local" (localStorage), "indexed" (indexedDB), "cache" (ETag)                                                          |
| name         | String           | Cookie name prefix, localStorage item name, or indexedDB table                                                                                        |
| purposeList  | Array of Integer | List of ordinal values of entries in the "purposes" array. e.g. \[0,1\] indicates the first and second purpose type is supported by this Storage Type |

### PurposeType Object properties ###

| **Property**   | **Type** | **Description**                                                                   |
| -------------- | -------- | --------------------------------------------------------------------------------- |
| name           | String   | Short identifying label for this purpose                                          |
| description    | String   | A human readable text clearly describing this purpose in the appropriate language |
| maxRetainedFor | Integer  | Number of seconds data is retained after collection                               |
| expiresIn      | Integer  | Number of seconds remaining before collected data is deleted                      |
| consented      | Boolean  | Dynamic indication of registered user agreement for this purpose                  |

*An example of the encoding.*

```javascript
{

"name": "BigCo Inc",

"policy": "https://www.bigco.com/privacy.html",

"storagePolicy": "https://www.bigco.com/cookie.html",

"about": "https://www.bigco.com/about",

"mayCollect": "true",

"mayShare": "true",

"mayCombine": "false",

"purposes": [

{

   "name": "behavioural advertising",

   "description": "compiling history of web sites visited",

   "maxRetainedFor": "1000000",
 
   "expiresIn": "45667",

   "consented": "false"

},

{

   "name": "website analytics",

   "description": "web audience measurement",

   "maxRetainedFor": "10000",
 
   "expiresIn": "3456",

   "consented": "false"

},

{
 
   "name": "authentication",

   "description": "logging in",

   "maxRetainedFor": "1000000",
 
   "expiresIn": "67854",

   "consented": "false"
}

],

"storage": [

{

   "type": "cookie",
  
   "name": "_ga",

   "purposeList": ["0","1"]

},

{

"type": "cookie",

"name" "user",

"purposeList": ["2"]

},

{

    "type": "local",

    "name" "dataname",

    "purposeList": ["0"]

}

],

"otherParties": [
    "[www.google.com]",
    "[www.google-analytis.com]",
    "adnxs.com"
    ],

"sameParties": [
    "ourcdn.com"
    ]

}
```

## Prior Art
*   Mike West has proposed a way for origins to assert they belong to a set managed by the same top-level or "first party" resource "[First-Party Sets](https://github.com/mikewest/first-party-sets)" 

*   The Tracking Protection Working Group's "[Tracking Preference Expression (DNT)](https://www.w3.org/TR/tracking-dnt/)" defined a server transparency declaration at `/.well-known/dnt/`
    This was designed to allow the entity managing any server (first-party or subresource) to declare various properties to aid transparency.

*   John Wilander has proposed amendments to the Same Origin Policy so sets of domains could be trusted as if they were first-party. "[Single Trust and Same-Origin Policy v2](https://lists.w3.org/Archives/Public/public-webappsec/2017Mar/0034.html)"

*   There is ongoing discussion within the IETF about recognising domain name "relatedness" in DNS records "[Related Domains By DNS](https://datatracker.ietf.org/doc/draft-brotman-rdbd/)"

*   Cookie Name Prefixes are discussed in the under-development replacement for RFC 6265 "[Cookies: HTTP State Management Mechanism draft-ietf-httpbis-rfc6265bis-02](https://tools.ietf.org/html/draft-ietf-httpbis-rfc6265bis-02#page-14)"

*   The IAB EU's "[IAB Europe Transparency & Consent Framework](https://advertisingconsent.eu/)" defines an externally hosted JSON resource that identifies Advertising technology vendors and a set of defined purposes.

*   There is ongoing work defining an origin wide server Origin Policy Manifest file at a well-known location. "[Origin Policy](https://wicg.github.io/origin-policy/)".
  
