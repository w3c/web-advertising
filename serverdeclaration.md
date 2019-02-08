# Advertisement Transparency via a Machine-readable Server Identity and Purpose Descriptor.

Web pages often contain many, sometimes hundreds, of elements that initiate transactions with servers other than those managed by the visited website. These “third-party” servers can collect personal data, link it to data from other sources, and the user is usually completely unaware of this.

Unfortunately, there is no recognised standard way for web servers to declare this information i.e. to deliver information that allow users to identify the companies or entities, what their purpose(s) for data collection are (if any), who they share it with, how long they keep it etc.

Websites in Europe must obtain user consent for tracking, including by third-parties, and this consent must be “freely given, specific and informed” so only valid when the user has been given clear and accurate information. Attempts have been made to have the relevant information either curated by the site or a service provider for the site, or by using a publicly available resource such as the IAB-EU Transparency and Consent Framework (TCF) vendor list. The former method is difficult because the individual third-party details is difficult to discover, and not guaranteed to be current. The TCF vendor list has the advantage that the information is supplied by the third-party entity that collects the data, but unfortunately is not associated with the web domains in a way independently discoverable by the user or their user agent.

Servers could be encouraged to supply the information by the need to comply with regulation (e.g. A.12/A.13 of the GDPR), or even better by user agent enforcement i.e. user agents (or script executing in the top-level context) could act on it to deliver it in an intelligible way to users and help protect user privacy by selectively blocking or inhibiting the action of third-party resources which do not make the transparency information available and where the user has not given their consent to the relevant purposes.

The following is a possible JSON encoding of the necessary information. The could be obtained by sending a secure HTTP GET to the resource /.well-known/privacy-info relative to any origin. For example the data declaration for the domain [www.bigco.com](http://www.bigco.com) would be at <https://www.bigco.com/.well-known/privacy-info/> and returned a JSON document with the Content-Type “application/privacy-info+json“.

Browsers or browser extensions could automatically parse the information and present it as a Javascript object at a standard location e.g. navigator.privacyInfo, which could then be decoded and used to display intelligible information to users. First-party sites could ensure this was always available by using an open source javascript library, and to support this the privacy-info resource should support CORS ([Cross-origin resource sharing](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing)). Javascript can examine the JSON encoded for the first-party then use the *otherParties* and *sameParties* (denotes domains managed by the same entity managing the first-party domain) arrays to fetch the correct privacy-info JSON resources from them (possible because the third-party resources are CORS enabled).

The resource should be dynamically generated so that each user agent would can ascertain its own consent status. The server would examine incoming cookies or other headers in order to calculate the correct value of the “consented” property, for example.

Root properties

| **Property**  | **Type**                       | **Description**                                                                                                          |
| ------------- | ------------------------------ | ------------------------------------------------------------------------------------------------------------------------ |
| name          | String                         | Recognisable & unique entity name e.g. “Google Inc.”                                                                     |
| policy        | String(Uri)                    | Human readable HTML page explaining the entity’s privacy policy                                                          |
| storagePolicy | String(Uri)                    | Human readable HTML page explaining the terminal storage policy                                                          |
| about         | String(Uri)                    | Human readable HTML page describing the entity                                                                           |
| mayCollect    | Boolean                        | “false” declares that no data is collected                                                                               |
| mayShare      | Boolean                        | “false” declares no data will be shared with other entities                                                              |
| mayCombine    | Boolean                        | “false” declares that data is not combined or linked with data from other sources                                        |
| purposes      | Array of *PurposeType* Objects | Lists all the purpose for which data is collected                                                                        |
| storage       | Array of *StorageType* Objects | Lists the terminal storage items that may be utilised                                                                    |
| otherParties  | Array of Strings               | Lists the third-party domains of embedded resources that may appear on this page                                         |
| sameParties   | Array of Strings               | Lists the first-party domains of embedded resources, i.e. those managed by the same entity, that may appear on this page |

The user can give their agreement for zero or more *purposes*. The *purposeType* Object for a particular purpose includes a Boolean *consented* which can be dynamically derived from the incoming HTTP request headers (e.g. cookies).

The *storage* objects are linked to the specific purposes which they are designed to implement. This gives user agents fine grained ability to restrict storage use to the purposes a user has agreed to.

A browser, browser extension or script executing in the top-level browsing context can use the *otherPartie*s and *sameParties* array to fetch the Descriptors for those domain origins (by fetching the resource at https://{*domain name*}/.well-known/privacy-info.

StorageType Object properties

| **Property** | **Type**         | **Description**                                                                                                                                       |
| ------------ | ---------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- |
| type         | String           | Storage Type, one of “cookie”, “local” (localStorage), “indexed” (indexedDB), “cache” (ETag)                                                          |
| name         | String           | Cookie name prefix, localStorage item name, or indexedDB table                                                                                        |
| purposeList  | Array of Integer | List of ordinal values of entries in the “purposes” array. e.g. \[0,1\] indicates the first and second purpose type is supported by this Storage Type |

PurposeType Object properties

| **Property**   | **Type** | **Description**                                                                   |
| -------------- | -------- | --------------------------------------------------------------------------------- |
| name           | String   | Short identifying label for this purpose                                          |
| description    | String   | A human readable text describing clearly this purpose in the appropriate language |
| maxRetainedFor | Integer  | Number of seconds data is retained after collection                               |
| expiresIn      | Integer  | Number of seconds remaining before collected data is deleted                      |
| consented      | Boolean  | Dynamic indication of registered user agreement for this purpose                  |

*An example of the encoding.*

{

“name”: “BigCo Inc”,

“policy”: “<https://www.bigco.com/privacy.html>”,

“storagePolicy”: “<https://www.bigco.com/cookie.html>”,

“about”: “<https://www.bigco.com/about>”,

“mayCollect”: “true”,

“mayShare”: “true”,

“mayCombine”:”false”,

“purposes”: \[

{

 “name”: “behavioural advertising”,

“description”: “compiling history of web sites visited”,

>  “maxRetainedFor”: “1000000”,
> 
> “expiresIn”: “45667”,

“consented”: “false”

},

{

“name”: “website analytics”,

“description”: “web audience measurement”,

> “maxRetainedFor”: “10000”,
> 
> “expiresIn”: “3456”,

“consented”: “false”

},

{

“name”: “authentication”,

“description”: “logging in”,

> “maxRetainedFor”: “1000000”,
> 
> “expiresIn”: “67854”,

“consented”: “false”

\],

“storage”: \[

{

> “type”: “cookie”,

“name” “\_ga”,

“purposeList”: \[0,1\]

},

{

“type”: “cookie”,

“name” “user”,

“purposeList”: \[2\]

},

> {

“type”: “local”,

“name” “dataname”,

“purposeList”: \[0\]

}

\],

“otherParties”: \[

“[www.google.com](http://www.google.com)”,

“[www.google-analytis.com](http://www.google-analytis.com)”,

“adnxs.com”

\],

“sameParties”: \[

“ourcdn.com”

\]

}
