# graylog
Simple Graylog CLI for viewing and tailing messages.

Originally came from https://github.com/bvargo/gtail.
I converted it to Python3 and added some features.

```
usage: graylog [-h] [--list-streams] [--query QUERY [QUERY ...]]
               [--limit LIMIT] [--stream STREAM_NAMES [STREAM_NAMES ...]]
               [--tail] [--config CONFIG_PATHS [CONFIG_PATHS ...]]
               [--range TIME_RANGE] [--absolute ABSOLUTE ABSOLUTE]

Tail logs from Graylog.

optional arguments:
  -h, --help            show this help message and exit
  --list-streams        List streams and exit.
  --query QUERY [QUERY ...], -q QUERY [QUERY ...]
                        Query terms to search on. Either query or stream needs
                        to be provided.
  --limit LIMIT, -l LIMIT
                        The maximum number of messages to display.
  --stream STREAM_NAMES [STREAM_NAMES ...], -s STREAM_NAMES [STREAM_NAMES ...]
                        The name of the streams to display. Default: all
                        streams.
  --tail, -t            Whether to tail the output.
  --config CONFIG_PATHS [CONFIG_PATHS ...], -c CONFIG_PATHS [CONFIG_PATHS ...]
                        Config files. Default: .graylog,
                        /Users/ctwise/.graylog
  --range TIME_RANGE, -r TIME_RANGE
                        Time range to search backwards relative to now
                        (defaults to seconds). Examples: 30m, 2h, 4d
  --absolute ABSOLUTE ABSOLUTE, -a ABSOLUTE ABSOLUTE
                        Absolute time range to search. Provide FROM and TO in
                        the format yyyy-MM-ddTHH:mm:ss.SSSZ (e.g.
                        2019-04-23T20:44:36.694Z)

Example configuration file:

[server]
; Graylog REST API
uri: http://graylog.example.com:12900
; optional username and password
username: USERNAME
password: PASSWORD
```
