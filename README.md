otp.mk
======

Tiny Makefile-based Erlang/OTP and reltool/relx/rebar/mix compatible build solution.

Prerequisites in PATH
---------------------

    make
    relx
    rebar
    to_erl
    run_erl

Commands
--------

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

See real example of usage in https://github.com/5HT/skyline

TODO
----

1. mix support
2. rebar.config compatible rebar replacement

Credits
-------

* Vladimir Kirillov
* Maxim Sokhatsky
* Max Treskin

OM A HUM
