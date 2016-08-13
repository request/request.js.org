# RequestJar

This is a wrapper for the synchronous interface of [`tough-cookie`][tough-cookie]'s [`CookieJar`][tough-cookie-cookiejar].



## Methods

### `.setCookie(cookieOrStr, uri[, options])`

Adds a cookie to the jar.

- `cookieOrStr`
  - Type: {[`Cookie`][tough-cookie-cookie] | `string`}

- `uri`
  - Type: {`string`}

- `options`
  - Type: {[`SetCookieOptions`][tough-cookie-cookiejar-setcookie]}
  - Optional

- **return**
  - Type: {[`Cookie`][tough-cookie-cookie]}
  
  **TODO**: check if you get a copy or the same cookie when passing a [`Cookie`][tough-cookie-cookie] for `cookieOrStr`.

### `.getCookieString(uri)`

**TODO**: Add description

- `uri`
  - Type: {`string`}

- **return**
  - Type: {`string`}

### `.getCookies(uri)`

**TODO**: Add description

- `uri`
  - Type: {`string`}

- **return**
  - Type: {`Array<`[`Cookie`][tough-cookie-cookie]`>`}



[tough-cookie]: https://github.com/SalesforceEng/tough-cookie
[tough-cookie-cookie]: https://github.com/SalesforceEng/tough-cookie#cookie
[tough-cookie-cookiejar]: https://github.com/SalesforceEng/tough-cookie#cookiejar
[tough-cookie-cookiejar-setcookie]: https://github.com/SalesforceEng/tough-cookie#setcookiecookieorstring-currenturl-options-cberrcookie
