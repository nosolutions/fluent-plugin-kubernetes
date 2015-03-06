# fluent-plugin-kubernetes, a plugin for [Fluentd](http://fluentd.org) 

## Requirements

Requires [fluent-plugin-docker-format](https://github.com/bwalex/fluent-plugin-docker-format)
plugin.

## Installation

    gem install fluent-plugin-kubernetes

## Configuration
```
<source>
  type tail
  path /var/lib/docker/containers/*/*-json.log
  pos_file fluentd-docker.pos
  time_format %Y-%m-%dT%H:%M:%S
  tag docker.*
  format json
</source>

<match docker.var.lib.docker.containers.*.*.log>
  type docker_format
  container_id ${tag_parts[5]}
  tag docker.${name}
</match>

<match docker.**>
  type kubernetes
</match>

<match kubernetes>
  type stdout
</match>
```

### Assuming following inputs are coming:
    test.aa: {"container_name":"k8s_CONTAINER.2f44475a_POD.NAMESPACE.api_ae0aeb72-c44f-11e4-a274-54ee7527188d_d442134f", "json":"dayo"}
### then output bocomes as belows
    test.aa: {"container_name":"k8s_CONTAINER.2f44475a_POD.NAMESPACE.api_ae0aeb72-c44f-11e4-a274-54ee7527188d_d442134f", "pod":"POD", "pod_namespace":"NAMESPACE", "pod_container":"CONTAINER", "json":"dayo", "hoge":"moge","hogehoge":"mogemoge"}

## Contributing

1. Fork it
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request

## Copyright
    Copyright (c) 2015 jimmidyson