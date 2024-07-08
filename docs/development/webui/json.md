['Development']
# Webui JSON api

For a demo: set the webui-template to "ajax_demo".

## Full client api
* url : /json/rpc
* rpc-api : http://en.wikipedia.org/wiki/JSON-RPC#Version_1.0
* methods : http://dev.deluge-torrent.org/wiki/Development/UiClient#Remoteapi
  
## added methods, json only

### update_ui(keys ,filter_dict , cache_id = None )
Composite call.
Goal : limit the number of ajax calls

input:

```
    keys: see get_torrent_status
    filter_dict: see label_get_filtered_ids # only effective if the label plugin is enabled.
    cache_id: # todo
```
returns:

```
{
    "torrents": see get_torrent_status
    "filters": see label_get_filters
    "stats": see get_stats
    "cache_id":int # todo
}
```

### get_stats()
returns:

```
{
'download_rate':float(),
'upload_rate':float(),
'max_download':float(),
'max_upload':float(),
'num_connections':int(),
'max_num_connections':int(),
'dht_nodes',int()
}
```

### get_webui_config
returns: a dict with the current webui config (excluding "pwd_*" fields)

### set_webui_config(config_dict)
input: a dict with the webui config-values  (extra possible key:"pwd")

### get_webui_templates()
returns: a list of strings.

### download_torrent_from_url(url)
input:

```
    url: the url of the torrent to download
```
returns:

```
    filename: the temporary file name of the torrent file
```

### get_torrent_info(filename)
Goal:
    allow the webui to retrieve data about the torrent

input:

```
    filename: the filename of the torrent to gather info about
```

returns:

```
{
    "filename": the torrent file
    "name": the torrent name
    "size": the total size of the torrent
    "files": the files the torrent contains
    "info_hash" the torrents info_hash
}
```

### add_torrents(torrents)
input:

```
    torrents [{
        path: the path of the torrent file,
        options: the torrent options
    }]
```