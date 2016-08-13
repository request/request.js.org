# OAuth Signing

[OAuth version 1.0](https://tools.ietf.org/html/rfc5849) is supported.  The
default signing algorithm is
[HMAC-SHA1](https://tools.ietf.org/html/rfc5849#section-3.4.2):

```js
// OAuth1.0 - 3-legged server side flow (Twitter example)
// step 1
var qs = require('querystring')
  , oauth =
    { callback: 'http://mysite.com/callback/'
    , consumer_key: CONSUMER_KEY
    , consumer_secret: CONSUMER_SECRET
    }
  , url = 'https://api.twitter.com/oauth/request_token'
  ;
request.post({url:url, oauth:oauth}, function (e, r, body) {
  // Ideally, you would take the body in the response
  // and construct a URL that a user clicks on (like a sign in button).
  // The verifier is only available in the response after a user has
  // verified with twitter that they are authorizing your app.

  // step 2
  var req_data = qs.parse(body)
  var uri = 'https://api.twitter.com/oauth/authenticate'
    + '?' + qs.stringify({oauth_token: req_data.oauth_token})
  // redirect the user to the authorize uri

  // step 3
  // after the user is redirected back to your server
  var auth_data = qs.parse(body)
    , oauth =
      { consumer_key: CONSUMER_KEY
      , consumer_secret: CONSUMER_SECRET
      , token: auth_data.oauth_token
      , token_secret: req_data.oauth_token_secret
      , verifier: auth_data.oauth_verifier
      }
    , url = 'https://api.twitter.com/oauth/access_token'
    ;
  request.post({url:url, oauth:oauth}, function (e, r, body) {
    // ready to make signed requests on behalf of the user
    var perm_data = qs.parse(body)
      , oauth =
        { consumer_key: CONSUMER_KEY
        , consumer_secret: CONSUMER_SECRET
        , token: perm_data.oauth_token
        , token_secret: perm_data.oauth_token_secret
        }
      , url = 'https://api.twitter.com/1.1/users/show.json'
      , qs =
        { screen_name: perm_data.screen_name
        , user_id: perm_data.user_id
        }
      ;
    request.get({url:url, oauth:oauth, qs:qs, json:true}, function (e, r, user) {
      console.log(user)
    })
  })
})
```

For [RSA-SHA1 signing](https://tools.ietf.org/html/rfc5849#section-3.4.3), make
the following changes to the OAuth options object:
* Pass `signature_method : 'RSA-SHA1'`
* Instead of `consumer_secret`, specify a `private_key` string in
  [PEM format](http://how2ssl.com/articles/working_with_pem_files/)

For [PLAINTEXT signing](http://oauth.net/core/1.0/#anchor22), make
the following changes to the OAuth options object:
* Pass `signature_method : 'PLAINTEXT'`

To send OAuth parameters via query params or in a post body as described in The
[Consumer Request Parameters](http://oauth.net/core/1.0/#consumer_req_param)
section of the oauth1 spec:
* Pass `transport_method : 'query'` or `transport_method : 'body'` in the OAuth
  options object.
* `transport_method` defaults to `'header'`

To use [Request Body Hash](https://oauth.googlecode.com/svn/spec/ext/body_hash/1.0/oauth-bodyhash.html) you can either
* Manually generate the body hash and pass it as a string `body_hash: '...'`
* Automatically generate the body hash by passing `body_hash: true`

---

[back to README.md](../../README.md#table-of-contents)
