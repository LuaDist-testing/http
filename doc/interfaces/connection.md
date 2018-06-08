## connection

lua-http has separate libraries for both HTTP 1 and HTTP 2 type communications. Future protocols will also be supported and exposed as new modules. As HTTP 1 and 2 share common concepts at the connection and stream level, the _[connection](#connection)_ and _[stream](#stream)_ modules have been written to contain common interfaces wherever possible. All _[connection](#connection)_ types expose the following fields:


### `connection.type` <!-- --> {#connection.type}

The mode of use for the connection object. Valid values are:

  - `"client"` - Connects to a remote URI
  - `"server"` - Listens for connection on a local URI


### `connection.version` <!-- --> {#connection.version}

The HTTP version number of the connection as a number.


### `connection:connect(timeout)` <!-- --> {#connection:connect}

Completes the connection to the remote server using the address specified, HTTP version and any options specified in the `connection.new` constructor. The `connect` function will yield until the connection attempt finishes (success or failure) or until `timeout` is exceeded. Connecting may include DNS lookups, TLS negotiation and HTTP2 settings exchange. Returns `true` on success. On error, returns `nil`, an error message and an error number.


### `connection:checktls()` <!-- --> {#connection:checktls}

Checks the socket for a valid Transport Layer Security connection. Returns the luaossl ssl object if the connection is secured. Returns `nil` and an error message if there is no active TLS session. Please see the [luaossl website](http://25thandclement.com/~william/projects/luaossl.html) for more information about the ssl object.


### `connection:localname()` <!-- --> {#connection:localname}

Returns the connection information for the local socket. Returns address family, IP address and port for an external socket. For Unix domain sockets, the function returns `AF_UNIX` and the path. If the connection object is not connected, returns `AF_UNSPEC` (0). On error, returns `nil`, an error message and an error number.


### `connection:peername()` <!-- --> {#connection:peername}

Returns the connection information for the socket *peer* (as in, the next hop). Returns address family, IP address and port for an external socket. For unix sockets, the function returns `AF_UNIX` and the path. If the connection object is not connected, returns `AF_UNSPEC` (0). On error, returns `nil`, an error message and an error number.

*Note: If the client is using a proxy, the values returned `:peername()` point to the proxy, not the remote server.*


### `connection:flush(timeout)` <!-- --> {#connection:flush}

Flushes all buffered outgoing data on the socket. Returns `true` on success. Returns `false` and the error if the socket fails to flush.


### `connection:shutdown()` <!-- --> {#connection:shutdown}

Performs an orderly shutdown of the connection by closing all streams and calls `:shutdown()` on the socket. The connection cannot be re-opened.


### `connection:close()` <!-- --> {#connection:close}

Closes a connection and releases operating systems resources. Note that `:close()` performs a [`connection:shutdown()`](#connection:shutdown) prior to releasing resources.


### `connection:new_stream()` <!-- --> {#connection:new_stream}

Creates and returns a new [*stream*](#stream) on the connection.


### `connection:get_next_incoming_stream(timeout)` <!-- --> {#connection:get_next_incoming_stream}

Returns the next peer initiated [*stream*](#stream) on the connection. This function can be used to yield and "listen" for incoming HTTP streams.


### `connection:onidle(new_handler)` <!-- --> {#http.connection:onidle}

Provide a callback to get called when the connection becomes idle i.e. when there is no request in progress and no pipelined streams waiting. When called it will receive the `connection` as the first argument. Returns the previous handler.
