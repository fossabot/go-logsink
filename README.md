# go-logsink

[![Go Report Card](https://goreportcard.com/badge/github.com/sascha-andres/go-logsink)](https://goreportcard.com/report/github.com/sascha-andres/go-logsink) [![codebeat badge](https://codebeat.co/badges/6e2d5bf5-5ca2-41a3-842d-631ba32d196c)](https://codebeat.co/projects/github-com-sascha-andres-go-logsink) [![Code Climate](https://codeclimate.com/github/sascha-andres/go-logsink/badges/gpa.svg)](https://codeclimate.com/github/sascha-andres/go-logsink)
[![FOSSA Status](https://app.fossa.io/api/projects/git%2Bgithub.com%2Fsascha-andres%2Fgo-logsink.svg?type=shield)](https://app.fossa.io/projects/git%2Bgithub.com%2Fsascha-andres%2Fgo-logsink?ref=badge_shield)

## What is go-logsink

go-logsink is a client and server application to aggregate multiple streams into one.

It does so by sending all data piped into the client to the server.

## Usage

To start a server just type `go-logsink listen`. By default the server listens on port 50051. You can change the default binding definition using the `bind` flag:

    go-logsink listen --bind ":55555"

In this sample, the server would listen on port 55555.

To send data to the server you have to start at least one client. For example to send the syslog to the server:

    sudo tail -f /var/log/syslog | go-logsink connect --address "localhost:55555"

Using the `address` flag it is possible to send data to the non default destination (`localhost:50051`)

An advanced usage would be to forward all logs from running docker containers:

    for cont in `docker ps -q`; do
        docker logs -f $cont | go-logsink connect &
    done

This assumes a running go-logsink server at localhost:50051

You can even start a webserver using

    go-logsink web

More usage: See [go-logsink documentation](docs/go-logsink.md)

## Develop

The applicatin uses a very simple protocol buffer based RPC service to communicate. You should install Protocol Buffers v3.1.0
from https://github.com/google/protobuf/releases/tag/v3.1.0 ( used to create this project initially ).

The Makefile contains a `protobuf` target which creates the implementation from the specification ( located in `logsink/logsink.proto` ).

The project uses govendor ( https://github.com/kardianos/govendor ) to populate the vedor directory. Use `govendor sync` to download the libraries. This can take quite a while.

### Cross compiling Windows

current state: untested binary

You have to `go get github.com/inconshreveable/mousetrap` before you can crosscompile.

### OSX

You can crosscompile OSX or download the darwin.tgz from the releases.
Rudimentary tests were done, no in depth testing, though

## History

|Version|Description|
|---|---|
|v1.1.0|Web included in binary|
||Deprecated relaying|
||Added prometheus metrics|
|v1.0.2|Graceful shutdown|
|v1.0.1|go-ps|
|v1.0.0|New versioning scheme|
||Lockfile support|
||go-releaser|
||Added .editorconfig|
|20170124|Forcing display space usage|
||Allow toggling of scroll|
||Add a `--break` switch to connect|
||Enabling CORS|
||Supporting SSL behind a proxy|
||Added `--pass-through` switch on connect to pass text to stdout|
||Allow changing the line limit in web|
||Breaking on web for pre element|
||Filtering in web|
|20170117|Web endpoint|
||doc command|
||version command|
||Allow providing a prefix|
|20170110|Initial version|


## License
[![FOSSA Status](https://app.fossa.io/api/projects/git%2Bgithub.com%2Fsascha-andres%2Fgo-logsink.svg?type=large)](https://app.fossa.io/projects/git%2Bgithub.com%2Fsascha-andres%2Fgo-logsink?ref=badge_large)