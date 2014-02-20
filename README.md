# Yesod.Auth.OAuth2

OAuth2 `AuthPlugin`s for Yesod.

## Basic Usage

To use one of the supported providers:

```haskell
import Yesod.Auth
import Yesod.Auth.OAuth2.Learn

instance YesodAuth App where
    -- ...

    -- https://learn.thoughtbot.com
    authPlugins _ = [oauth2Learn clientId clientSecret]

clientId :: Text
clientId = "..."

clientSecret :: Text
clientSecret = "..."
```

## Advanced Usage

To use any other provider:

```haskell
import Yesod.Auth
import Yesod.Auth.OAuth2

instance YesodAuth App where
    -- ...

    authPlugins _ = [myPlugin]

myPlugin :: AuthPlugin m
myPlugin = authOAuth2 "mysite"
    (OAuth2
        { oauthClientId            = "..."
        , oauthClientSecret        = "..."
        , oauthOAuthorizeEndpoint  = "https://mysite.com/oauth/authorize"
        , oauthAccessTokenEndpoint = "https://mysite.com/oauth/token"
        , oauthCallback            = Nothing
        })
    makeCredentials

makeCredentials :: AccessToken -> IO (Creds m)
makeCredentials token = do
    result <- authGetJSON token "https://mysite.com/api/me.json"
    return $ -- Parse the JSON into (Creds m)
```

*If you write one of these, please consider opening a Pull Request*
