Self-Review Questionnaire: Interoperability, Choice, Accessibility and Accountability
=====================================================================================

W3C Unofficial Draft, 16th July 2020

Master Document:
https://51degrees.sharepoint.com/:w:/g/ESOZJuu55vNFqR1hvN4NIIUBgLdURwkgt6sAigaRiJxasw?e=PW31i8

Editors:

James Rosewell (james@51degrees.com) 

Joshua Koran (jkoran@zetaglobal.com) 

Contributors:

Alan Chapell (achapell@chapellassociates.com) 

Arnaud Blanchard (a.blanchard@criteo.com) 

Anthony Rouillot (anthony@adcash.com)

Brad Rodriguez (brodriguez@rubiconproject.com) 

Daniel Sepulveda (dsepulveda@mediamath.com) 

David St. Pierre (dgstpierre2@gmail.com)

Hardeep Bindra (hbindra@gmail.com) 

Ian Meyers (imeyers@liveramp.com) 

Jochen Schlosser (Jochen.Schlosser@adform.com) 

John Sabella (john.sabella@pubmatic.com) 

Kristoffer Nelson (<knelson@srax.com>)  

Paul Bannister (paul@cafemedia.com) 

Paul Chachko (pchachko@throtle.io) 

Rotem Dar (r.dar@eyeo.com)

Scott Menzer (scott@id5.io) 

Taha Dharamsi (tdharamsi@postmedia.com)

Tom Kershaw (tkershaw@rubiconproject.com) 

Wilfried Schobeiri (wschobeiri@mediamath.com)

David St. Pierre (dgstpierre2@gmail.com)

Anthony Rouillot (anthony@adcash.com)

Abstract
--------

The purpose of this document is to help those designing new internet
technologies to consider the important individual, social and business impacts
of new features or specifications as well as suggest common mitigation
strategies of those impacts as we improve web advertising. Thus, this document
does not lay down dogma about how any specific internet technology should be
designed.

The self-review questions below are meant to be useful when considering the
impacts of a new feature or specification on key stakeholders. The mitigation
strategies are meant to assist in the design of the feature or specification.

This document is not meant as a "requirements checklist", nor does an editor or
group’s use of this document obviate the editor or group’s responsibility to
obtain "wide review" of their specification before implementation. Furthermore,
the principles below should not be understood as exhaustive, although addressing
these during the review process should ensure new technologies improve web
advertising.

The W3C has a shared goal of preserving the web as an open platform for diverse
and rich experiences provided by multiple parties. Towards this end, our goal is
to provide monetization opportunities that support the open web while balancing
the needs of publishers and the advertisers that fund them with improvements to
protect people from the individual and societal impacts of tracking content
consumption over time. 

The recommendations in this document refer to the data collection and processing
required for continued ad-funding of web content and services, and not to the
processes by which it is created, nor to the devices or user agents to which it
is delivered. 

Introduction
------------

New features make the web a stronger and livelier platform. Throughout the
feature development process there are both foreseeable and unexpected impacts to
multiple stakeholders. These risks may arise from the nature of the feature,
some of its part(s), or unforeseen interactions with other features. Such risks
and impacts may be mitigated through careful design and application of the
principles and design patterns described below.

Standardizing web features presents unique challenges. Descriptions, protocols
and algorithms need to be considered strictly before they are broadly adopted by
vendors with large user bases. If features are found to have undesirable impacts
on important stakeholder interests after they are standardized, then, it is
better to transparently list these ahead of browser vendors implementations, to
give opportunity for broader feedback.

Stakeholder Impact Modelling
----------------------------

To consider impacts on stakeholders it is convenient to think in terms of their
interests, as a way to illuminate the possible risks to people as they interact
with ad-funded publishers. While [existing documents
highlight](https://intarchboard.github.io/for-the-users/draft-iab-for-the-users.html)
the risks to individuals this document adds a wider social context of groups of
individuals that form communities, businesses, governments, and other
organizations. [INTERNET-FOR-END-USERS].

Thus this document provides questions to review the concrete social and
competition concerns that should be considered when developing a feature for the
web platform in balance with the important security and privacy interests of
individuals. The various advertising stakeholders and their interests are
documented in the [W3C Improving Web Advertising Business Group - Success
Criteria](https://github.com/w3c/web-advertising/blob/master/success-criteria.md)
[IWA-SUCCESS-CRITERIA]. The use cases that currently support these stakeholders
are described in the [Advertising Use
Cases](https://github.com/w3c/web-advertising/blob/master/support_for_advertising_use_cases.md)
[USE-CASES] document. These individual and organizational interests can be
encapsulated into four key principles:

-   Interoperability: Ensuring interoperable standards of communication and ease
    of data exchanges among individuals and market actors.

-   Choice: Ensuring market actors have choices regarding which organizations
    they can work with, which requires a level playing field for all
    organizations to compete on their merits.

-   Accessibility: Ensuring cost-free access for all, regardless of economic
    means.

-   Accountability: Ensuring people feel trust with the information and digital
    services they engage with to access it, while ensuring market actors trust
    the markets are operating fairly.

In the mitigations section, this document outlines a number of techniques that
can be applied to mitigate impacts to these important stakeholder interests.

Documenting the various concerns and impacts in web principles sections of a
document is a good way to help implementers and web developers understand the
risks and impacts that a feature presents, and to ensure that adequate
mitigations are in place. Simply adding a section to your specification with
yes/no responses to the questions in this document is insufficient.

If it seems like a feature does not have an impact on the improve web
advertising principles, then say so inline in the spec section for that feature:

-   There are no known social or competitive impacts of this feature. Saying so
    explicitly in the specification serves several purposes:

    -   Shows that a spec author/editor has explicitly considered these impacts
        when designing a feature.

    -   Provides some sense of confidence that there might be no such impacts.

-   Challenges social and competitive minded individuals to think of and find
    even the potential for such impacts.

-   Demonstrates the spec author/editor’s receptivity to feedback about such
    impacts.

-   Demonstrates a desire that the specification should not be introducing
    social or competitive impacts.

Questions to Consider
=====================

### What web principles might this feature impact, and are those impacts necessary? 

The following section outlines the key principles to balance the benefits and
impacts to stakeholders as we seek to improve web advertising.

In answering this question, it often helps to ensure that the use cases your
feature and specification enable are made clear in the specification itself to
ensure that the reader understands the feature-privacy trade-offs being made.
Are the benefits outweighed by the potential risks? If so, how?

### Interoperability: Does this specification impact interoperability of the web? 

Improved Web Advertising should facilitate data exchange between people and
among multiple, separate internet services required to support ad-funded
publishers. Personal data is often used as an input to provide these internet
services.

The end service people enjoy are dependent on data interoperability. By
definition, multiple organizations need to work together when operating a robust
supply chain.

Given individual’s security and privacy concerns it is necessary to balance
those important interests with the interoperability requirements of the
organizations people interact with. Data minimization and pseudonymization are
important techniques to minimize these risks. Below are some of the questions to
help identify how a specification may impact any of these interests.

-   IWAR01.1 Does the specification impact a form of identity and data transfer
    required for data interoperability?

-   IWAR01.2 Does the specification facilitate portability of personal data
    across services and service providers?

-   IWAR01.3 Does the specification support secure point-to-point communication?

-   IWAR01.4 Does the specification introduce any cross-publisher navigational
    issues?

-   IWAR01.5 Does the specification support providing consistent experiences
    across publishers?

-   IWAR01.6 Does the specification impact people’s choice over how data is
    shared with whom?

### **Choice: Does this specification impact competition?** 

Improved Web Advertising should not impose high barriers to entry for new market
entrants. Ensuring a level playing field requires that specifications not biased
in favour of vertically-integrated organizations over smaller publishers.
Empowering smaller publishers to benefit from the supply chain service providers
mitigates against the risk of unfair competition and provides people greater
diversity of publisher content to choose from. Ensuring marketers with equal
access to budget control and measurement (e.g., frequency capping) across
publishers and on a single publisher further mitigates against the risk of
unfair competition.

Below are some questions to help identify if the specification may impact
publisher or marketer choices on who they can work with.

-   IWAR02.1 Does the specification support decentralization or offer sufficient
    options for centralized alternatives?

-   IWAR02.2 Does the specification support open market competition?

### Accessibility: Does this specification impact people’s access? 

Improved Web Advertising should not impose high costs to send or receive
internet-enabled data. The primary cost many people are concerned with involves
conditioning access upon a direct financial payment, which can discriminate
against the economically disadvantaged. This risk can be mitigated by enabling
publishers to provide access to their internet content and services in exchange
for the information required to fund their website via advertising.

A benefit of reducing financial barriers to access is that a wider diversity of
publishers can enable broader expression of viewpoints. Below are some questions
to help identify if the specification may reduce the diversity of internet
content and services people can access.

-   IWAR03.1 Does the specification support frictionless access to a wide
    diversity of publishers?

-   IWAR03.2 Does the specification support people’s access to internet
    services, regardless of economic means?

-   IWAR03.3 Does the specification impact freedom of expression, diversity
    including access to minority opinions, and freedom of information?

-   IWAR03.4 Does the specification support keeping people free from
    self-censorship?

### Accountability: Does this specification impact security or privacy harms? 

Improved Web Advertising participants must be responsible for their actions to
encourage trustworthy data collection and processing. Below are some questions
to help identify if the specification provides additional transparency or
accountability around the collection and processing of personal data.

-   IWAR04.1 Does the specification support the auditability of data exchanges
    and processing?

-   IWAR04.2 Does the specification support enforceability for violations
    related to inappropriate actions?

-   IWAR04.3 Does the specification support supplying appropriate remedies for
    harm?

-   IWAR04.4 Does the specification impact freedom of the press to investigate
    and report on wrongdoing?

-   IWAR04.5 Does the specification provide people the right to be forgotten?

-   IWAR04.6 Does the specification provide people the right to correction or
    deletion?

-   IWAR04.7 Does the specification facilitate an organization’s compliance with
    privacy regulations?

### What should this questionnaire have asked?

This questionnaire is not exhaustive. After completing an impact review, it may
be that there are additional aspects of your specification that a strict
reading, and response to, this questionnaire, would not have revealed. If this
is the case, please convey those concerns, and indicate if you can think of
improved or new questions that would have covered this aspect.

Mitigation Strategies
---------------------

To mitigate the impact to stakeholders by the changes proposed in your
specification, you may want to review how to apply one or more of the
mitigations described below to your feature.

### Interoperability 

The W3C promotes interoperability by designing and promoting open
(non-proprietary) computer languages and protocols. Data interoperability
provides more opportunity for new services to innovate solutions with the same
data. More specifically, data interoperability fosters innovation by reducing
access costs by standardizing the protocols and formats used to exchange
information. These standards help provide transparency, which is important for
creating the accountability that builds trust.

For example, standardized ad creative formats streamline digital advertising
marketplaces by reducing creative production costs for marketers thus increasing
the number of available publishers that can deliver their message. These same
standards reduce publisher operations costs by providing access to increased
marketer demand relative to customized ad formats.

Because informed choice requires transparency, organizations should communicate
advertising-related metadata with precisely defined standards that enable people
to understand which brand sent the ads they receive. This metadata enables all
the touchpoints with the people’s internet client to transparently display this
information as well as more easily communicate appropriate signals of consent
between people and organizations. [IWAR01.1]

Interoperable standards of consent and preference choices facilitate people’s
communication with organizations related to this collection and processing of
their personal data. People must have choice over how their data is shared and
with whom. [IWAR01.6 ] The standardized description and display of data
collection and processing purposes further aids the transparent, bi-directional
communication between people and organizations. [IWAR01.2]

Securing point-to-point transmission of both organizations’ advertising content
and people’s choice-signals reduces the risk that an active attacker may alter
these messages. [IWAR01.3 ]

Standardization of privacy and security signals also supports the ease of
navigation for individuals across different publisher properties and services.
[IWAR01.4] To further ease cross-publisher navigation, privacy preferences may
be centralized. People’s consent and preference signals should persist across
all individual and organization interactions. [IWAR01.5 ]

For example, the [W3C’s Proposed Recommendation
Process](https://www.w3.org/2005/10/Process-20051014/tr.html) [TEC-DEV] defines
several requirements for specification entrance criteria for any Proposed
Recommendations, among which is Interoperability. Interoperability is one of the
[seven core principles](https://www.w3.org/Consortium/Points/) [SEVEN-POINTS]
behind many of the existing web standards:

### Choice

Societies today are plagued by fake news and attempts at foreign manipulation of
democratic elections. Providing people greater choice over access to news and
information, mitigates these risks by increasing the cost for malicious
attackers to dominate the information people can access. A free press can
provide appropriate counterweight to fake news, while watch-dog reporting on
important issues helps uncover the abuse by bad actors.

To mitigate the above risks, organizations must support either decentralization
or sufficient options for centralized alternatives. Decentralization is
furthered by enabling publishers to offer mixed content from different sources.
By reducing the cost of market entry, more publishers can offer their views on
important social topics. Making internet content easily shareable also supports
freedom of information.

Moreover, organizations should support open market competition that requires a
level playing field for all organizations to compete on their merits. Moreover,
the choices of publishers and marketers over their supply chain partners
requires this competition. [IWAR02.2 ]

To further foster competition and innovation, organizations must ensure people
can start new organizations to enter the market and compete against existing
incumbents.

For example; the [W3C’s Antitrust and Competition
Guidance](https://www.w3.org/Consortium/Legal/2017/antitrust-guidance)
[ANTITRUST] requires the W3C not to restrict competition and by implication
choice.

### Accessibility 

[W3C promotes “One Web”](https://www.w3.org/Consortium/mission) that is
available on any device. [W3C-MISSION] The web is designed to be [accessible for
all people regardless of economic means](https://contractfortheweb.org/).
[W3C-MISSION; CONTRACT-FOR-THE-WEB; IWAR03.2 ] Accordingly, organizations should
provide people frictionless access to a wide diversity of publishers. [IWAR03.1]

The web is not a “read-only” network but encourages people’s collaborative
interaction with organizations and one another. Accordingly, organizations
should support freedom of expression for people to interact with the
internet-enabled content and services they engage with. An additional benefit of
freedom of expression is that it promotes diversity by enabling minority
opinions and views to be better represented by different organizations.[IWAR03.3
]

Given privacy-preserving requirements that people’s offline, directly
identifiable identity not be disclosed by default when people interact with
digital content and services, individuals may make different privacy choices on
each device or application or instance of an application. For example, incognito
or private browsing activities must not persist beyond a temporary session
generated during this selected mode. Moreover, the browsing activities generated
prior to enabling the private mode must not be made accessible while this mode
is enabled. Preserving private mode access to information and communication,
helps mitigate the risks to society that can result from repressing minority
views. Accordingly, organizations must keep people free from self-censorship
that could result from re-identification of people’s content consumption without
appropriate notice and choice. [IWAR03.4]

### **Accountability** 

Accountability for appropriate data collection and processing requires both
transparency and auditability. Transparency requires that organizations must
provide individuals access rights to their personal data as well as how their
personal data is used to match advertising content and measure its
effectiveness. [IWAR04.1]

To reduce the risk of harm, organizations must provide people the right to
correction or deletion of their personal data as well as to provide people the
right to be forgotten. [IWAR04.5 ] and [IWAR04.6]

Risk mitigation does not eliminate the possibility of harm. Thus, accountability
further means that violations for inappropriate action must supply people with
appropriate remedies for harm. [IWAR04.3] To aid in this effort, organizations
must keep appropriate policy-aware records or audit logs that can identify
non-compliance with regulations. [IWAR04.2] While different jurisdictions have
different laws and regulations, organization’s audit records must aid their
compliance with privacy regulations. [IWAR04.7]

Independent oversight regarding the collection and processing of personal data
is an additional safeguard to ensure organizations are abiding by their duties.
While sometimes this oversight is provided by trade organizations or public
agencies, the free press also helps provide necessary safeguards. Organizations
must support freedom of the press to investigate and report on wrongdoing to
hold all organizations accountable to the same standards. [IWAR04.4]

The W3C has created a separate [Security and Privacy
Questionnaire](https://www.w3.org/TR/2020/NOTE-security-privacy-questionnaire-20200508/)
[[SECURITYPRIVACY]](https://www.w3.org/TR/2020/NOTE-security-privacy-questionnaire-20200508/)
to further help developers consider these important individual issues.

Conformance
-----------

Conformance requirements are expressed with a combination of descriptive
assertions and RFC 2119 terminology. The key words “MUST”, “MUST NOT”,
“REQUIRED”, “SHALL”, “SHALL NOT”, “SHOULD”, “SHOULD NOT”, “RECOMMENDED”, “MAY”,
and “OPTIONAL” in the normative parts of this document are to be interpreted as
described in RFC 2119. However, for readability, these words do not appear in
all uppercase letters in this specification.

All of the text of this specification is normative except sections explicitly
marked as non-normative, examples, and notes. [RFC2119]

Examples in this specification are introduced with the words “for example” or
are set apart from the normative text with class="example", like this:

This is an example of an informative example.

Informative notes begin with the word “Note” and are set apart from the
normative text with class="note", like this:

Note, this is an informative note.

Index
-----

Terms defined by reference

[WCA] defines the following terms:

>   Client

>   Cookie

>   Gateway

>   Message

>   Publisher

>   Server

>   User

>   User Session

>   Web Page

[BIZ-DIC] defines the following terms:

Marketer

References
----------

### Normative References

**[ANTITRUST]**

Wendy Seltzer. [Antitrust and Competition
Guidance](https://www.w3.org/Consortium/Legal/2017/antitrust-guidance)
01-March-2017. URL:
<https://www.w3.org/Consortium/Legal/2017/antitrust-guidance>

**[RFC2119]**

S. Bradner. . March 1997. Best Current Practice.
URL: <https://tools.ietf.org/html/rfc2119>

**[SECURITYPRIVACY]**

Jason Novak; Lukasz Olejnik; Mike West. Working Group Note, 8 May 2020. URL:
<https://www.w3.org/TR/2020/NOTE-security-privacy-questionnaire-20200508/>

**[WCA]**

Brian Lavoie; Henrik Frystyk Nielsen. [Web Characterization Terminology &
Definitions Sheet](https://www.w3.org/1999/05/WCA-terms/). 24-May-1999. URL:
<https://www.w3.org/1999/05/WCA-terms>

### Informative References

**[BIZ-DIC]**

Business Dictionary. URL:

**[CONTRACT-FOR-THE-WEB]**

Sir Tim Berners-Lee. [Contract for the Web](https://contractfortheweb.org). 25
November 2019. URL: <https://contractfortheweb.org>

Principle 1: Ensure everyone can connect to the internet

Principle 2: Keep all of the internet available, all of the time

Principle 3: Respect and protect people’s fundamental online privacy and data
rights

Principle 4: Make the internet affordable and accessible to everyone

Principle 5: Respect and protect people’s privacy and personal data to build
online trust

Principle 6: Develop technologies that support the best in humanity and
challenge the worst

Principle 7: Be creators and collaborators on the Web

Principle 8: Build strong communities that respect civil discourse and human
dignity

Principle 9: Fight for the Web

**[INTERNET-FOR-END-USERS]**

M. Nottingham.[The Internet is for
End](https://intarchboard.github.io/for-the-users/draft-iab-for-the-users.html).
10 March 2020. Users URL:
<https://intarchboard.github.io/for-the-users/draft-iab-for-the-users.html>

**[SEVEN-POINTS]**

W3C. Seven point summary of W3C. 15th December 2005. URL:
<https://www.w3.org/Consortium/Points/>

**[IWA-SUCCESS-CRITERIA]**

J. Rosewell. [W3C Improving Web Advertising Business Group - Success
Criteria](https://github.com/w3c/web-advertising/blob/master/success-criteria.md).
June 2020. URL:
<https://github.com/w3c/web-advertising/blob/master/success-criteria.md>

**[TEC-DEV]**

Ian Jacobs. World Wide Web Consortium Process Document chapter W3C Technical
Report Development Process. 14th October 2005. URL:
<https://www.w3.org/2005/10/Process-20051014/tr.html>

**[USE-CASES]**

B. Savage. [Advertising Use Cases](Advertising%20Use%20Cases). June 2020. URL:
<https://github.com/w3c/web-advertising/blob/master/support_for_advertising_use_cases.md>

**[W3C-MISSION]**

W3C Mission. 30 June 2004. URL: <https://www.w3.org/Consortium/mission>

Original scorecard below with suggested edits for anything we keep:

Appendix
========

| **Criteria reference** | **Primary stakeholder interest** | **Interest category (Functionality / Privacy / Interoperability /etc)** | **Interest priority (Must / Should)** | **Interest description**                                                                            |
|------------------------|----------------------------------|-------------------------------------------------------------------------|---------------------------------------|-----------------------------------------------------------------------------------------------------|
| IWAR01.1               | Organizations                    | Interoperability                                                        | Should                                | Organizations should communicate advertising-related data with precisely defined standards          |
| IWAR01.2               | Individual                       | Interoperability                                                        | Must                                  | Organizations must enable data portability of personal information across services                  |
| IWAR01.3               | Individual                       | Interoperability                                                        | Should                                | Organizations should support secure point-to-point communication                                    |
| IWAR01.4               | Individual                       | Interoperability                                                        | Should                                | Organizations should support ease of navigation for individuals                                     |
| IWAR01.5               | Individual                       | Interoperability                                                        | Should                                | An individual’s choices should persist across all individual\<\>organization interactions           |
| IWAR01.6               | Individual                       | Interoperability                                                        | Must                                  | An individual must have choice over how their data is shared and with whom.                         |
| IWAR02.1               | Organizations                    | Choice                                                                  | Must                                  | Organizations must support decentralization or sufficient options for centralized alternatives      |
| IWAR02.2               | Organizations                    | Choice                                                                  | Should                                | Organizations should support open market competition                                                |
| IWAR03.1               | Individual                       | Accessibility                                                           | Should                                | Organizations should provide people frictionless access to a wide diversity of publishers           |
| IWAR03.2               | Individual                       | Accessibility                                                           | Must                                  | Web must be accessible for all people regardless of economic means                                  |
| IWAR03.3               | Individual                       | Accessibility                                                           | Should                                | Organizations should support freedom of expression for people                                       |
| IWAR03.4               | Individual                       | Accessibility                                                           | Must                                  | Organizations must keep people free from self-censorship                                            |
| IWAR04.1               | Individual                       | Accountability                                                          | Must                                  | Organizations must provide Individual data access rights and processing use-cases must be auditable |
| IWAR04.2               | Individual                       | Accountability                                                          | Should                                | Violations for inappropriate actions should be enforceable                                          |
| IWAR04.3               | Individual                       | Accountability                                                          | Must                                  | Organizations must supply people appropriate remedies for harm                                      |
| IWAR04.4               | Organizations                    | Accountability                                                          | Must                                  | Organizations must support freedom of the press to investigate and report on wrongdoing             |
| IWAR04.5               | Individual                       | Accountability                                                          | Must                                  | Organizations must provide people the right to be forgotten                                         |
| IWAR04.6               | Individual                       | Accountability                                                          | Must                                  | Organizations must provide people the right to correction or deletion                               |
| IWAR04.7               | Organizations                    | Accountability                                                          | Must                                  | Must aid organizations’ compliance with privacy regulations                                         |
