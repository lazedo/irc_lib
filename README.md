Irc client library for erlang
==============================

irc_lib - is irc client library for erlang application. It provides simple functional for communicating with irc servers.

[![Build Status](https://travis-ci.org/0xAX/irc_lib.png)](https://travis-ci.org/0xAX/irc_lib)

Features
========

  * Simple connect to irc channel
  * Callbacks
  * SSL support
  * Reconnect ability
  * Multichannel support
  * Dependency free

Usage
======

First of all compile irc lib:

```
./rebar compile
```

Start root supervisor in your project or application:

```erlang
irc_lib_sup:start_link().
```

Now create callback module which will be run and receive incoming irc messages:

```erlang
-module(irc_test).

-behavior(gen_server).

-export([start_link/0]).
 
%% gen_server callbacks
-export([init/1,
         handle_call/3,
         handle_cast/2,
         handle_info/2,
         terminate/2,
         code_change/3]).
 
% state
-record(state, {}).

start_link() ->
    gen_server:start_link({local, ?MODULE}, ?MODULE, [], []).

init([]) ->
	irc_lib_sup:start_link(),
	irc_lib_sup:start_irc_client(?MODULE, <<"irc.freenode.net">>, 5667, false, [{<<"#erlang">>, ""}, {<<"#WeberMVC">>, ""}] <<"some-user-name">>, false, 1000),
    {ok, #state{}}.
 
handle_call(_Request, _From, State) ->
    {reply, ignored, State}.

handle_cast(_Msg, State) ->
    {noreply, State}.

handle_info({incoming_message, From, IncomingMessage}, State) ->
	io:format("Incoming message: ~p ~p ~n", [IncomingMessage, From]),
	{noreply, State};

handle_info(_Info, State) ->
    {noreply, State}.

terminate(_Reason, _State) ->
    ok.

code_change(_OldVsn, State, _Extra) ->
    {ok, State}.
 
%% Internal functions
```

Also sending message to irc channel:

```erlang
irc_lib_client:send_message(Pid, <<"Hello erlangers">>).
```

Where Pid - is pid of irc client process.

## Contribute

  * Fork `irc_lib` repo
  * Make changes
  * Pull request
  * Thank you.

Authors
========

[@0xAX](https://twitter.com/anotherworldofw)
