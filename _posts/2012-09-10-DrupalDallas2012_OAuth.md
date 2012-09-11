---
layout: post
title: "OAuth: the spec, the dance, and Drupal"
date: 2012-09-10 20:00
comments: false
categories:
- drupal
- presentations
---

## [Presentation](http://www.youtube.com/watch?v=RUylSUkz8o4&feature=player_embedded) from the [2012 Dallas Drupal Days](http://dallasdrupal.org/sessions/oauth-spec-dance-and-drupal)

<iframe width="618" height="463" src="http://www.youtube.com/embed/RUylSUkz8o4" frameborder="0" allowfullscreen></iframe>

## [Slides](http://imetchrischris.com/dallasdrupal2012/)

<script async class="speakerdeck-embed" data-id="504fc37c256d9200020295fb" data-ratio="1.6" src="//speakerdeck.com/assets/embed.js"></script>

(Note: the post as follows is identical to the slides)

----

## The Web: HTTP/1.0

*  Client <-- --> Server
*  GET, PUT, POST, DELETE, HEAD, ...
*  Headers

----

## Auth experience

*  [HTTP Basic Auth](http://en.wikipedia.org/wiki/Basic_access_authentication) - [IETF RFC 1945](http://tools.ietf.org/html/rfc1945#section-10.16)
   *  Simple!
   *  Plaintext (SSL can "help")
   *  Relys on client/browser authentication popup

Example with Basic Auth:

    curl -u "username:password" https://www.example.com/special

----

## Auth experience

*  Session Auth (cookies) [IETF RFC 6265](http://tools.ietf.org/html/rfc6265)
   *  "free" with most web frameworks
   *  A session is usually tied to one user account and is normally authorized fully or not.

Example with cookies holding state through a session:

    curl -c cookiejar.txt https://www.example.com/login
    curl -b cookiejar.txt https://www.example.com/special

----

## Auth experience

*  OAuth [IETF RFC 5849](http://tools.ietf.org/html/rfc5849)
   *  "Valet key for the web" (limited access OAuth Token)
   *  Scope access to resources

Example of a signed OAuth request:

    <special signing sauce>
    curl -v -H 'Authorization: OAuth
     oauth_consumer_key="zsQpwbL3AGRNV4272Xc8Msi3hxhQWGrS",
     oauth_signature_method="HMAC-SHA1",
     oauth_timestamp="1346887460",
     oauth_nonce="1548267549",
     oauth_version="1.0",
     oauth_token="wvokahqtGMLS5o4AvVvokGZaA9pZjBcW",
     oauth_signature="tvHRw2fLNxYE2FR62EfH6tAfBW4%3D"'
    https://www.example.com/special


----

## So what's so special about OAuth?

![Authorization has meaning](http://hueniverse.com/wp-content/uploads/2007/12/My-Endpoints.png)

----

## So what's so special about OAuth?

*  Each **client** is assigned **credentials** granted by a **resource owner** to access a **protected resource** on a **server**
   *  Note: Client <-- (stuff) --> Server
   *  Therefore the credentials granted to a *specific client* can be *managed/revoked* by the resource owner.
*  An endpoint(s)/HTTP resource(s) can be **scoped** (to limit its functionality)
   *  "I give you (Ms. client) access to my API, but read only."
   *  "Access all of my public data (Mr. client)"
   *  "(Mrs. Client) Is requesting access you your bank account, allow?"

----

# Halt!

What about that guy on the internet ([Eran Hammer](http://hueniverse.com/oauth/)) that was like "OAuth 2.0 sucks" **!!rage quit!!** ?!!

Agree or disgree … most importantly: he's an expert and is prosthelytizing great information - **Listen to him!** (and read carefully)

**The takeaway:** dont throw the baby out with the bathwater and his commentary is directed at the 2.0 *draft*

----

# Let's Dance

![Footloose dance](http://f.cl.ly/items/2f1T0o283F2T1B1a3V0Y/footlose1_i270x270.jpg)

----

## What OAuth looks like

![Facebook](http://f.cl.ly/items/1d2i1g2R3S2L3b2b2X3O/20120906001549.png)
![Twitter](http://f.cl.ly/items/031j2u1K1b2P1i1e3u1G/20120906001612.png)
![AllPlayers](http://f.cl.ly/items/2A2F3P2I1D3p0T2E390d/20120906001808.png)

----

## What OAuth looks like: [Protocol workflow](http://hueniverse.com/oauth/guide/workflow/)

![faji: photo gallery site](http://hueniverse.com/wp-content/uploads/2009/09/screen1.png)

![beppa: photo printing site](http://hueniverse.com/wp-content/uploads/2009/09/screen2.png)

----

## What OAuth looks like: [Protocol workflow](http://hueniverse.com/oauth/guide/workflow/)

![Access faji from beppa's site](http://hueniverse.com/wp-content/uploads/2009/12/flow2grfc.png)

![Redirection to get access on faji](http://hueniverse.com/wp-content/uploads/2009/09/screen3.png)

----

## What OAuth looks like: [Protocol workflow](http://hueniverse.com/oauth/guide/workflow/)

![Approve/Authorize faji to give tokens to beppa to access as you](http://hueniverse.com/wp-content/uploads/2009/09/screen4.png)

![Redirect to Beppa](http://hueniverse.com/wp-content/uploads/2009/09/screen5.png)

----

## What OAuth looks like: [Protocol workflow](http://hueniverse.com/oauth/guide/workflow/)

![Ask again using temp tokens and get real live access tokens](http://hueniverse.com/wp-content/uploads/2009/12/flow3grfc.png)

![Request from the API with real live access tokens](http://hueniverse.com/wp-content/uploads/2009/09/screen6.png)

----

## Technical pieces

### [Terminology](http://tools.ietf.org/html/rfc5849#section-1.1)

*  Consumer:  client
*  Service Provider:  server
*  User:  resource owner
*  Consumer Key and Secret:  client credentials
*  Request Token and Secret:  temporary credentials
*  Access Token and Secret:  token credentials


### URL pattern(s)

(Related to protocol workflow)

*  https://provider.example.net/{initiate,request_token} (Temporary Credential Request)
*  https://provider.example.net/authorize (Resource Owner Authorization URI)
*  https://provider.example.net/{token,access_token} (Token Request UR)
*  http://consumer.example.com/{oauth_redirect,ready,...}

(Ref: URL patterns for [Twitter](https://dev.twitter.com/), [AllPlayers.com](http://develop.allplayers.com/oauth.html))

----

## Refs

*  http://tools.ietf.org/html/rfc1945#section-10.16 / http://en.wikipedia.org/wiki/Basic_access_authentication
*  http://tools.ietf.org/html/rfc6265 / http://en.wikipedia.org/wiki/HTTP_cookie
*  http://tools.ietf.org/html/rfc5849 / http://en.wikipedia.org/wiki/OAuth
*  [OAuth Checklist](http://oauthchecklist.org/)
*  [Build out and scratch pad notes](https://gist.github.com/1595718)
*  [Public Key Cryptography: Diffie-Hellman Key Exchange](http://www.youtube.com/watch?v=3QnD2c4Xovk&feature=related)

