otp.mk
======

Tiny Makefile-based Erlang/OTP and reltool/relx/rebar compatible build solution.

Prerequisites in PATH
---------------------

    make
    relx
    rebar
    to_erl
    run_erl

Commands
--------

    make get-deps	Get-Deps from rebar.config
    make [compile]	Compile Dependencies with rebar
    make .applist	Generate Applications List
    make clean		Clean BEAM Files
    make console	Run Apps in Dev Mode Console
    make start		Start bundle with run_erl in Dev Mode
    make attach		Attach to bundle with to_erl in Dev Mode
    make release	Build Release with relx

See real example of usage in https://github.com/5HT/skyline

TODO
----

1. Raw erlc support from rebar.config
2. Raw git clone support from rebar config

Credits
-------

Vladimir Kirillov
Maxim Sokhatsky

OM A HUM
