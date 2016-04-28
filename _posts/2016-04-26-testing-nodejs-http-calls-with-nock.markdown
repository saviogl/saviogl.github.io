---
title: Testing Node.js HTTP calls with Nock
date:   2016-04-26 20:13:00
description: Mocking HTTP request with Nock library to allow your node modules tests to run in isolation
---

Tests have always been a fundamental step for building a stable and maintainable application, and the npm registry provides us with tons of great libraries to set them up. Most recently I stumbled upon a great HTTP mocking library when I was trying to find new ways of testing in isolation modules that perform HTTP requests. [Nock](https://github.com/node-nock/nock#nock) easily allows you to intercept any HTTP request call and provide a pre-defined HTTP response to it.

# Putting Nock into use
Earlier this month I had to create a passport [strategy](https://github.com/saviogl/passport-idsus) to enable a custom oAuth authentication system to work with [NodeBB](https://nodebb.org). This strategy needed to make use of a specific SDK which wraps the logic to communicate with the authentication system through HTTP calls. With that it's implied that to test the whole scope of this new module we would be dependent of an Internet connection, not to mention rely on a running server to respond to those calls. This would not be a good practice not to mention ineffective, and to avoid that and build reliable and independent test suites I've used Nock to handle those external calls.

## Quick Intro to Nock

Nock provides an easy and clean way of setting up interceptors for any given HTTP call. Suppose that you want to intercept a call made to **http://www.google.com** and reply with an error status code, and no content in the response's body, you would simply do:

``` javascript

nock('http://www.example.com')
    .get('/')
    .reply(500);

```

What Nock is doing in the above code is setting up a middle man (interceptor) in front of the **http(s)** native module to intercept any HTTP calls made to **http://www.google.com/** and reply with a 500 status code. Let's set up a minor example:

``` javascript

const nock = require('nock');
nock('http://www.google.com').get('/').reply(500);

const http = require('http');

http.get('http://www.google.com', res => {
    let chunck = '';
    res.on('data', data => { chunck += data });
    res.on('end', ()=>{
        console.log(`End: ${chunck}`);
        console.log(`Status code: ${res.statusCode}`);
    });
})
.on('error', e => { console.log(`Got error: ${e.message}`); })

```

Running the code above will output the following to the terminal:

```
End:
Status code: 500
```

If we were to reply the HTTP call with a success status code and some content in the response's body e.g. ***reply(200, 'Any content')*** we would get the following output:

```
End: Any content
Status code: 200
```

[Nock](https://github.com/node-nock/nock#nock) has tons of features to cover pretty much anything related to the HTTP protocol which makes it very powerful and perfect for testing.

## Test Case Example

Now that we are more familiar with Nock let me show you an example of how I integrated Nock into my test suite for the passport strategy module. As I've explained before, this module wraps a custom oAuth authentication sdk and to complete the flow of the authentication two HTTP requests are necessary, one to retrieve the access token and another one to retrieve the user's data. This way I had to set up two Nocks interceptors one for each url that resolves the two step authentication flow.

``` javascript
  describe('test', function() {
    before(function() {
      // Setup Nock inteceptor for valid token
      nock(IdsusStrategyParams.authURL)
        .post('/oauth/token/')
        .reply(200, fixtures.accessToken);

      // Setup Nock inteceptor for user profile
      nock(IdsusStrategyParams.authURL)
        .get('/api/user/')
        .query(true)
        .reply(200, fixtures.userSchema);
    });
    ...
  });
```

As you can see over the execution of this test Nock will intercept the HTTP calls made to those requests at those specific paths and reply with the content given by us. Isn't it awesome? This way we have a fully functional, isolated and independent test case making our tests more reliable and our module more mature. 

You can check the complete test spec [here](https://github.com/saviogl/passport-idsus/blob/master/test/strategy.spec.js#L107)

# Until the next post

There are other ways of mocking HTTP requests for tests purpose, but Nock definitely stands out for its simplicity and effectiveness. A shout out to the [guys](https://github.com/node-nock/nock/graphs/contributors) behind Nock library, great job!