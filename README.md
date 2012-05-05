# Erlang Inotifywait Wrapper

A simple wrapper around Linux *inotify* using `inotifywait` from *inotify-tools*.

Wrappping is done via a port spawning an external `inotifywait` command in
monitor mode. You simply listen to messages from the wrapper to get inotify
events. You can also start an event manager and use a gen\_event behaviour
to subscribe to events.

The module should work on any box where `inotifywait` and a standard posix `sh`
shell are available.

## Flow

`start` spawns a *wrapper* erlang process which will check passed options
and then spawns a *inotifywait* external process through a plain erlang port
(using `erlang:open_port/2` with the `spawn` option). The *wrapper* process
receives *inotifywait* standard output as messages from the port, parses them
and sends them forward as structured messages to the process that invoked
`start` in the first place.

## Why not a custom port written in C or a port driver?

Because I wanted to experiment with the idea of using a plain port to an external
process and wanted to keep the code small. Besides there already exists an erlang
inotify module built around the inotify C API (for instance 
[there](https://github.com/massemanet/inotify))