## Cloud Foundry build pack: Erlang

This is an updated Cloud Foundry build pack for Erlang OTP apps. Apps are built using [Rebar3](http://www.rebar3.org/) which in turn, assumes your Erlang app is a standard OTP app.

It is based on an older version by [Aaron Speigel](https://github.com/spiegela/cf-buildpack-erlang) that appears no longer to be maintained.

## Update

The following updates have been:

* Erlang tarballs are downloaded from Heroku Cedar 14 stack
* Rebar3 is used instead of Rebar2.x
* Default Erlang OTP version is now `20.1`, not `master`.

### Select an Erlang version

The Erlang/OTP release version that will be used to build and run your application is now sourced from a dot file called `.preferred_otp_version`.

This file must exist in the root directory of your repo and must contain only the OTP version number you require.  Pre-compiled binaries from version from `17.0` upwards are available from the Cedar 14 stack.

#### Example

If your `.preferred_otp_version` file contains `19.2`, then the file `OTP-19.2.tar.gz` will be downloaded from `https://s3.amazonaws.com/heroku-buildpack-elixir/erlang/cedar-14/`.


### Create a Procfile for CF to run

Rebar projects must be run via their application name, so it must be defined in a Procfile

    $ echo "web: erl -pa ebin deps/*/ebin -noshell -s <app name>" > Procfile
    $ git commit -m "Added Procfile" Procfile

### Build your CF App

    $ cf push <app name> -b https://github.com/ChrisWhealy/cf-buildpack-erlang

You may need to write a new commit and push if your code was already up to date.
