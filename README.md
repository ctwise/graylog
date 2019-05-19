# graylog
Simple Graylog CLI for viewing and tailing messages.

Originally came from https://github.com/bvargo/gtail.
I converted it to Python3 and added some features.

```
usage: graylog [-h] [--list-streams] [--application APPLICATION]
               [--query QUERY [QUERY ...]] [--export FIELDS [FIELDS ...]]
               [--limit LIMIT] [--stream STREAM_NAMES [STREAM_NAMES ...]]
               [--tail] [--config CONFIG_PATHS [CONFIG_PATHS ...]]
               [--range TIME_RANGE] [--absolute ABSOLUTE ABSOLUTE]

Tail logs from Graylog.

optional arguments:
  -h, --help            show this help message and exit
  --list-streams        List streams and exit.
  --application APPLICATION, -a APPLICATION
                        Shorthand to specify a query for an application, e.g.,
                        -a ms-email is equivalent to -q 'application:ms-
                        email'.
  --query QUERY [QUERY ...], -q QUERY [QUERY ...]
                        Query terms to search on. Either query or stream needs
                        to be provided.
  --export FIELDS [FIELDS ...], -e FIELDS [FIELDS ...]
                        Export specified fields as CSV into a file named
                        export.csv. Requires --absolute option.
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
  --absolute ABSOLUTE ABSOLUTE
                        Absolute time range to search. Provide FROM and TO in
                        the format 'yyyy-MM-dd HH:mm:ss' (e.g. '2019-05-19
                        09:57:44')
  --json, -j            Output messages in raw/json format.

Example configuration file:

[server]
; Graylog REST API
uri: https://my-graylog:9000/api
; optional username and password
username: USERNAME
password: PASSWORD
[formats]
; log formats (list them most specific to least specific, they will be tried in order)
; all fields must be present or the format won't be applied
;
; access log w/bytes
format1: <{source}> {client_ip} {ident} {auth} [{apache_timestamp}] "{method} {request_page} HTTP/{http_version}" {server_response} {bytes}
; access log w/o bytes
format2: <{source}> {client_ip} {ident} {auth} [{apache_timestamp}] "{method} {request_page} HTTP/{http_version}" {server_response}
; java log entry
format3: <{source}> {_long_time_timestamp} {_level_color}{loglevel!s:5.5}{_reset} {_short_classname!s:20.20} : {_message_text}
; syslog
format4: <{source}> {_long_time_timestamp} {_level_color}{loglevel!s:5.5}{_reset} [{facility}] : {_message_text}
; generic entry with a loglevel
format5: <{source}> {_long_time_timestamp} {_level_color}{loglevel!s:5.5}{_reset} : {_message_text}

This file should be located at any of the following paths: .graylog, /Users/ctwise/.graylog.
```

Requires the package python-dateutil, e.g., pip3 install python-dateutil.

The formats are attempted IN ORDER. If all fields in a format exist in the message, the format is used and the message is output using that format string.

Fields that begin with an underscore (_) are computed fields. The computed fields available for use are:

- _long_time_timestamp : the message timestamp in the format: 'yyyy-mm-ddThh:mm:ss.sssZ'
- _short_classname : the final part of a Java-style classname extracted from the 'classname' field.
- _message_text : the 'message' field modified for clearer display and with additional lines taken from the
  'original_message' or 'full_message' fields.
- _level_color : the escape sequence that will output the appropriate color for the error level of the message (taken from the loglevel or level field)
- _reset : the escape sequence to return the color to normal
- _matching_streams : the streams the message belongs to (if any)
