## Cloud Foundry build pack: Erlang

This is an updated Cloud Foundry build pack for Erlang OTP apps. Apps are built using [Rebar3](http://www.rebar3.org/) which in turn, assumes your Erlang app is a standard OTP app.

It is based on an older version by [Aaron Speigel](https://github.com/spiegela/cf-buildpack-erlang) that appears no longer to be maintained.

## WARNING

This build pack is currently under development - it is not ready for productive use yet!

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

### Create an Erlang Release

Please ensure that your OTP app contains a release definition in the `rebar.config` file:

    {relx, [
        {release,
          { <app_name>, "1.0.0" },
          [ <app_name>, sasl, runtime_tools]
        },

        {sys_config, "./config/sys.config"},
        {vm_args,    "./config/vm.args"},

        {dev_mode, false},
        {include_erts, true}
      ]
    }.



### Create a Procfile for CF to run

Erlang releases are run via their application name, so in the root directory of your repo, ensure that there is a Procfile containing the following:

    web: _build/default/rel/<app_name>/bin/<app_name> start



### Build your CF App

    $ cf push <app name> -b https://github.com/ChrisWhealy/cf-buildpack-erlang

