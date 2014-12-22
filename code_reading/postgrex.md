# Postgrex

[Postgrex](https://github.com/ericmj/postgrex)

## `Postgrex.Connection`

### `start_link/1`

PGUSER, PGPASSWORDなどの環境変数を読んで`GenServer.start_link`。
その後`GenServer.call({:connect, opts}, timeout)`。

### `init([])`

引き回す状態はmap.

|name               |type              |description                      |
|:------------------|:-----------------|:--------------------------------|
|`sock`             |`{:gen_tcp, sock}`|TCP接続ソケット                  |
|`tail`             |`binary`          ||
|`state`            |`atom`            |`:ready, :parsing, :ssl, :auth`  |
|`parameters`       |`map`             ||
|`backend_key`      |`{pid, key}`      ||
|`rows`             |`list of tuples`  |selectの結果                     |
|`statement`        |`map`             |`%{columns: [], row_info: {}}`   |
|`portal`           |`list`            |パラメータのOIDs                 |
|`bootstrap`        |`bool`            ||
|`types`            |`atom or tuple`   |`:types_removed`, `{oids, types}`|
|`transactions`     |`integer`         |走ってるトランザクション数       |
|`queue`            |`queue`           |コマンドをキュー制御             |
|`opts`             |                  ||
|`listeners`        |`HashDict`        ||
|`listener_monitors`|`HashDict`        ||
|`lisner_pids`      |`HashDict`        ||


### `handle_call({:connect, opts}, from, %{queue: queue})`

`:gen_tcp.connect/4`で`opts`のホスト、ポートに接続。


## `Postgrex.Protocol`

### `startup/1`

postgresに認証してもらう。`{:noreply, state}`か`{:stop, :normal, %Error{}}`


## `Postgrex.Messages`


## `Postgrex.Types`


## `Postgrex.Result`

|attr      |type        |description                 |
|:---------|:-----------|:---------------------------|
|`command` |`atom`      |`:select`とか`:insert`      |
|`columns` |`[String.t]`|カラム名                    |
|`rows`    |`[tuple]`   |ResultSet                   |
|`num_rows`|`integer`   |取得した、または影響した列数|

## `Postgrex.TypeInfo`

|attr        |type           |description                               |
|:-----------|:--------------|:-----------------------------------------|
|`oid`       |`pos_integer`  |Object ID                                 |
|`sender`    |`String.t`     |psqlが型を文字表記するときに使う関数の名前|
|`type`      |`String.t`     |型の名前                                  |
|`array_elem`|`pos_integer`  |typeが配列の場合、その要素のOID           |
|`comp_elems`|`[pos_integer]`|typeが複合型の場合、その要素のOID         |
