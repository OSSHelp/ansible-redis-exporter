# redis-exporter

[![Build Status](https://drone.osshelp.ru/api/badges/ansible/redis-exporter/status.svg)](https://drone.osshelp.ru/ansible/redis-exporter)

Ansible role that installs and configures [redis-exporter](https://github.com/oliver006/redis_exporter).

## Usage (example)

Minimal:

```yaml
    - role: redis-exporter
```

Configure only:

```yaml
    - role: redis-exporter
      redis_exporter_setup: configure
      redis_exporter_params:
        addr: "redis://localhost:6379"
        password: "{{ pass_from_vault }}"
```

## Available parameters

### Version control

| Param | Description |
| -------- | -------- |
| redis_exporter_version | Which exporter version to install. The value will be used in binary download url formation. Available releases could be found [here](https://github.com/oliver006/redis_exporter/releases). |

### redis_exporter_params

Global settings and defaults.

| Param | Description |
| -------- | -------- |
| `redis_exporter_setup` | Setup mode. See [OSSHelp KB article](https://oss.help/kb4895) |
| addr | Address of the Redis instance, defaults to `redis://localhost:6379`. |
| password | Password of the Redis instance, defaults to `""` (no password). |
| exporter_check_keys | Comma separated list of key patterns to export value and length/size, eg: `db3=user_count` will export key `user_count` from db `3`. db defaults to `0` if omitted. The key patterns specified with this flag will be found using [SCAN](https://redis.io/commands/scan).  Use this option if you need glob pattern matching; `check-single-keys` is faster for non-pattern keys. Warning: using `--check-keys` to match a very large number of keys can slow down the exporter to the point where it doesn't finish scraping the redis instance. |
| check_single_keys | Comma separated list of keys to export value and length/size, eg: `db3=user_count` will export key `user_count` from db `3`. db defaults to `0` if omitted.  The keys specified with this flag will be looked up directly without any glob pattern matching.  Use this option if you don't need glob pattern matching;  it is faster than `check-keys`. |
| script | Path to Redis Lua script for gathering extra metrics. |
| debug | Verbose debug output. |
| log_format | Log format, valid options are `txt` (default) and `json`. |
| namespace | Namespace for the metrics, defaults to `redis`. |
| connection_timeout | Timeout for connection to Redis instance, defaults to "15s" (in Golang duration format). |
| web_listen_address | Address to listen on for web interface and telemetry, defaults to `0.0.0.0:9121`. |
| web_telemetry_path | Path under which to expose metrics, defaults to `/metrics`. |
| redis_only_metrics | Whether to also export go runtime metrics, defaults to false. |
| incl_system_metrics | Whether to include system metrics like `total_system_memory_bytes`, defaults to true. |
| ping_on_connect | Whether to ping the redis instance after connecting and record the duration as a metric, defaults to true. |
| is_tile38 | Whether to scrape Tile38 specific metrics, defaults to false. |
| export_client_list | Whether to scrape Client List specific metrics, defaults to false. |
| skip_tls_verification | Whether to skip TLS verification. |
| tls_client_key_file | Name of the client key file (including full path) if the server requires TLS client authentication. |
| tls_client_cert_file | Name the client cert file (including full path) if the server requires TLS client authentication. |
| set_client_name | Whether to set client name to redis_exporter, defaults to true. |

### Auxiliary params

Do not change any of these without a clear understanding of the goal you want to achieve. Any mistake in them will completely break the role.

| Param | Description |
| -------- | -------- |
| redis_exporter_targz_url | Url for binary archive download. |
| redis_exporter_targz_sums | Url to sha256sums. |
| redis_exporter_config_file | Name of the configuration file. |
| redis_exporter_user_name | User and group, that will be created for the exporter. |
| redis_exporter_service_name | Name of the systemd service that will be created. |
| redis_exporter_systemd_unit | Absolute path to systemd unit. |
| redis_exporter_dirs.binary_dir | Directory to use for binary placement. |
| redis_exporter_dirs.config_dir | Directory to use for configuration file placement. |

## FAQ

### I need to override only one parameter from redis_exporter_params

You can do it! There's no need to describe the whole list of redis_exporter_params in playbook. Default values will be used for missing params for cfg-file generation.

## Useful links

- [Official documentation](https://github.com/oliver006/redis_exporter/blob/master/README.md)
- [Available releases](https://github.com/oliver006/redis_exporter/releases)
- [Our article about Redis memory usage and eviction policies](https://oss.help/kb2344)

## TODO

- Move default parameter to redis_exporter_default_params, merge with redis_exporter_params, and remove default values from templates

## License

GPL3

## Author

OSSHelp Team, see <https://oss.help>
