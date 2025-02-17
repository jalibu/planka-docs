# OIDC
## Single Sign on with OIDC

Planka can be configured to use an OIDC provider for logging in. If a user doesn't exist it will be automatically created. If a user exists and the email claim matches the email stored in Planka the accounts will be linked.

### Required Configuration Values
* **OIDC_ISSUER**: URL pointing to the identity provider. This is used to pull the `.well-known/openid-configuration` endpoint that is used to identify the necessary endpoints.
* **OIDC_CLIENT_ID**: The OAUTH client id you created in the identity provider.
* **OIDC_CLIENT_SECRET**: The OAUTH client secret you created in the identity provider.

### Optional Configuration Values
* **OIDC_SCOPES**: Scopes to request from the identity provider. This controls what values the OAuth client has access to. Planka needs the email and name claims. By default it requests `openid profile email`.
* **OIDC_ADMIN_ROLES**: Looks in the claim declared by `OIDC_ROLES_ATTRIBUTE` to see if the user is an admin. By default the `admin` role is used.
* **OIDC_EMAIL_ATTRIBUTE**: The claim containing the email. By default `email` is used.
* **OIDC_NAME_ATTRIBUTE**: The claim containing the name. By default `name` is used.
* **OIDC_USERNAME_ATTRIBUTE**: The claim containing the username. By default `preferred_username` is used.
* **OIDC_ROLES_ATTRIBUTE**: The claim containing the group/roles that will be used to identify an admin. It is expected that this will be a flat list. By default `groups` is used.
* **OIDC_IGNORE_USERNAME**: If set to `true` the `OIDC_USERNAME_ATTRIBUTE` will be ignored. This is useful if the format of usernames in your identity provider differs from the format in Planka. By default it's not ignored.
* **OIDC_IGNORE_ROLES**: If set to `true` the `OIDC_ADMIN_ROLES` and `OIDC_ROLES_ATTRIBUTE` will be ignored. This is useful if you want to use OIDC for authentication but not for authorization. Like that the user roles will be managed by Planka. By default they're not ignored.
* **OIDC_ENFORCED**: If set to `true` all built-in authentication/authorization will be deactivated. By default it's not enforced.

## Examples
### Authentik
This is an example of the environment variables used to configure Planka to use [Authentik](https://goauthentik.io/ "Homepage for authentik"). It will work with any OIDC provider.

```
OIDC_ISSUER=https://auth.local/application/o/planka/
OIDC_CLIENT_ID=sxxaAIAxVXlCxTmc1YLHBbQr8NL8MqLI2DUbt42d
OIDC_CLIENT_SECRET=om4RTMRVHRszU7bqxB7RZNkHIzA8e4sGYWxeCwIMYQXPwEBWe4SY5a0wwCe9ltB3zrq5f0dnFnp34cEHD7QSMHsKvV9AiV5Z7eqDraMnv0I8IFivmuV5wovAECAYreSI
OIDC_SCOPES=openid profile email
OIDC_ADMIN_ROLES=planka-admin
# OIDC_EMAIL_ATTRIBUTE=email
# OIDC_NAME_ATTRIBUTE=name
# OIDC_USERNAME_ATTRIBUTE=preferred_username
# OIDC_ROLES_ATTRIBUTE=groups
# OIDC_IGNORE_USERNAME=true
# OIDC_IGNORE_ROLES=true
# OIDC_ENFORCED=true
```

At least these values will need to be modified:
* `auth.local` is the domain authentik is running on.
* `sxxaAIAxVXlCxTmc1YLHBbQr8NL8MqLI2DUbt42d` is the client id generated by authentik.
* `om4RTMRVHRszU7bqxB7RZNkHIzA8e4sGYWxeCwIMYQXPwEBWe4SY5a0wwCe9ltB3zrq5f0dnFnp34cEHD7QSMHsKvV9AiV5Z7eqDraMnv0I8IFivmuV5wovAECAYreSI` is the client secret generated by authentik.
* `planka-admin` is the group in authentik, this is used to create admin accounts (or alternatively you can set `OIDC_IGNORE_ROLES` to `true`)

### Google

* Go to [console.cloud.google.com/apis/credentials](https://console.cloud.google.com/apis/credentials)
* Select a existing project at the top or create a new one
* Select “create credentials”
* Pick oAuth Client ID
* Application type: Web application
* Name: Planka
* Add Redirect URI: `https://your-domain.com/oidc-callback`
* Set the displayed ClientID and Client Secret as environment variables

```
OIDC_ISSUER=https://accounts.google.com
OIDC_CLIENT_ID=xxx-xxx.apps.googleusercontent.com
OIDC_CLIENT_SECRET=xxxx-xxxx-xx
OIDC_SCOPES=openid profile email
```
