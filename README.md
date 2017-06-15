eNiXHosting.influxdb
=================

A role for deploying and configuring [InfluxDB](http://www.influxdata.com) and extensions on unix hosts using [Ansible](http://www.ansible.com/).


Requirements
------------

Supported targets:

- Ubuntu 16.04 "Xenial"
- Debian 7 "Wheezy"
- Debian 8 "Jessie"


Role Variables
--------------

This roles comes preloaded with almost every available default. You can override each one in your hosts/group vars, in your inventory, or in your play. See the annotated defaults in `defaults/main.yml` for help in configuration. All provided variables start with `influxdb__`.

```yaml
influxdb__base_dir: "/var/lib/influxdb"

# config
influxdb__reporting_disabled: "false"
# define only if needed
#influxdb__hostname:

# [meta]
influxdb__meta_dir: "{{ influxdb__base_dir }}/meta"
influxdb__retention_autocreate: "true"
influxdb__meta_logging_enabled: "true"

# [data]
influxdb__data_dir: "{{ influxdb__base_dir }}/data"
influxdb__wal_dir: "{{ influxdb__base_dir }}/wal"
influxdb__data_trace_logging_enabled: "false"
influxdb__data_query_log_enabled: "true"
influxdb__data_cache_max_memory_size: 1048576000
influxdb__data_cache_snapshot_memory_size: 26214400
influxdb__data_cache_snapshot_write_cold_duration: "10m"
influxdb__data_compact_full_write_cold_duration: "4h"
influxdb__data_max_series_per_database: 1000000
influxdb__data_max_values_per_tag: 100000

# [coordinator]
influxdb__coordinator_write_timeout: "10s"
influxdb__coordinator_max_concurrent_queries: 0
influxdb__coordinator_query_timeout: "0s"
influxdb__coordinator_log_queries_after: "0s"
influxdb__coordinator_max_select_point: 0
influxdb__coordinator_max_select_series: 0
influxdb__coordinator_max_select_buckets: 0

# [retention]
influxdb__retention_enabled: "true"
influxdb__retention_check_interval: "30m"

# [shard-precreation]
influxdb__shard_creation_enabled: "true"
influxdb__shard_creation_check_interval: "10m"
influxdb__shard_creation_advance_period: "30m"

# [monitor]
influxdb__monitor_store_enabled: "true"
influxdb__monitor_store_database: "_internal"
influxdb__monitor_store_interval: "10s"

# [admin]
influxdb__admin_enabled: "false"
influxdb__admin_bind_address: ":8083"
influxdb__admin_https_enabled: "false"
influxdb__admin_https_certificate: "/etc/ssl/influxdb.pem"

# [http]
influxdb__http_enabled: "true"
influxdb__http_bind_address: ":8086"
influxdb__http_auth_enabled: "false"
influxdb__http_realm: "InfluxDB"
influxdb__http_log_enabled: "true"
influxdb__http_write_tracing: "false"
influxdb__http_pprof_enabled: "true"
influxdb__http_https_enabled: "false"
influxdb__http_https_certificate: "/etc/ssl/influxdb.pem"
influxdb__http_https_private_key: ""
influxdb__http_shared_sercret: ""
influxdb__http_max_row_limit: 10000
influxdb__http_max_connection_limit: 0
influxdb__http_unix_socket_enabled: "false"
influxdb__http_bind_socket: "/var/run/influxdb.sock"

# [subscriber]
influxdb__subscriber_enabled: "true"
influxdb__subscriber_http_timeout: "30s"
influxdb__subscriber_insecure_skip_verify: "false"
influxdb__subscriber_ca_certs: ""
influxdb__subscriber_write_concurrency: 40
influxdb__subscriber_write_buffer_size: 1000

# [[graphite]]
influxdb__graphite_enabled: "false"
influxdb__graphite_database: "graphite"
influxdb__graphite_retention_policy: ""
influxdb__graphite_bind_address: ":2003"
influxdb__graphite_protocol: "tcp"
influxdb__graphite_consistency_level: "one"
influxdb__graphite_batch_size: 5000
influxdb__graphite_batch_pending: 5
influxdb__graphite_batch_timeout: "1s"
influxdb__graphite_udp_read_buffer: 0
influxdb__graphite_separator: "."
#influxdb__graphite_tags: [ "region=us-east", "zone=1c" ]
influxdb__graphite_templates:
  - "type.host.measurement.device"

# [[collectd]]
influxdb__collectd_enabled: "false"
influxdb__collectd_bind_address: ":25826"
influxdb__collectd_database: "collectd"
influxdb__collectd_retention_policy: ""
influxdb__collectd_typesdb: "/usr/share/collectd/types.db"
influxdb__collectd_security_level: "none"
influxdb__collectd_auth_file: "/etc/collectd/auth_file"
influxdb__collectd_batch_size: 5000
influxdb__collectd_batch_pending: 10
influxdb__collectd_batch_timeout: "10s"
influxdb__collectd_read_buffer: 0

# [[opentsdb]]
influxdb__tsb_enabled: "false"
influxdb__tsb_bind_address: ":4242"
influxdb__tsb_database: "opentsdb"
influxdb__tsb_retention_policy: ""
influxdb__tsb_consistency_level: "one"
influxdb__tsb_tls_enabled: "false"
influxdb__tsb_certificate: "/etc/ssl/influxdb.pem"
influxdb__tsb_log_point_errors: "true"
influxdb__tsb_batch_size: 1000
influxdb__tsb_batch_pending: 5
influxdb__tsb_batch_timeout: "1s"

# [[udp]]
influxdb__udp_enabled: "false"
influxdb__udp_bind_address: ":8089"
influxdb__udp_database: "udp"
influxdb__udp_retention_policy: ""
influxdb__udp_batch_size: 5000
influxdb__udp_batch_pending: 10
influxdb__udp_batch_timeout: "1s"
influxdb__udp_read_buffer: 0

# [continuous_queries]
influxdb__cqueries_enabled: "true"
influxdb__cqueries_log_enabled: "true"
influxdb__cqueries_run_interval: "1s"

```

Dependencies
------------

- None

Usage
-----

Clone this repo into your roles directory:

    $ git clone https://gitlab.enix.org/ansible/ansible-influxdb.git roles/influxdb

Or use Ansible galaxy requirements.yml

    # influxdb from eNiXHosting
    - src: eNiXHosting.influxdb
      name: influxdb

And add it to your play's roles:

    - hosts: all
      roles:
        - role influxdb:
            influxdb__var: true

You can also use the role as a playbook. You will be asked which hosts to provision, and you can further configure the play by using `--extra-vars`.

    $ ansible-playbook -i inventory --extra-vars='{...}' main.yml

Still to do
-----------

- Add SSL certificates Support
- Add collectd input configuration
- Add credentials configuration / https://docs.influxdata.com/influxdb/v1.2/query_language/authentication_and_authorization
- Add RedHat installation


Changelog
---------

### 1.0

Initial version.

License
-------

GPLv2

Author Information
------------------

Laurent Corbes <laurent.corbes@enix.fr> - http://www.enix.fr
