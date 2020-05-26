W3C Improving Web Advertising Success Criteria
==============================================

Abstract
--------

Referencing prior W3C values and principles, GDPR, and a limited number of
external documents, this document defines success criteria to evaluate the
interests of societies, people, publishers, marketers, and browser vendors. No
one factor is assumed to be more or less important than another.

The Improve Web Advertising working group has a shared goal of preserving the
web as an open platform for diverse and rich experiences provided by multiple
parties. Towards this end, our goal is to provide monetization opportunities
that support the open web while balancing the needs of publishers and the
advertisers that fund them with improvements to protect end users from the
individual and societal impacts of tracking content consumption over time.

Principles to Improve Web Advertising 
--------------------------------------

The open web has become a critical global utility that supports the free flow of
communication, commerce, content and competition. The open web is increasingly
becoming the primary method used by citizens to access news, communicate with
each other, engage with governments, conduct commerce and consume entertainment.
Any individual, business, charity or government organization regardless of
background, geography or wealth can now cost effectively participate in this
global resource. A set of common technical and policy standards makes all this
possible.

The web is an open platform.
[Interoperability](https://www.w3.org/Promotion/WIP) is a fundamental principle
that supports all web technologies. Existing specifications and protocols for
encoding, transmitting and rendering information facilitate these exchanges.
Data portability also helps ensure that the web is interoperable ([GDPR, Recital
68](https://gdpr.eu/recital-68-right-of-data-portability/)).

The web underpins trillion-dollar industries. Accordingly, any proposed change
to the underpinning policy and technical standards can have global
ramifications. While no one individual is able to understand every ecosystem
dependency to ensure other participants are not unintentionally undermined, a
common set of principles can and should be used to review these proposed
changes.

This document provides a set of improved web advertising principles to ensure
proposed changes appropriately balance the benefits of the change against the
impact on the important rights of individuals and the societies they live in.
When describing how to balance of these important interests in promoting
openness and fairness in the standards they develop, the Internet Engineering
Task Force (IEFT) emphasizes this point:

>   *the IETF is concerned with developing and maintaining the Internet to
>   promote the social good, and the society that the IETF is attempting to
>   enhance is composed of end users, along with groups of them forming
>   businesses, governments, clubs, civil society organizations, and other
>   institutions.* ([Draft - Internet Architecture Board for the End
>   Users](https://intarchboard.github.io/for-the-users/draft-iab-for-the-users.html)).

This does not mean that all stakeholders are unanimously agreed and aligned on
how to "improve" the web. Scholars describe diverging interests of stakeholders
as a "tussle."

>   The resulting tussles span a broad scope: the rights of the individual vs.
>   the state, the profit seeking of competitors, the resistance to those with
>   malicious intent, those with secrets vs. those who would reveal them, and
>   those who seek anonymity vs. those would identify them and hold them
>   accountable. ([Tussle in
>   Cyperspace](http://groups.csail.mit.edu/ana/Publications/PubPDFs/Tussle2002.pdf)).

Achieving a balance across the diverse interests of global stakeholders when
determining tradeoffs among speed, fairness, security, public accountability,
diversity and quality is in accordance with [values of the
W3C](https://www.w3.org/standards/about.html). To resolve this tussle and in the
interests of end users requires us to carefully examine what alternate means are
possible of achieving the desired goals. If a negative impact to stakeholders is
unavoidable, then the reasoning behind this decision ought to be thoroughly
documented. Accordingly, this document describes the principles that support the
key interests of stakeholders that enable people's internet-enabled access to
information.

They are called "principles" (rather than, for example, guidelines or
requirements) because they attempt to capture important concepts and aspirations
that are [not specific to any particular
realization](https://www.w3.org/TR/2003/NOTE-di-princ-20030901). These
principles can be distinguished from bottoms up, granular "use cases," which
illustrate how an actor can conduct a process to achieve a goal. Like [other
working groups](https://www.w3.org/TR/wsa-reqs/#id2605084), the Improve Web
Advertising working group may choose to adopt the Critical Success Factor (CSF)
Analysis method to better communicate our work. These principles are examined
from three [perspectives](https://www.w3.org/Consortium/Points): that of
individuals (both in aggregate and individually), publishers (both authors and
the business model that funds them) and the delivery access mechanism (both
connectivity and navigation).

Any technology can be abused. Modern societies do not attempt to suppress
technology, but rather put [appropriate
regulations](https://iabeurope.eu/all-news/iab-europes-press-statement-openrtb-and-eu-data-protection-law)
in place to define acceptable and unacceptable uses of that technology. For
example, automobiles are not required to integrate functionality that
technically prevents them from exceeding the speed limit. Instead, drivers are
educated and trained in traffic rules, and drivers who violate speed limits are
subject tofines and/or deprived of their permits. However, documenting specific
criteria as to what constitutes a violation helps enable easier detection and
reporting of non-compliance with the regulations that govern technology.

By documenting the defined norms and principles behind appropriate and
inappropriate data collection and processing, we can better devise methods of
accountability. This accountability requires each participant that has access to
data collection and processing to abide by its responsibility not to abuse the
data under its control. This in turn requires definitions of legitimate data
collection and processing as well as transparency around whether the data
controller has fulfilled its obligations.One of the first assumptions we
document is that an advertising-funded business model supports the open web, and
hence changes which degrade the efficacy of this business model negatively
impacts end users. While end users increasingly understand advertising funds
their free access to the open web, they desire improved transparency and control
over their personal data.

### Privacy

Individual privacy is a critical issue that societies around the world must
address. Given the pervasiveness of internet-enabled content and services that
support modern societies, it is important we protect people's privacy while
ensuring we do not undermine important societal goals.

Tracking content consumption over time poses risks to end users. Privacy
regulations have identified numerous harms from these risks, including
manipulation of political elections by foreign parties, discrimination against
protected classes, using content consumption activity ("behaviour") in a manner
that causes a substantive life impacts to people's access to healthcare,
financial resources or infliction of emotional distress, deceptive manipulation,
and fraud. As this first example illustrates, these harms pose risks not just to
the individual but also to the larger society in which people live.

As GDPR concisely states, data protection must be balanced with these other
fundamental rights:

>   *The processing of personal data should be designed to serve mankind. 2 The
>   right to the protection of personal data is not an absolute right; it must
>   be considered in relation to its function in society and be balanced against
>   other fundamental rights, in accordance with the principle of
>   proportionality. 3 This Regulation respects all fundamental rights and
>   observes the freedoms and principles recognized in the Charter as enshrined
>   in the Treaties, in particular the respect for private and family life, home
>   and communications, the protection of personal data, freedom of thought,
>   conscience and religion, freedom of expression and information, freedom to
>   conduct a business, the right to an effective remedy and to a fair trial,
>   and cultural, religious and linguistic diversity.* -- [GDPR, Recital
>   4](https://gdpr.eu/recital-4-data-protection-in-balance-with-other-fundamental-rights/)

In an effort to balance these interests, many privacy regulations describe the privacy risks to people relative to the scale, sensitivity of the data collected and potential of a significant economic or social impact from its inappropriate processing. Accordingly, data minimization, purpose limitation, limited storage, and reliance on pseudonymous identifiers are often recommended to minimize these risks. (GDPR, Art. 5, 25; [Recital 78](https://gdpr.eu/recital-78-appropriate-technical-and-organisational-measures/))

Moreover, privacy regulations and national laws often provide specific and appropriate remedies for violations of their codes of conduct. Ensuring improved accountability is a chief principle of improving web advertising.

### Trust and Interoperability Among Decentralized Parties 

[W3C mission](https://www.w3.org/Consortium/Points) is to provide "technologies
(specifications, guidelines, software, and tools) that will create a forum for
information, commerce, inspiration, independent thought, and collective
understanding." Trust and interoperability are two of the core goalsin support
of W3C's mission.

Trust requires a system that supports "confidentiality, instils confidence, and
makes it possible for people to take responsibility for (or be accountable for)
what they publish on the Web." By improving both transparency and accountability
we can ensure market actors earn trust while enhancing efficiency, efficacy and
fairness in the matching of advertising content to people.

Much of the web consists of embedded content and centralized services provided
to decentralized publishers (e.g., single sign on, web payments, advertising).
People trust many of the brands they purchase or interact with. Behind most of
these brands are numerous supply chain partners that enable people to benefit
from each brand's end product or service. Digital publishers are no different.
Independent publishers must rely on networks of direct partners and indirect
partners of the marketers that fund their operations. This interoperability is a
goal in support of W3C's mission and the first principle in support of improved
web advertising.The trust in this interoperable network requires transparency
and improved documentation of acceptable and unacceptable uses of data. [Some
organizations](https://assets.publishing.service.gov.uk/media/5d78ba3540f0b61c7a66407c/190802_Google_-_Response_to_SoS__Non-confidential_.pdf)
have pointed out that transparency is advanced by vertical-integration. "Opacity
sometimes is a function of fragmentation.... Vertical integration can sometimes
resolve some of the concerns around a lack of transparency and complexity....
Vertical integration means there is a single point of contact for advertisers
and publishers and it eliminates concerns about the possibility of rent-seeking
by difficult-to-identify themselves intermediaries.... vertical integration in
the ad tech state creates efficiencies for users. Changes ought to benefit all
stakeholders, not just one set."

The final sentence emphasizes decentralization, which is a third goalin support
of [W3C's mission](https://www.w3.org/Consortium/Points). "Decentralization is a
principle of modern distributed systems, including societies." Among the
rationales supporting decentralization are choice, competition, and the freedom
of information. The common element among these rationales is the accessibility
to a wide array of diverse publishers. As U.S. Supreme Court Justice Brandeis
[wrote](http://www.columbia.edu/itc/journalism/j6075/edit/readings/brandeis_concurring1.html):
"Among free men, the deterrents ordinarily to be applied to prevent crime are
education and punishment for violations of the law, not abridgment of the rights
of free speech and assembly." Thus to exercise this freedom, people should have
digital access to publishers, which equates to both the right to assembly and
freedom of speech. Safeguarding and improving this accessibility and choice are
the third and fourth principles of improvedweb advertising.

The Internet Architecture Board (IAB) also expressed concerns as to growing
consolidation of power on the Internet.

>   *While the IAB, the Internet Society, and others are examining this
>   phenomenon to understand it better, it is nevertheless prudent to consider
>   whether proposals for changes to how the Internet works favors or counters
>   consolidation. Favoring entities with existing advantages - like resources,
>   size, or market share - is not necessarily a factor that disqualifies a new
>   proposal, but it needs to be considered as a cost of enabling that
>   technology.* ([Draft - IAB ESCAPE
>   Report](https://tools.ietf.org/html/draft-iab-escape-report-00)).

The 2019/20 UK [Competition and Market Authority (CMA) studied online platforms
supported by digital
advertising](https://www.gov.uk/cma-cases/online-platforms-and-digital-advertising-market-study).
The [CMA set out five
goals](https://assets.publishing.service.gov.uk/government/uploads/system/uploads/attachment_data/file/814709/cma_digital_strategy_2019.pdf)
for supporting a trusted, decentralized, interoperable open web, whose operation
relies on web advertising:

1.  promoting "competition for the benefit of consumers";

2.  ensuring "the enormous innovation and benefits brought about through
    digitisation can continue";

3.  creating a "level playing field" for all businesses to compete on the
    merits;

4.  ensuring new competitors can enter digital markets; and

5.  enabling people "to feel trust in online markets.

The above principles for an interoperable web seek to help advance the mission
of the W3C as well as further the objectives outlined by the UK CMA "to promote
competition for the benefit of consumers, both within and outside the UK, to
make markets work well for consumers, businesses, and the economy."

### Principles for Improving Interoperable Web Advertising

The key stakeholders involved in web advertising include individual and groups
of people, publishers, marketers and delivery access providers. We believe the
majority of these interests are compatible with each other in ensuring people
have their rights protected and receive better products, which is advanced
through free market competition. The following sections outline the key
interests and principal goals for each stakeholder group.

#### Interests of Society

-   Diversity of publishers that protect

    -   Freedom of expression to represent minority voices

        -   Free elections protected against foreign manipulation

        -   While free speech requires allowing speech not approved of by the
            majority, we can label political speech by nationality of author and
            whether it is endorsed by one or more candidates Freedom of the
            press to enable watch-dog reporting on important issues and combat
            fake news

    -   Freedom of information to provide fast, easy access to internet-enabled
        content for all

        -   Cost-free access to enable all to access, regardless of economic
            means

        -   Freedom from self-censorship due to content consumption being
            associated with directly-identifiable, offline identity

-   Free-market economies rely on competition, and competition benefits from
    lower barriers to entry for industry newcomers

    -   Competition benefits from

        -   low barriers of entry for people to start new businesses and compete
            against existing incumbents

        -   market actors having choices over which organizations they can work
            with

        -   interoperable standards of communication and ease of data exchanges
            among market actors

    -   Transparent pricing and fees to ensure markets are operating fairly

-   Appropriate remedies for members of society harmed by other entities

    -   Fines

    -   Antitrust intervention

#### Interests of Individual People

-   Same interests as society-level plus

    -   Fast, frictionless experience to access a wide array of internet-enabled
        content and services that makes the Web so valuable

    -   Secure access to access a wide array of internet-enabled content and
        services that makes the Web so valuable

-   Appropriate risk mitigation and remedies

    -   Increased transparency on data collection and processing purposes

        -   Easy access to understand descriptions of data collection and
            processing purposes

        -   Right to data portability

    -   Increased control over any stable ID to which content consumption
        activity is associated

    -   Increased control over legitimate data processing purposes

        -   Consent for

            -   Use of interest-based advertising

            -   Use of precise geolocation data

            -   Use of sensitive health and financial data or information
                related to protected classes

            -   Association of a pseudonymous digital ID with
                directly-identifiable data

            -   Content consumption, communication or commercial activity tied
                to offline identity

            -   Access to adult content by appropriate guardian to prevent
                unauthorized viewing by underage family members

    -   Remedy for the inappropriate use of personal data

        -   Right to be forgotten that benefits from

            -   Ability to reset a pseudonymous digital ID

            -   Dissociation of previously associated devices

            -   Dissociation of previously collected data with a pseudonymous
                digital ID

            -   Correction/deletion of directly-identifiable data

        -   Right to object to data processing

        -   Right to be informed of high-risk data breaches

        -   Right to appropriate remedies for harm (e.g., compensation)

#### Interests of Marketers

-   The publisher ad-funded business model is supported by addressing marketers
    needs and wants

    -   [Impacting these marketer interests, reduces the revenue publishers can
        earn](https://services.google.com/fh/files/misc/disabling_third-party_cookies_publisher_revenue.pdf)

    -   Reducing publisher revenues, impacts the interests of society

-   Marketers who invest in cross-publisher advertising need scaled,
    interoperable measurement and control.

    -   Real-time feedback to improve content matching and budget reallocation
        to better engage with prospects and customers

    -   Fraud and robot detection

    -   Independent verification of delivery and measurement

    -   Attribution of subsequent first-party engagement to prior third-party
        exposure

    -   Aggregate content consumption trends

-   Appropriate risk mitigation and remedies.

    -   Remedy for being charged for inaccurate delivery of content to the
        "right" individuals or the inaccurate measurement of total exposures or
        interactions

#### Interests of Publishers

-   Ad-funded business model to provide free access to all

    -   Same interests as marketers that maximize the value of advertising
        inventory

    -   Same interests as marketers to attract new people to the publisher's own
        property

-   Freedom to provide internet-enabled content/services with the support of an
    open marketplace of vendors.

-   Appropriate risk mitigation and remedies

    -   Remedy for publisher brand being misappropriated ("repurposing")

#### Interests of Delivery Access Mechanisms (Browsers)

-   Same interests desired by society and individuals plus

    -   Ability to facilitate publisher and marketer engagement with end users

        -   Interoperable standards to support the ease of navigation across
            publishers' internet-enabled content and services

    -   Ability to differentiate on

        -   speed of rendering content

        -   ease of navigating the open web

        -   customizability for each person

        -   efficiency in CPU (and hence power and battery consumption)

-   Browsers do not want to differentiate on

    -   Which internet-enabled content and services (publishers) they are
        compatible with or break

    -   Security (the internet should be equally secure across all browsers)

        -   Protection against malware

Principles to Improve Web Advertising

IWAG01 Interoperable

Improved Web Advertising should facilitate data exchange among multiple,
separate services.

-   IWAR01.1 should consist of precisely defined standards

-   IWAR01.2 must enable data portability across services

-   IWAR01.3 should support secure point-to-point communication

-   IWAR01.4 should support ease of navigation

-   IWAR01.5 should provide consistent experiences

IWAG02 Accountable

Improved Web Advertising participants must be responsible for their actions so
as to encourage trustworthy data collection and processing.

-   IWAR02.1 data exchanges and processing must be auditable

-   IWAR02.2 violations for inappropriate actions should be enforceable

-   IWAR02.3 must supply appropriate remedies for harm

-   IWAR02.4 must support freedom of the press to investigate and report on
    wrongdoing

-   IWAR02.5 must provide individual right to be forgotten

-   IWAR02.6 must provide individual right to correction or deletion

IWAG03 Accessible

Improved Web Advertising should not impose high costs to send or receive
internet-enabled data.

-   IWAR03.1 should provide frictionless access to a wide diversity of
    publishers

-   IWAR03.2 must be accessible regardless of economic means

-   IWAR03.3 should support freedom of expression

-   IWAR03.4 must be keep users free from self-censorship

IWAG04 Choice

Improved Web Advertising should not impose high barriers to entry for new market
entrants.

-   IWAR04.1 must support decentralization or sufficient options for centralized
    alternatives

-   IWAR04.2 should support open market competition

-   IWAR04.3 should support freedom of information

-   IWAR04.4 should supporting diversity, especially minority opinions
