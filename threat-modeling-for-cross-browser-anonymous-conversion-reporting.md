# Threat modeling for Cross-Browser Anonymous Conversion Reporting

This document considers how an attacker might attempt to compromise the privacy of an individual by attacking the cross-browser anonymous conversion reporting scheme as outlined in [this document](https://github.com/w3c/web-advertising/blob/master/cross-browser-anonymous-conversion-reporting.md). It also attempts to lay out suggestions for how one might try to mitigate these concerns.

## Attack #1: compromise two social media accounts by stealing logged-in session cookies

If an attacker is able to steal the logged-in session cookies for two social media accounts (e.g. using malware installed on the person’s computer), they could take a new browser (let’s call it “Attacker Browser”), place these logged-in session cookies on that “Attacker Browser”, and then send a new domain-scoped public key to both websites. If that is successful, after some sequence of log-ins from their real browsers, we might wind up with that person’s other browsers synchronizing ad click / opportunity data with this “Attacker Browser”. 

One mitigation we can put in place, is to have the Identity Providers watch out for browsers that try to send a new public key from the same logged-in session. This should never happen under normal circumstances, so if that happens, it would probably be a good idea to log the person out by invalidating that particular session, rendering that particular cookie useless.

We need to ensure that malware cannot steal any of the public / private keys or local contact lists for any of the domain scopes or global scopes. This is a matter of ensuring that browsers are hardened against such attacks and this type of information is encrypted at rest. For people that are using legitimate operating systems, and actively utilizing anti-virus software concerns should be mitigated. I’m most concerned about people who are using pirated versions of Windows, and those who are not employing any type of anti-virus software.

## Attack #2: compromise two social media accounts via username / password 

If an attacker is able to obtain the username and password for two social media accounts belonging to the same person that would cause a problem. This would enable them to create a new browser (let’s call it “Attacker Browser”), log into both of these accounts, and after a few log-ins from both the real and “Attacker” browsers, ad click / opportunity data would be synced with this “Attacker Browser”.

One mitigation we can put in place, is to have “Identity Provider” websites require 2-fac confirmation when someone attempts to log into their account from a new browser.

I have enabled 2-fac confirmation for my Facebook account, so here is what I see when I log-in to Facebook from a new browser:

![2fac popup shown when a new browser signs into my Facebook account](https://github.com/w3c/web-advertising/blob/master/2fac_popup.jpg)

Notice the mobile notification at the top as well as well as the requirement to input a code. I can validate this browser either way. If I tap that notification, it opens the Facebook app, where I have a new notification. Here is what that notification shows me:

![Step 1 of the flow to allow a new browser to access my Facebook account](https://github.com/w3c/web-advertising/blob/master/review_recent_login1.jpg)

Clicking “continue” shows the following screen:

![Step 2 of the flow to allow a new browser to access my Facebook account](https://github.com/w3c/web-advertising/blob/master/review_recent_login2.jpg)

Clicking on “This was me” shows the following:

![Step 3 of the flow to allow a new browser to access my Facebook account](https://github.com/w3c/web-advertising/blob/master/review_recent_login3.jpg)

Not all people have enabled 2-fac, and it would be a big ask to get Social Media websites to require this for all users. We can opt for a reasonable middle-ground where “Identity Provider” websites alert users when a suspicious log-in happens (e.g. from an IP address from a totally unexpected location). We can also ask “Identity Providers” to all offer 2-fac as an option for their users, and push it with upsells (as Gmail currently does).

## Attack #3: Malicious identity provider and one compromised social media account

If an “Identity Provider” is malicious, they could insert malicious “encrypted messages” into their user-scoped inbox, where the encrypted message payload is the globally scoped public key of an “Attacker Browser”. 

To succeed at this attack, the “Identity Provider” would also need to compromise one of this person’s other social media accounts (one that they do not own and operate). That would be enough to get ad click / opportunity data synced to the “Attacker Browser”. 

The way the attack would work, would be to add a malicious “encrypted message”, encrypted using a public key of a real browser, obtained from compromising another account. To obtain a domain-scoped public key of a legit browser only requires stealing their logged-in session cookies, as the “Identity Provider” shares the list of domain-scoped public-keys upon each authenticated site access. The attacker does not need to add a new public-key to the list. 

One mitigation we can put in place is to essentially make ad click / opportunity syncing “opt-in”, not syncing data to a new browser until the person confirms this browser belongs to them. 

Here’s how this might work: Each time their browser successfully decrypts an end-to-end-encrypted message relayed to them via an “Identity Provider”, and thus comes into possession of the globally scoped public key of another browser that should belong to them, their browser could prompt them to confirm that this is indeed a browser that belongs to them. Until the user confirms that this is indeed one of their browsers, we would not sync any ad click / opportunity data. If the user states that they do not recognize this browser, we could blacklist this globally scoped public key.

We probably want to make trust “transitive” to avoid seeing this same confirmation dialog across multiple browsers. If a person decides to “trust” a browser, it could send out some kind of encrypted message to all the previously trusted browsers, telling them to also trust this new browser. Similarly, if the user specifies that this browser does not belong to them, we could send out an encrypted message to all their other browsers telling them to also blacklist this public key.

Here’s a super-rough mock up of how this flow might look:

![Mockup of what a dialog might look like in a mobile browser, asking someone to confirm that a newly connected browser belongs to them](https://github.com/w3c/web-advertising/blob/master/mockup_of_new_browser_confirmation_dialog.png)

This extra data about the device where the browser is installed, and an image (or phrase) that represents this device could also be sent along with the encrypted message. Ideally the image (or phrase) would be derived directly from the globally scoped public key, so that it’s not spoofable.

Although this mockup uses a QR-code, I think a phrase would be better. Actually, Facebook has already implemented public-key-to-phrase functionality. This is utilized for a very similar type of situation with a similar threat model: Facebook login.

This also implies that for any browser, you can go and see some page somewhere that displays this same visual representation of your globally scoped public key in image form. 

That same page could also display all of the other browsers that are in the globally scoped “contact list”, and allow the user to disconnect any of them at any time.

Of course, on that page we could provide people the ability choose to turn off all cross-browser related functionality if they so desire.

This mitigation would also prevent “click stuffing” because browsers only sync ad click / opportunity data when the sender is in their verified globally-scoped contact list (by checking the signature). 

