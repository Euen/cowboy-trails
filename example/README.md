# Cowboy Trails Example
===

To try this example, you need GNU `make` , `git` and `Erlang` in your PATH.

```
  make deps
```

To start the example server in a interactive way do this:
```
  make app shell
```

Example outputs
---------------

If you put this url in your browser, you will see a beatiful picture :).
```
http://localhost:8080/index.html
```

## Echo example

You can set a response on the server with `PUT http://localhost:8080/message/echo`.

```
curl -v -H "Content-Type: text/plain" -X PUT http://localhost:8080/message/yahooooo!!!
< HTTP/1.1 200 OK
< connection: keep-alive
* Server Cowboy is not blacklisted
< server: Cowboy
< date: Wed, 01 Jul 2015 18:27:22 GMT
< content-length: 25
< content-type: text/plain
<
* Connection #0 to host localhost left intact
You put an echo! yahooooo!⏎
```

## Description example

if you run this `curl` command, you will see all trails description of this server.

```
curl -i http://localhost:8080/description

[#{constraints => [],
   handler => cowboy_static,
   metadata => #{get => #{desc => "Static Data"}},
   options => {priv_dir,example,[],[{mimetypes,cow_mimetypes,all}]},
   path_match => <<"/[...]">>},
 #{constraints => [],
   handler => example_echo_handler,
   metadata => #{get => #{desc => "Gets echo var in the server"},
     put => #{desc => "Sets echo var in the server"}},
   options => [],
   path_match => <<"/message/[:echo]">>},
 #{constraints => [],
   handler => example_description_handler,
   metadata => #{get => #{desc => "Retrives trails's server description"}},
   options => [],
   path_match => <<"/description">>}]
```

## Dumb Key-Value Store

There is an  additional endpoint that implement a really simple key-value store
that uses the application's env variable as its backend. It is possible to `PUT`,
`GET` and `DELETE` values in the following way:

```
$ curl -XPUT "localhost:8080/dumb-kv/some-key/some-value" -H "content-type: text/plain"

some-value

$ curl -XGET "localhost:8080/dumb-kv/some-key" -H "content-type: text/plain"

some-value

$ curl -v -XGET "localhost:8080/dumb-kv/non-existing-key" -H "content-type: text/plain"

...
< HTTP/1.1 404 Not Found
...

$ curl -v -XDELETE "localhost:8080/dumb-kv/some-key" -H "content-type: text/plain"

...
< HTTP/1.1 204 No Content
...

$ curl -v -XDELETE "localhost:8080/dumb-kv/some-key" -H "content-type: text/plain"

...
< HTTP/1.1 404 Not Found
...
```
