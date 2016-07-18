# Node Adwords Api

This is an unofficial Adwords API for NodeJS > 3.0. This Api mirrors the official
PHP api pretty well so you can always look at that documentation if
something doesn't stand out.

You will Need an Adwords developer token. Apply [here](https://developers.google.com/adwords/api/docs/guides/signup)

## Getting Started

The main adwords user object follows the [auth] (https://github.com/googleads/googleads-php-lib/blob/master/src/Google/Api/Ads/AdWords/auth.ini) parameters

```js
var AdwordsUser = require('node-adwords').AdwordsUser;

var user = new AdwordsUser({
    developerToken: 'INSERT_DEVELOPER_TOKEN_HERE', //your adwords developerToken
    userAgent: 'INSERT_COMPANY_NAME_HERE', //any company name
    clientCustomerId: 'INSERT_CLIENT_CUSTOMER_ID_HERE', //the Adwords Account id (e.g. 123-123-123)
    client_id: 'INSERT_OAUTH2_CLIENT_ID_HERE', //this is the api console client_id
    client_secret: 'INSERT_OAUTH2_CLIENT_SECRET_HERE',
    refresh_token: 'INSERT_OAUTH2_REFRESH_TOKEN_HERE'
});
```

## Usage

```js
var AdwordsUser = require('node-adwords').AdwordsUser;
var AdwordsConstants = require('node-adwords').AdwordsConstants;

var user = new AdwordsUser({...});
var campaignService = user.getService('CampaignService', 'v201605')

//create selector
var selector = {
    fields: ['Id', 'Name'],
    ordering: [{field: 'Name', sortOrder: 'ASCENDING'}],
    paging: {startIndex: 0, numberResults: AdwordsConstants.RECOMMENDED_PAGE_SIZE}
}

campaignService.get(selector, (error, campaigns) => {
    console.log(error, campaigns);
})

```

## Authentication
We use the [official google api client](https://github.com/google/google-api-nodejs-client)
for authenticating users. Using the `https://www.googleapis.com/auth/adwords` scope.
The node-adwords api has some helper methods for you to authenticate if you do not
need additional scopes.

```js
var AdwordsUser = require('node-adwords').AdwordsAuth;

var auth = new AdwordsAuth({
    client_id: 'INSERT_OAUTH2_CLIENT_ID_HERE', //this is the api console client_id
    client_secret: 'INSERT_OAUTH2_CLIENT_SECRET_HERE'
}, 'https://myredirecturlhere.com/adwords/auth' /** insert your redirect url here */);

//assuming express
app.get('/adwords/go', (req, res) => {
    res.redirect(auth.generateAuthenticationUrl());
})

app.get('/adwords/auth', (req, res) => {
    auth.getAccessTokenFromAuthorizationCode(req.query.code, (error, tokens) => {
        //save access and especially the refresh tokens here
    })
});

```


## Testing
For testing, you will need a refresh token as well as a developer token.
These should be placed as environmental variables:

```
$ ADWORDS_API_TEST_DEVTOKEN=123453152342352352
$ ADWORDS_API_TEST_REFRESHTOKEN=a45ikg94k3994kg94kg4k
$ npm test
```