## http.hsts

Data structures useful for HSTS (HTTP Strict Transport Security)

### `new_store()` <!-- --> {#http.hsts.new_store}

Creates and returns a new HSTS store.


### `hsts_store:clone()` <!-- --> {#http.hsts:clone}

Creates and returns a copy of a store.


### `hsts_store:store(host, directives)` <!-- --> {#http.hsts:store}

Add new directives to the store about the given `host`. `directives` should be a table of directives, which *must* include the key `"max-age"`.


### `hsts_store:check(host)` <!-- --> {#http.hsts:check}

Returns a boolean indicating if the given `host` is a known HSTS host.


### `hsts_store:clean()` <!-- --> {#http.hsts:clean}

Removes expired entries from the store.
