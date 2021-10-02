![Flight Deck](https://raw.githubusercontent.com/ten7/flight-deck/master/flightdeck-logo.png)

# Flight Deck Memcache

Flight Deck Memcache is a minimalist memcache container for Drupal sites on Kubernetes and Docker. You can use it both for local development and production.

Features:
* ConfigMap-friendly YAML configuration
* Supports multiple cores
* Supports custom cores via ConfigMaps or volumes

## Tags and versions

There are several tags available for this container, each with different memcache and Drupal module support:

| Tags | Memcache Version | Search API | Custom cores |
| ---- | ------------ | ---------- | ------------ |
| latest | 1.6 | Yes | Yes |
| x.y.z | 1.6 | Yes | Yes |


## Configuration

Instead of a large number of environment variables, this container relies on a file to perform all runtime configuration, `flightdeck-memcache.yml`. Inside the file, create following:

```yaml
---
flightdeck_memcache: {}
```

All configuration is done as items under the `flightdeck_memcache` variable. See the following sections for details as to particular configurations.

You can provide this file in one of three ways to the container:

* Mount the configuration file at path `/config/memcache/flightdeckmemcache.yml` inside the container using a bind mount, configmap, or secret.
* Mount the config file anywhere in the container, and set the `FLIGHTDECK_CONFIG_FILE` environment variable to the path of the file.
* Encode the contents of `flightdeck-memcache.yml` as base64 and assign the result to the `FLIGHTDECK_CONFIG` environment variable.

### Basic settings

```yaml
---
flightdeck_memcache:
  port: "11211"
  memory: "64"
  threads: "4"
```

Where:
* **flightdeck_memcache.port** is the port on which to run memcache. Optional, defailts to `11211`.
* **flightdeck_memcache.memory** is the memory in megabytes to use for caching. Optional, defaults to `64`.
* **flightdeck_memcache.threads** is the number of threads to use to serve requests. Optional, defaults to `4`.

### External file storage

Memcache has the option to use an external file for storage. This allows you to use persistent storage in addition to memory for larger capacity. Note, this feature is experimental.

```yaml
---
flightdeck_memcache:
  port: "11211"
  memory: "64"
  threads: "4"
  file: "/path/to/file"
```

Where:
* **flightdeck_memcache.file** is the path to persistent file to use for storage. Optional. When not specified, persistent storage is not used.

## Part of Flight Deck

This container is part of the [Flight Deck library](https://github.com/ten7/flight-deck) of containers for Drupal local development and production workloads on Docker, Swarm, and Kubernetes.

Flight Deck is used and supported by [TEN7](https://ten7.com/).


## Debugging

If you need to get verbose output from the entrypoint, set `flightdeck_debug` to `true` or `yes` in the config file.

```yaml
---
flightdeck_debug: yes
```

This container uses [Ansible](https://www.ansible.com/) to perform start-up tasks. To get even more verbose output from the start up scripts, set the `ANSIBLE_VERBOSITY` environment variable to `4`.

If the container will not start due to a failure of the entrypoint, set the `FLIGHTDECK_SKIP_ENTRYPOINT` environment variable to `true` or `1`, then restart the container.

## License

Flight Deck is licensed under GPLv3. See [LICENSE](https://raw.githubusercontent.com/ten7/flight-deck/master/LICENSE) for the complete language.
