<p align="center"><a href="https://auth0.com" target="_blank"><img src=".github/logo.svg?sanitize=true&raw=true" width="300"></a></p>

<p align="center">
<a href="https://circleci.com/gh/auth0/auth0-PHP"><img src="https://img.shields.io/circleci/project/github/auth0/auth0-PHP/master.svg" alt="Build Status"></a>
<a href="https://packagist.org/packages/auth0/auth0-PHP"><img src="https://img.shields.io/packagist/dt/auth0/auth0-PHP" alt="Total Downloads"></a>
<a href="https://packagist.org/packages/auth0/auth0-PHP"><img src="https://img.shields.io/packagist/v/auth0/auth0-PHP?label=stable" alt="Latest Stable Version"></a>
<a href="https://packagist.org/packages/auth0/auth0-PHP"><img src="https://img.shields.io/packagist/php-v/auth0/auth0-php" alt="PHP Support"></a>
<a href="https://codecov.io/gh/auth0/auth0-PHP"><img src="https://codecov.io/gh/auth0/auth0-PHP/branch/master/graph/badge.svg" alt="Code Coverage"></a>
<a href="https://packagist.org/packages/auth0/auth0-PHP"><img src="https://img.shields.io/packagist/l/auth0/auth0-php" alt="License"></a>
<a href="https://app.fossa.com/projects/git%2Bgithub.com%2Fauth0%2Fauth0-PHP?ref=badge_shield"><img src="https://app.fossa.com/api/projects/git%2Bgithub.com%2Fauth0%2Fauth0-PHP.svg?type=shield" alt="FOSSA"></a>
</p>

Auth0 enables you to rapidly integrate authentication and authorization for your applications, so you can focus on your core business. ([Learn more](https://auth0.com/why-auth0))

Our PHP SDK provides a straight-forward and rigorously tested interface for accessing Auth0's Authentication and Management API endpoints through modern releases of PHP.

This is [one of many libraries we offer](https://auth0.com/docs/libraries) supporting numerous platforms.

## Documentation

- [Documentation](https://auth0.com/docs/libraries/auth0-php)
  - [Installation](https://auth0.com/docs/libraries/auth0-php#installation)
  - [Getting Started](https://auth0.com/docs/libraries/auth0-php#getting-started)
  - [Basic Usage](https://auth0.com/docs/libraries/auth0-php/auth0-php-basic-use)
  - [Authentication API](https://auth0.com/docs/libraries/auth0-php/using-the-authentication-api-with-auth0-php)
  - [Management API](https://auth0.com/docs/libraries/auth0-php/using-the-management-api-with-auth0-php)
  - [Troubleshooting](https://auth0.com/docs/libraries/auth0-php/troubleshoot-auth0-php-library)
- Quickstarts
  - [Basic authentication example](https://auth0.com/docs/quickstart/webapp/php/) ([GitHub repo](https://github.com/auth0-samples/auth0-php-web-app/tree/master/00-Starter-Seed))
  - [Authenticated backend API example](https://auth0.com/docs/quickstart/backend/php/) ([GitHub repo](https://github.com/auth0-samples/auth0-php-api-samples/tree/master/01-Authenticate))

## Installation

The recommended way to install the SDK is through [Composer](https://getcomposer.org/):

```bash
$ composer require auth0/auth0-php
```

Guidance on setting up Composer and alternative installation methods can be found in our [documentation](https://auth0.com/docs/libraries/auth0-php#installation).

## Getting Started

To get started, you'll need to create a [free Auth0 account](https://auth0.com/signup) and register an [Application](https://auth0.com/docs/applications).

### Authentication API Client

Begin by instantiating the SDK and passing the relevant details from your Application's settings page:

```php
use Auth0\SDK\Auth0;

$auth0 = new Auth0([
  // The values below are found on the Application settings tab.
  'domain'        => '{YOUR_TENANT}.auth0.com',
  'client_id'     => '{YOUR_APPLICATION_CLIENT_ID}',
  'client_secret' => '{YOUR_APPLICATION_CLIENT_SECRET}',

  // This is your application URL that will be used to process the login.
  // Save this URL in the "Allowed Callback URLs" field on the Application settings tab
  'redirect_uri' => 'https://{YOUR_APPLICATION_CALLBACK_URL}',
]);
```

**Note:** _In a production application you should never hardcode these values. Consider using environment variables to store and pass these values to your application, as suggested in our [documentation](https://auth0.com/docs/libraries/auth0-php#getting-started)._

Using the SDK, making requests to Auth0's endpoints couldn't be simpler. For example, signing users in using Auth0's [Universal Login](https://auth0.com/docs/universal-login) and retrieving user details can be done in a few lines of code:

```php
// Do we have an authenticated session available?
if ($userinfo = $auth0->getUser()) {
  // Output the authenticated user
  var_dump($userinfo);
  exit;
}

// No session was available, so redirect to Universal Login page
$auth0->login();
```

Further examples of how you can use the Authentication API Client can be found on [our documentation site](https://auth0.com/docs/libraries/auth0-php/).

### Management API Client

This SDK also offers an interface for Auth0's Management API which, in order to access, requires an Access Token that is issued specifically for your tenant's Management API by specifying the corresponding Audience.

The process for retrieving such an Access Token is described in our [documentation](https://auth0.com/docs/libraries/auth0-php/using-the-management-api-with-auth0-php).

```php
use Auth0\SDK\API\Management;

$mgmt_api = new Management('{{YOUR_ACCESS_TOKEN}}', 'https://{{YOUR_TENANT}}.auth0.com');
```

The SDK provides convenient interfaces to the Management API's endpoints. For example, to search for users:

```php
$results = $mgmt_api->users()->getAll([
  'q' => 'josh'
]);

if (! empty($results)) {
  echo '<h2>User Search</h2>';

  foreach ($results as $datum) {
    printf(
      '<p><strong>%s</strong> &lt;%s&gt; - %s</p>',
      !empty($datum['nickname']) ? $datum['nickname'] : 'No nickname',
      !empty($datum['email']) ? $datum['email'] : 'No email',
      $datum['user_id']
    );
  }
}
```

At the moment the best way to see what endpoints are covered is to read through the `\Auth0\SDK\API\Management` class, [available here](https://github.com/auth0/auth0-PHP/blob/master/src/API/Management.php).

## Contributing

We appreciate your feedback and contributions to the project! Before you get started, please review the following:

- [Auth0's general contribution guidelines](https://github.com/auth0/open-source-template/blob/master/GENERAL-CONTRIBUTING.md)
- [Auth0's code of conduct guidelines](https://github.com/auth0/open-source-template/blob/master/CODE-OF-CONDUCT.md)
- [The Auth0 PHP SDK contribution guide](CONTRIBUTING.md)

## Support + Feedback

- The [Auth0 Community](https://community.auth0.com/) is a valuable resource for asking questions and finding answers, staffed by the Auth0 team and a community of enthusiastic developers
- For code-level support (such as feature requests and bug reports) we encourage you to [open issues](https://github.com/auth0/auth0-PHP/issues) here on our repo
- For customers on [paid plans](https://auth0.com/pricing/), our [support center](https://support.auth0.com/) is available for opening tickets with our knowledgeable support specialists

Further details about our support solutions are [available on our website.](https://auth0.com/docs/support)

## Vulnerability Reporting

Please do not report security vulnerabilities on the public GitHub issue tracker. The [Responsible Disclosure Program](https://auth0.com/whitehat) details the procedure for disclosing security issues.

## What is Auth0?

Auth0 helps you to:

- Add authentication with [multiple authentication sources](https://docs.auth0.com/identityproviders), either social like Google, Facebook, Microsoft, LinkedIn, GitHub, Twitter, Box, Salesforce (amongst others), or enterprise identity systems like Windows Azure AD, Google Apps, Active Directory, ADFS or any SAML Identity Provider.
- Add authentication through more traditional **[username/password databases](https://docs.auth0.com/mysql-connection-tutorial)**.
- Add support for [passwordless](https://auth0.com/passwordless) and [multi-factor authentication](https://auth0.com/docs/mfa).
- Add support for [linking different user accounts](https://docs.auth0.com/link-accounts) with the same user.
- Analytics of how, when and where users are logging in.
- Pull data from other sources and add it to the user profile, through [JavaScript rules](https://docs.auth0.com/rules).

[Why Auth0?](https://auth0.com/why-auth0)

## License

The Auth0 PHP SDK is open source software licensed under [the MIT license](https://opensource.org/licenses/MIT). See the [LICENSE](LICENSE.txt) file for more info.

[![FOSSA Status](https://app.fossa.com/api/projects/git%2Bgithub.com%2Fauth0%2Fauth0-PHP.svg?type=large)](https://app.fossa.com/projects/git%2Bgithub.com%2Fauth0%2Fauth0-PHP?ref=badge_large)
