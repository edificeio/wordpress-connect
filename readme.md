## OpenID Connect Generic Client

License: GPLv2 or later
License URI: http://www.gnu.org/licenses/gpl-2.0.html

A simple module that lets you connect to a wordpress website using the ENT authorization system.

Credits to [daggerhart](https://github.com/daggerhart/openid-connect-generic). This module is an adaptation of his.

### Description

This plugin allows to authenticate users against OpenID Connect OAuth2 API with Authorization Code Flow. The authorization is provided by your ENT application.

### Installation
This module can be installed and configured in 3 easy steps:

1. Configure the ENT
2. Install and configure this wordpress plugin
3. Import authorized users

#### Configure the ENT

- Connect to the console admin
- Create a new app with the params below
- Give the workflow right (related to this app) to the group concerned

|Property|Value|
| ------------- |:-------------:|
|URL|WORDPRESS_DOMAIN/wp-admin/admin-post.php?action=openid-connect-redirect|
|Target|New Page|
|OAuth Scope|openid userinfo|
|Client ID|CLIENT_ID|
|Client Secret|CLIENT_SECRET|
|Identication Mode|Code|

> Make sure that you have configured your springboard to enable openidconnect (openid-connect field). And make sure that you have configured certificates and private keys that will be used to sign JWT tokens.

#### Install and Configure the wordpress module

- Upload this module to the `/wp-content/plugins/` directory
- Activate the plugin
- Visit Settings > OpenID Connect and configure as follows

|Property|Value|
| ------------- |:-------------:|
|Login Type|OpenID Connect button on login form|
|Client ID|CLIENT_ID|
|Client Secret|CLIENT_SECRET|
|OpenID Scope|openid userinfo|
|Login Endpoint URL|ENT_DOMAIN/auth/oauth2/auth|
|Userinfo Endpoint URL|ENT_DOMAIN/auth/oauth2/userinfo|
|Token Validation Endpoint URL|ENT_DOMAIN/auth/oauth2/token|
|End Session Endpoint URL|ENT_DOMAIN/auth/logout|
|Identity Key|username|
|Disable SSL Verify	|false in production|
|Nickname Key|username|
|Email Formatting|{email}|
|Display Name Formatting|{lastName} {firstName}|
|Identify with User Name|true|
|Link Existing Users|true|
|Redirect Back to Origin Page|true|
|Redirect to the login screen session is expired|true|
|Enforce Privacy|false|
|Alternate Redirect URI	|true|

> CLIENT_ID and CLIENT_SECRET are the same as ones saved in the ENT.


#### Import users

ENT users that can connect to the wordpress admin *must* exists in the wordpress database. 
The link between ENT users and wordpress users is done using *externalId*.
To import our users we will use this plugin :  [import-users-from-csv-with-meta/](https://wordpress.org/plugins/import-users-from-csv-with-meta/)

The CSV file to import should looks like as follow:

```
Username,Email,Password,openid-connect-generic-subject-identity
julie.devost,julie.devost@example.com,wp_password,1769fbd0-3e33-499f-9694-e09f0dc0b624
```

> The "openid-connect-generic-subject-identity" is in fact the externalId in the ENT.

When all is done, your users can see the new application and on click they will be redirected to the wordpress admin.