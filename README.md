This is a fairly generic REST interface wrapper that allows Chargify URLs to be accessed from node.js.
See <http://docs.chargify.com/api-resources> and surrounding pages for up-to-date Chargify documentation.


## Examples ##

A site is wrapped in a representation of its base URL:

    var wrapped_site = chargify.wrapSite('example-site', "API_KEY");

This base URL can be extended...

    var some_subscription = wrapped_site('subscriptions')(42);

...and queried (i.e. GET):

    some_subscription(function (status, data) { if (status === 200) console.log(data.subscription.state); });

...and updated (i.e. PUT):

    some_subscription('components')(5)({component:{allocated_quantity:9}}, function (status, info) { console.log(info); });

Or, used to add or remove objects (i.e. POST, DELETE):

    var customerInfo = {customer:{first_name:"Sir",last_name:"Pedro",email:"user@example.com"}};
    wrapped_site('customers').add(customerInfo, function (s, info) {
        // NOTE: Chargify currently returns 403 from this request, as customer deletion is not supported
        wrapped_site('customers')(info.customer.id).remove(function (s, info) {});
    });


## Documentation ##

The module has but one function:

* `chargify.wrapSite(subdomain, key)` - return base URL wrapper for a Chargify site using the given API key.

The actions available on this URL wrapper interface are as follows:

* `()` - return wrapped URL as string
* `(string/number/etc.)` - return another URL wrapper with given path component appended
* `(callback)` - GET on wrapped URL [alias for `.get(null, cb)`]
* `(dict, callback)` - PUT on wrapped URL [alias for `.put`]
* `.add(dict, callback)` - POST on wrapped URL [alias for `.post`]
* `.remove(callback)` - DELETE on wrapped URL [alias for `.del(null, cb)`]
* `.get(dict, callback)` - GET on wrapped URL with query parameters from dict
* `.put(dict, callback)` - PUT on wrapped URL with dict sent as JSON body
* `.post(dict, callback)` - POST on wrapped URL with dict sent as JSON body
* `.del(dict, callback)` - DELETE on wrapped URL with dict sent as JSON body
* `.req(query, method, dict, callback)` - _METHOD_ on wrapped URL (plus query parameters) with data as JSON for body (`query` and `data` may be null)


## License ##

Copyright © 2011 by &yet, LLC. Released under the terms of the MIT License:

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.