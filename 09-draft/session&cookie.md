# Session & Cookie



## Session

1. Each session has a beginning and an end.
2. Each session is relatively short-lived.
3. Either the user agent or the origin server may terminate a session.
4. The session is implicit in the exchange of state information.





## Cookie

>  Document.cookie

### Intro[ 3]

An HTTP cookie (web cookie, browser cookie) is a small piece of data that a server sends to the user's web browser. The browser may store it and send it back with later requests to the same server. Typically, it's used to tell if two requests came from the same browser — keeping a user logged-in, for example. It remembers stateful information for the [stateless](https://developer.mozilla.org/en-US/docs/Web/HTTP/Overview#HTTP_is_stateless_but_not_sessionless) HTTP protocol.



#### Used for three purposes:

- Session management

  Logins, shopping carts, game scores, or anything else the server should remember

- Personalization

  User preferences, themes, and other settings

- Tracking

  Recording and analyzing user behavior



### Attributes

- Secure

  A cookie with the `Secure` attribute is sent to the server only with an encrypted request over the HTTPS protocol

- HttpOnly

  A cookie with the `HttpOnly` attribute is inaccessible to the JavaScript [`Document.cookie`](https://developer.mozilla.org/en-US/docs/Web/API/Document/cookie) API

  For example, cookies that persist server-side sessions don't need to be available to JavaScript, and should have the `HttpOnly` attribute. This precaution helps mitigate cross-site scripting ([XSS](https://developer.mozilla.org/en-US/docs/Web/Security/Types_of_attacks#Cross-site_scripting_(XSS))) attacks.

- Domain

  Define the *scope* of the cookie: what URLs the cookies should be sent to.

  Which hosts are allowed to receive the cookie.

- Path

  define the *scope* of the cookie: what URLs the cookies should be sent to.

   indicates a URL path that must exist in the requested URL

- SameSite

  servers require that a cookie shouldn't be sent with cross-origin requests 

  It takes three possible values: `Strict`, `Lax`, and `None`



## Compare









筆記筆記

- 後端要設定各種 header, ex: Access-Control-Allow-Credentials

- 設定 cookie ，要同一個 domain 才可以設定成功 ex: localhost 不能設定 domain .lollipop.camera，但是布版到 .lollipop.camera 之後就可以設定成功了



## Reference

[1] https://blog.techbridge.cc/2019/08/10/session-and-cookie-rfc/

[2] https://developers.google.com/web/tools/chrome-devtools/storage/cookies

[3] https://developer.mozilla.org/en-US/docs/Web/HTTP/Cookies

[4] Header set-cookie https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Set-Cookie

