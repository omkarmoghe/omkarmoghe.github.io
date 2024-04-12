---
layout: project
title: "Pokémap"
github_url: "https://github.com/omkarmoghe/pokemap"
languages:
  - Java (Android)
emoji: 🔎
order: 2
---

[Pokémap](http://pokemapgo.xyz/) was a project I built, with the help of a ton of [talented individuals](https://github.com/omkarmoghe/Pokemap/graphs/contributors), during the peak of the Pokémon Go hype in the summer of 2016. It started with a Reddit post where someone had [reverse engineered](https://github.com/AHAAAAAAA/PokemonGo-Map/) (retired) the Pokémon Go API in Javascript.

At the time I was pretty involved in Android development, and decided to create an Android app using the API which someone had ported to Java. The idea was simple &mdash; create a simple app to display nearby Pokémon. In order to use the API you needed a valid token, or in other words, you needed to pretend like you were a legitimate Pokémon Go client.

## Obtaining the API Key

Obtaining an API key was the first challenge. Luckily, other users on Reddit and GitHub had traced the Pokémon Go app's network traffic to figure out some key URLs and how the clients communicated with Niantic's servers.

```java
private static final String LOGIN_URL = "https://sso.pokemon.com/sso/login?service=https://sso.pokemon.com/sso/oauth2.0/callbackAuthorize";
private static final String LOGIN_OAUTH = "https://sso.pokemon.com/sso/oauth2.0/accessToken";
private static final String PTC_CLIENT_SECRET = "w8ScCUXJQc6kXKw8FiOhd8Fixzht18Dq3PEVkUCP5ZPxtgyWsbTvWHFLm2wNY0JR";
public static final String CLIENT_ID = "mobile-app_pokemon-go";
public static final String REDIRECT_URI = "https://www.nianticlabs.com/pokemongo/error";
```

Shout-out to whoever pulled the client secret from a packaged app. With this we could initiate a login request with a Pokémon Go player's legitimate credentials, intercept and follow the redirect ourselves to get a valid API token. From my Android app's `MainActivity`, I invoked a method I called `getToken` which handled this flow.

```java
private void getToken(final String username, final String password) throws IOException {
    // Build the HTTP client with our middleware and config.
    final OkHttpClient client = new OkHttpClient.Builder()
        .hostnameVerifier(new HostnameVerifier() {
            @Override
            public boolean verify(String s, SSLSession sslSession) {
                return true;
            }
        })
        .cookieJar(new PersistentCookieJar(new SetCookieCache(), new SharedPrefsCookiePersistor(getApplicationContext())))
        .followRedirects(false)
        .followSslRedirects(false)
        .build();

    // Kick off the initial login request.
    Request initialRequest = new Request.Builder()
        .addHeader("User-Agent", "Niantic App")
        .url(LOGIN_URL)
        .build();

    client.newCall(initialRequest).enqueue(new Callback() {
        @Override
        public void onFailure(Call call, IOException e) {
            Log.e(TAG, "fuck :(", e);
        }

        @Override
        public void onResponse(Call call, Response response) throws IOException {
            String body = response.body().string();
            try {
                JSONObject data = new JSONObject(body);
                Log.d(TAG, data.toString());

                // Get some data from the response and build the login request with the actual user info.
                RequestBody formBody = new FormBody.Builder()
                    .add("lt", data.getString("lt"))
                    .add("execution", data.getString("execution"))
                    .add("_eventId", "submit")
                    .add("username", username)
                    .add("password", password)
                    .build();

                Request interceptRedirect = new Request.Builder()
                    .addHeader("User-Agent", "Niantic App")
                    .url(LOGIN_URL)
                    .post(formBody)
                    .build();

                client.newCall(interceptRedirect).enqueue(new Callback() {
                    @Override
                    public void onFailure(Call call, IOException e) {
                        Log.e(TAG, "fuck :(", e);
                    }

                    @Override
                    public void onResponse(Call call, Response response) throws IOException {
                        Log.d(TAG, String.valueOf(response.code())); // should be a 302 (redirect)
                        Log.d(TAG, response.headers().toString()); // should contain a "Location" header

                        String ticket = response.header("Location").split("ticket=")[1];

                        // Grab the `ticket` from the redirect response's header. Use that in our final login request.
                        RequestBody loginForm = new FormBody.Builder()
                            .add("client_id", CLIENT_ID)
                            .add("redirect_uri", REDIRECT_URI)
                            .add("client_secret", PTC_CLIENT_SECRET)
                            .add("grant_type", "refresh_token")
                            .add("code", ticket)
                            .build();

                        Request loginRequest = new Request.Builder()
                            .addHeader("User-Agent", "Niantic App")
                            .url(LOGIN_OAUTH)
                            .post(loginForm)
                            .build();

                        client.newCall(loginRequest).enqueue(new Callback() {
                            @Override
                            public void onFailure(Call call, IOException e) {
                            }

                            @Override
                            public void onResponse(Call call, Response response) throws IOException {
                                // Clean & grab the token!
                                String rawToken = response.body().string();
                                String cleanToken = rawToken.replaceAll("&expires.*", "").replaceAll(".*access_token=", "");

                                Log.d(TAG, cleanToken); // success!

                                token = cleanToken;
                            }
                        });
                    }
                });
            } catch (JSONException e) {
                e.printStackTrace();
            }
        }
    });
}
```

Yeah, I know, Java is annoying to read. You can view the entire `MainActivity` [on GitHub](https://github.com/omkarmoghe/Pokemap/blob/b668cc89e2b8230a2fb19c33fa3bc9aeb5771a53/app/src/main/java/com/omkarmoghe/pokemap/MainActivity.java). At this point, Pokémap was not just an idea &mdash; it was alive. I created the repo and pushed the basic Android app with virtually no user interface, but a working "login" that effectively pretended to be just another Pokémon Go client.

With the `token`, anyone could pretend to be a legitimate Pokémon Go client and make requests to the game's servers. All of the guards were seemed to be enforced within the clients, at least early in the game's history. This meant that clients could act as bots and effectively farm nearby Pokémon by simply telling the server that it had been caught (which was determined on the client). While project simply aimed to provide mapping and tracking capabilities as a _Pokédex_ of sorts, many forks of our repo would become bots designed to track and instantly catch various Pokémon. In hindsight, I think this was added incentive for Niantic to shut down such projects, both benign or otherwise.

## GPS Spoofing & Hex Search

Search in the map worked by requesting nearby Pokémon from the game sever by providing it with your current location.

## Cease and Desist

On Friday, January 20th 2017 we received a cease and desist letter from The Pokémap Company International, Inc. and Niantic, Inc.

![Cease and desist public notice](https://i.imgur.com/Pr9zqY4.png)

I wonder if they've seen this website?
