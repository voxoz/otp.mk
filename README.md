otp.mk
======

Tiny Makefile-based Erlang/OTP and reltool/relx/rebar/mix compatible build solution.
Today otp.mk costs us 42 LOC and depman.erl 30 LOC and we want to keep that size.

Overview
--------

Everyone preffer its own Erlang build solution along with its favourite BEAM language.
We want to introduce our vision for maintaining Erlang projects. Doesn't matter you use
raw OTP reltool or rebar/relx or even Elixir mix we whant to hide implementation of those
tools behind makefile otp.mk.

Commands API
============

Frontend commands

    make get-deps		Get-Deps from rebar.config
    make update-deps	Update-Deps from rebar.config
    make [compile]		Compile Dependencies with rebar
    make .applist		Generate Applications List
    make clean			Clean BEAM Files
    make console		Run Apps in Dev Mode Console
    make start			Start bundle with run_erl in Dev Mode
    make attach			Attach to bundle with to_erl in Dev Mode
    make release		Build Release with relx
    make dialyzer		Run OTP Dializer
    make tar			Pack relx relase without ERTS
    make ct				Run Common Test suite
    make eunit			Run eunit

These commands also could be accessed via REST API in Voxoz CI LXC.

Backends
========

We cannot guarantee that underlying backends would be fixed. However we are
open to discuss best practice for resolving dependedcies, building and releasing
Erlang application bundles. One thing we should remember that our main
criteria is small size of otp.mk and clear design.

Resolving (get, update-deps, clean)
-----------------------------------

There are severals way to resolve dependencies: using rebar.config,
using mix.exs or using information based on *.app.src files.
Basic resolving neeeded for determinig correct order of
application:start(App) for launching release in developer mode.
We are using reltool_server for that purposes in depman.erl.

    rebar
    mix
    reltool

Also we need to fetch dependencies. We can do it manually by parsing rebar.config and performing git clone or
running rebar/mix. We use both rebar and mix for fetching deps in mixed Elixir/Erlang projects.

Releasing (release, tar)
------------------------

We support all ways of releasing, but for keeping simple we have chosen high-level tool relx by Eric Merritt.
In case you are using raw "rebar -f generate" or "relx" releasing in development mode you should
substitute all ebin folder with symlinks to appropriate ebin in apps/deps folder to make Rusty's
sync or Synrc active work. relx.config is generating based on APPS RELEASE NODE COOKIE information.

    relx
    rebar -f generate
    reltool

You need patching the release with relpatch.sh in order to make sync/active work.
In development mode bundles runned with "make start" or "make console" you don't need it 

Building (compile, ct, dialyzer, eunit)
---------------------------------------

Each BEAM language use its own compiler, so for Elixir we need to use mix,
for Joxa we need to use joxa and for Erlang we can use rebar or compile:file/2.
Knowing the build order we can use OTP copmiler with infromation from reltool.
Today we use rebar/mix for building. But things are going to change.

    mix
    joxa
    rebar
    compile:file/2

Controlling (start, stop, console, attach)
------------------------------------------

There are two modes you can run application bundles.

* **Development Mode** when you can inject modules into the running system,
handle code changes, staring using raw application:start/1. In that mode you can use
make start/stop/console/attach. Currently otp.mk uses the same way provided by
release tools for attaching remote nodes with pipes: to_erl/run_erl.

* **Release Mode** when you run application bundle as OTP release with
its own boot loader script usually made with relx or rebar -f generate.
If you want git pull updates for code using active/sync file system
watchers you need to patch releases with relpatch.sh. In that mode use nodetool
script generated with release manager. Currently otp.mk uses relx.

Variables
---------

    APPS        -- n2o n2o_sample cowboy erlydtl ranch gproc mimetypes
    RELEASE     -- unirel
    NODE        -- node@localhost
    COOKIE      -- secret
    ERL_ARGS    -- -args_file vm.args -config sys.config
    RUN_DIR     -- .
    LOG_DIR     -- logs
    ROOTS       -- apps deps

Prerequisites in PATH
---------------------

    joxa
    elixir
    erlc
    mix
    rebar
    make
    relx
    to_erl
    run_erl

See real example of usage in https://github.com/5HT/skyline

Credits
-------

* Vladimir Kirillov -- main author
* Maxim Sokhatsky
* Max Treskin
* Peter Bruinsma

OM A HUM
