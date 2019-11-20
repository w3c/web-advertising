# Enabling browsers that belong to the same person to discover one another

For broader context about enabling privacy-preserving, cross-browser conversion attribution, please see the [overview document](https://github.com/w3c/web-advertising/blob/master/cross-browser-anonymous-conversion-reporting.md). This document aims to go into a much greater level of depth about how browsers might discover one another, expanding upon the abstract available in the overview document.

## Discovery Flow

In this sequence of diagrams, we hope to explain how two browsers belonging to the same person might discover one another, validate the authenticity of the other, and establish a secure connection over which they can communicate.

For the sake of concreteness, we will use the following example:

* Ben has 2 devices:
  * An iPhone running the Safari browser
  * A ThinkPad Laptop running the Chrome browser
* Ben uses Twitter and Reddit from both devices.

## Step 0: unconnected state

Before this process begins, both browsers have nothing that identifies them as being related in any way.

![Step 0](https://github.com/w3c/web-advertising/blob/master/Identity%20Provider%20facilitated%20browser%20discovery%20mechanism%20-%20step%200.png)

## Nomenclature:

* We will refer to all **public keys** with capital letters, and all private keys with lower case letters.
* We will refer to all keys (public or private) associated with Ben’s Safari browser using the letter “A” and all keys associated with Ben’s Chrome browser using the letter “B”
* We will use subscript notation to make it clear which scope the key applies to.
* We assume that public keys make it clear which domain they are associated with, so that others know where to send messages. We use @domain syntax at the end of public-keys to express this concept
* Example 1: A<sub>TWTR</sub>@twitter refers to the public key, scoped to the twitter domain, created by Ben’s Safari browser.
* Example 2: a<sub>TWTR</sub> refers to the private key, scoped to the twitter domain, created by Ben’s Safari browser.
* Example 3: B<sub>GLOBAL</sub>@chrome refers to the public key, with global scope, created by Ben’s Chrome browser, and which everyone knows was created by a Chrome browser, and so messages to this entity should be directed to the Chrome hosted E2EE messaging service.

## Step 1: Ben logs into website #1 from browser #1

* Ben logs into twitter.com from mobile Safari on his iPhone. 
* He enters his username and password to prove to the twitter.com website that he is himself.
* The Safari browser generates a new public/private keypair, which is scoped to the eTLD+1 of twitter.com (and as such, cannot be used for cross-site tracking). 
* This public/private keypair will be stored in his browser indefinitely from this point onwards, and will only be removed if Ben clears his cookies.
* Upon login, this newly generated public-key is sent to twitter.com
* Twitter.com stores this public-key and associates it with Ben’s account.

![Step 0](https://github.com/w3c/web-advertising/blob/master/Identity%20Provider%20facilitated%20browser%20discovery%20mechanism%20-%20step%201.png)

## Step 2: Ben logs into website #2 with browser #1

* Ben logs into reddit.com from mobile Safari on his iPhone.
* He enters his username and password to prove to the reddit.com website that he is himself.
* The Safari browser generates another public/private keypair, which is scoped to the eTLD+1 of reddit.com. 
* Upon login, this newly generated public-key is sent to reddit.com
* Reddit.com stores this public-key and associates it with Ben’s account.

![Step 0](https://github.com/w3c/web-advertising/blob/master/Identity%20Provider%20facilitated%20browser%20discovery%20mechanism%20-%20step%202.png)

## Step 3: Ben logs into website #1 with browser #2

* Ben logs into twitter.com from Chrome on his ThinkPad laptop. 
* He enters his username and password to prove to the twitter.com website that he is himself.
* The Chrome browser generates a new public/private keypair, which is scoped to the eTLD+1 of twitter.com
* Upon login, this newly generated public-key is sent to twitter.com
* Twitter.com stores this public-key and associates it with Ben’s account.
* Twitter.com is now aware of 2 public keys that are associated with Ben’s account, belonging to different browsers. 
* After Ben logs in, the Twitter website tells his Chrome browser about all the other public keys it knows about that are associated with his account (in this case, there is only one other and that is A<sub>TWTR</sub>@twitter)
* Ben’s Chrome browser just stores this public key for now, and doesn’t do anything else.

![Step 0](https://github.com/w3c/web-advertising/blob/master/Identity%20Provider%20facilitated%20browser%20discovery%20mechanism%20-%20step%203.png)

## Step 4: Ben logs into website #2 with browser #2

* Ben logs into reddit.com from Chrome on his ThinkPad laptop.
* He enters his username and password to prove to the reddit.com website that he is himself.
* The Chrome browser generates another public/private keypair, which is scoped to the eTLD+1 of reddit.com.
* Upon login, this newly generated public-key is sent to reddit.com
* Reddit.com stores this public-key and associates it with Ben’s account.
* Reddit.com is now aware of 2 public keys that are associated with Ben’s account, belonging to different browsers. 
* After Ben logs in, the Reddit website tells his Chrome browser about all the other public keys it knows about that are associated with his account (in this case, just A<sub>REDDIT</sub>@reddit)
* Ben’s Chrome browser stores this public key

![Step 0](https://github.com/w3c/web-advertising/blob/master/Identity%20Provider%20facilitated%20browser%20discovery%20mechanism%20-%20step%204.png)

## Step 5: Ben’s browser #2 sends a message to website #2

* Ben’s Chrome browser is now signed into 2 websites, and is aware of at least 2 public keys that are ostensibly associated with other browsers belonging to him.
* It’s entirely possible that one of these “Identity Provider” websites is lying to him though, and has sent him the public key of a browser belonging to “Eve the eavesdropper”. 
* Ben really doesn’t want to start syncing ad click / opportunity / blind-signature data with Eve. That would enable her to surveil a lot of his activity all over the internet.
* As such, Ben’s Chrome browser encrypts the message it sends back to reddit.com **_using the public key it learned about from the twitter.com website_**. 
* encrypted_message_payload = AsymmetricEncrypt(public_key: A<sub>TWTR</sub>@twitter, data_to_encrypt: …)
* What data is being sent in this message? It depends on the decision we make about how end-to-end-encrypted messaging is done between the browsers. See the [overview document](https://github.com/w3c/web-advertising/blob/master/cross-browser-anonymous-conversion-reporting.md) for a discussion of the tradeoffs here. But let’s just assume we go with the approach of “browser vendors provide E2EE messaging service”.
* In that world, both of Ben’s browsers have a globally scoped public/private keypair that was initialized at installation time.
* It is this global public key that is sent across as the message body.

![Step 0](https://github.com/w3c/web-advertising/blob/master/Identity%20Provider%20facilitated%20browser%20discovery%20mechanism%20-%20step%205.png)

## Step 6: Ben logs into website #2 with browser #1 a second time

* Ben opens reddit.com on mobile Safari on his iPhone (a second time)
* He may or may not need to enter his username and password, as he might still be logged in. 
* Next, the Reddit website tells his Safari browser about all the other public keys it knows about that are associated with his account (in this case, just B<sub>REDDIT</sub>@reddit)
* Ben’s Safari browser stores this public key in the reddit-scoped contact list
* Safari browser checks to see if there are any new E2EE messages waiting for it
* Reddit sends along the encrypted-message it had previously received from Ben’s Chrome browser.
* The Safari browser iterates over all of the “Identity Provider” scopes for which it has generated public/private keypairs, and attempts to decrypt the message using each private key.
* It can just skip the reddit.com scope, because we know based on how this protocol works that the message was certainly not encrypted with a reddit-scoped key.
* If the message is actually decryptable using any of these keys, hooray!
* Now the Safari browser is in possession of the globally-scoped public key of another browser, belonging to the same person, which has logged into at least 2 websites in common. In our nomenclature this is B<sub>GLOBAL</sub>@chrome
* It adds this public key to its globally scoped “contact list”.
* From this point onwards, Ben’s Safari browser can start sending end-to-end-encrypted messages to his Chrome browser. Each time there is an ad click / opportunity it can send a message to Chrome’s server-side infrastructure, addressed to the public key we are calling B<sub>GLOBAL</sub>@chrome

![Step 0](https://github.com/w3c/web-advertising/blob/master/Identity%20Provider%20facilitated%20browser%20discovery%20mechanism%20-%20step%206.png)

## Step 7: Ben logs into website #1 with browser #1 a second time

* Ben opens twitter.com from mobile Safari on his iPhone a second time.
* He may or may not need to enter his username and password, as he might still be logged in. 
* Next, the Twitter website tells his Safari browser about all the other public keys it knows about that are associated with his account (in this case, just B<sub>TWTR</sub>@twitter)
* Ben’s Safari browser stores this public key in the twitter-scoped contact list
* Just as before, Ben’s Safari browser sends an asymmetrically encrypted message back to the twitter.com servers **_using the public key it learned about from the reddit.com website_**. Ben’s Safari browser’s globally scoped public key is the message body.
* encrypted_message_payload = AsymmetricEncrypt(public_key: B<sub>REDDIT</sub>@reddit, data_to_encrypt: A<sub>GLOBAL</sub>@safari)
* Twitter stores this encrypted message with Ben’s account

![Step 0](https://github.com/w3c/web-advertising/blob/master/Identity%20Provider%20facilitated%20browser%20discovery%20mechanism%20-%20step%207.png)

## Step 8: Ben logs into website #1 with browser #2 a second time

* Ben opens twitter.com on Chrome on his ThinkPad laptop (a second time)
* He may or may not need to enter his username and password, as he might still be logged in. 
* Ben’s Chrome browser checks to see if there are any new E2EE messages waiting for it
* Twitter sends along the encrypted-message it had previously received from Ben’s Safari browser.
* The Chrome browser iterates over all of the “Identity Provider” scopes for which it has generated public/private keypairs, and attempts to decrypt the message using each private key.
* It can just skip the twitter.com scope, because we know based on how this protocol works that the message was certainly not encrypted with a twitter-scoped key.
* If the message is actually decryptable using any of these keys, hooray!
* Now the Chrome browser is in possession of the globally-scoped public key of another browser, belonging to the same person, which has logged into at least 2 websites in common. In our nomenclature this is A<sub>GLOBAL</sub>@safari
* It adds this public key to its globally scoped “contact list”.
* From this point onwards, Ben’s Chrome browser can start sending end-to-end-encrypted messages to his Safari browser. Each time there is an ad click / opportunity it can send a message to Safari’s server-side infrastructure, addressed to the public key we are calling A<sub>GLOBAL</sub>@safari

![Step 0](https://github.com/w3c/web-advertising/blob/master/Identity%20Provider%20facilitated%20browser%20discovery%20mechanism%20-%20step%208.png)

## Summary

This is just one potential flow. In reality, there are infinite combinations of sequences that might transpire. It doesn’t really matter, in the end, after enough time, and enough sequential log-ins across browsers, they will eventually learn about one another and acquire the public keys of the other browsers belonging to the same person.

Here is the pseudo-code for the “Identity Provider” protocol. You’ll see that at each step, we just followed this same logic:

```
// Upon log-in or authenticated site access:

// Browser:
function sendDomainScopedPublicKeyToIdentityProvider(domain) {
    if (!existsDomainScopedKeypair(domain)) {
        generateDomainScopedKeypair(domain);
    }

    return getDomainScopedPublicKey(domain);
}

// Identity Provider:
function processLogin(user_id, public_key) {
    AppendToUserProfile(user_id, public_key);

    all_public_keys = GetAllPublicKeysForUser(user_id);
    encrypted_messages = GetAllEncryptedMessagesForUser(user_id);

    return {
        ‘public_keys’ => all_public_keys,
        ‘encrypted_messages’ => encrypted_messages,
    };
}

// Browser:
function processResponseAndMaybeSendEncryptedMessagesBack(
    domain,
    all_public_keys_known_to_domain,
    encrypted_messages,
) {
    // Update domain scoped “contact list”
    combined_public_key_list = SetRemove(
        SetUnion(
            getDomainScopedContactList(domain),
            all_public_keys_known_to_domain,
        ),
        getDomainScopedPublicKey(domain),
    );
    OverwriteDomainScopedContactList(
        domain,
        combined_public_key_list,
    );

    // Attempt to decrypt messages
    all_private_keys_for_other_domains = map(
        filter(
            domain_scopes, 
            d ⇒ d != domain,
        ),
        d ⇒ getDomainScopedPrivateKey(domain);
    );
    foreach (encrypted_messages as msg) {
        foreach (all_private_keys_for_other_domains as private_key) {
            decrypted_message = AsymmetricDecrypt(
                private_key,
                msg,
            );
            if (DecryptionSuccessful(decrypted_message) {
                AppendToGloballyScopedContactList(decrypted_message);
                break;
            }
        }
    }

    // Maybe send encrypted messages back
    unified_contact_list_from_other_domains = flatten(map(
        filter(
            domain_scopes, 
            d ⇒ d != domain,
        ),
        d ⇒ getDomainScopedContactList(d),
    ));
    if (empty(unified_contact_list_from_other_domains)) {
        return null;
    }
    encrypted_messages_to_send = map(
        unified_contact_list_from_other_domains,
        public_key ⇒ AsymmetricEncrypt(
            public_key, // key to encrypt with
            getGlobalPublicKey(),
        ),
    );
    return encrypted_messages_to_send;
}

// Identity Provider
function storeEncryptedMessages(user_id, encrypted_messages) {
    AppendToEncryptedMessagesForUser(user_id, encrypted_messages);
}
```
