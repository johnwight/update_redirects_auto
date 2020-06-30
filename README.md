# Create redirect URIs programmatically

You can use the application management [`a/{client_id}`](https://docs.nylas.com/reference#application-management) API endpoint to create or update redirect URIs programmatically. This saves you the time and effort of manually configuring redirects on the Nylas dashboard. This endpoint provides a quick method for the following:

* Viewing current redirect URIs for a customer 

* Adding redirect URIs straight from code

* Helping your customers who have customized redirect URIs according to any application they access. With this endpoint, they can bookmark multiple applications.

* Whitelist multiple domains in cases where your customers use their own, separate domains to access your solution

Additionally, this endpoint provides help with application information, including:

* Viewing current application details for a customer 

* Updating an application name associated with a customer

To create or update a redirect URI with this API:

1. Make sure that the account is [properly authenticated](https://docs.nylas.com/docs/authentication-guide).

2. If applicable, see if the correct redirect URI or application name already exists for the customer. To see a list of the current redirect URIs and applications, make a `GET` call to the [`a/{client_id}`](https://docs.nylas.com/reference#application-management) endpoint, as shown in the following example.  

## Example request

```
curl -X GET "https://api.nylas.com/a/6yvh3ynadnwqtumyfper72svf"
-H "Content-Type: application/json" \
-H "Authorization: Basic mysUp3RS3crEtCl1EntS3crET="
```
In the above request example:

* `{client_id}` is the ID of your Nylas developer application (shown as `6yvh3ynadnwqtumyfper72svf`).

* Authorization is the client secret, passed as an HTTP Basic Auth username.

3. If the new redirect URI is not included in the response, make a `PUT` to the [`a/{client_id}`](https://docs.nylas.com/reference#application-management) endpoint. The authorization for this call follows the same pattern as the `GET` call. This endpoint includes two parameters: one for creating or updating the redirect URI and another for updating the associated application name. The following example shows how to update both:

## Example request

```
curl -X PUT "https://api.nylas.com/a/2ajt0hf90hrrfoi41j6fl95vr" \
-H "Content-Type: application/json" \
-d '{
    "application_name": "My New App Name",
    "redirect_uris": ["test.nylas.com", "staging.nylas.com", "newapp.nylas.com"]
}'
```

In the above example:

* The `application_name` parameter is the new name for the application (shown as `My New App Name`). This parameter is optional.

* The `redirict_uris` parameter takes an array of strings. Each string is a single redirect URI for the application. The request example shows three:

  * "test.nylas.com"
  * "staging.nylas.com"
  * "newapp.nylas.com"

### Important! 

* The entire array for this parameter must not exceed 64 KB in size. 

* Because `PUTS` are idempotent, you will need to include every redirect string each time you update with this endpoint. For example, if you add two more redirect URIs at a later date, you will need to include the previous three (for a total of five URIs) in order to both maintain the existing URIs and to add the new ones.

## Responses

* Responses are UTF-8 encoded JSON objects, unless otherwise noted.

* The format of the sync status codes with Nylas APIs is slightly different than the information displayed on the Nylas dashboard. See [API Sync Statuses](https://docs.nylas.com/docs/sync-statuses) for more information on using APIs for determining sync statuses.


## Additional Information

* In order to maintain security and full compliance with the [OAuth 2.0 specification](https://tools.ietf.org/html/rfc6749#section-3.1.2), wildcards are not supported with the [`a/{client_id}`](https://docs.nylas.com/reference#application-management) endpoint.

* The ability to edit the `client_id`, or the ID of your Nylas developer application, is not supported with this API.


      **This is the end of the sample document.**


## Questions for SMEs:

For this guide, I would present questions to the PM and the endpoint developer, unless otherwise directed.

### For PMs:

* When will this feature be released?
* Where and how do you want this delivered as a doc to the public?
* Any more feature or functional changes on this API before release?
* Can you review this doc and provide any technical corrections?

### For the endpoint developer: 

* What are the current limitations for migrating to mediumtext fields for `redirect_uris`? 
* If migration to mediumtext is currently functional, what specific or Nylas-unique steps need to be taken for this?
* Are you still providing spec changes for this API before its release date?
* Can you review this doc and provide any technical corrections?

## Assumptions

* A general, technical assumption: With the exception of status codes, this API endpoint replicates the same actions as the Dashboard UI.
* Response examples are worthwhile in such a guide but would be more useful with the proper environment setup and with organizational permissions, so I would add those later.

## Thanks!

Thanks to Tasia Potasinski whose [doc](https://www.nylas.com/blog/introducing-programmatic-redirect-of-uris/) was helpful in providing a model for this topic.

Thanks for the opportunity to write this guide. I enjoyed it! I would be very excited to write your API docs and really think your product is very cool.
