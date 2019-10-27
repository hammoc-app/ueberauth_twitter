# Überauth Twitter

> Twitter strategy for Überauth.

_Note_: Sessions are required for this strategy.

## Installation

1. Setup your application at [Twitter Developers](https://dev.twitter.com/).

1. Add `:ueberauth_twitter` to your list of dependencies in `mix.exs`:

    ```elixir
    def deps do
      [
        {:ueberauth_twitter, "~> 0.3"}
      ]
    end
    ```

1. Add Twitter to your Überauth configuration:

    ```elixir
    config :ueberauth, Ueberauth,
      providers: [
        twitter: {Ueberauth.Strategy.Twitter, []}
      ]
    ```

1.  Update your provider configuration:

    ```elixir
    config :ueberauth, Ueberauth.Strategy.Twitter.OAuth,
      consumer_key: System.get_env("TWITTER_CONSUMER_KEY"),
      consumer_secret: System.get_env("TWITTER_CONSUMER_SECRET")
    ```

1.  Include the Überauth plug in your controller:

    ```elixir
    defmodule MyApp.AuthController do
      use MyApp.Web, :controller
      plug Ueberauth
      ...
    end
    ```

1.  Create the request and callback routes if you haven't already:

    ```elixir
    scope "/auth", MyApp do
      pipe_through :browser

      get "/:provider", AuthController, :request
      get "/:provider/callback", AuthController, :callback
    end
    ```

1. Your controller needs to implement callbacks to deal with `Ueberauth.Auth` and `Ueberauth.Failure` responses.

For an example implementation see the [Überauth Example](https://github.com/ueberauth/ueberauth_example) application.

## Calling

Depending on the configured url you can initiate the request through:

    /auth/twitter

Query parameters such as `force_login` and `screen_name` will be forwarded to the [OAuth endpoint](https://developer.twitter.com/en/docs/basics/authentication/api-reference/authenticate). For example, to always ask for a user name and password (even if logged in to Twitter), initiate the request through:

    /auth/twitter?force_login=true

## Development mode

As noted when registering your application on the Twitter Developer site, you need to explicitly specify the `oauth_callback` url.  While in development, this is an example url you need to enter.

    Website - http://127.0.0.1
    Callback URL - http://127.0.0.1:4000/auth/twitter/callback

## License

Please see [LICENSE](https://github.com/ueberauth/ueberauth_twitter/blob/master/LICENSE) for licensing details.

