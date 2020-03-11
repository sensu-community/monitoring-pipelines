# Sensu Monitoring Pipelines

This project contains a number of Sensu monitoring pipeline configurations to
bootstrap effective monitoring with [Sensu Go][0]. Although pipelines are
intended to be a starting point for Sensu Go users, some minor per-deployment
modifications are to be expected (e.g. configuration of a [secrets
provider][1]).

[0]: https://sensu.io
[1]: https://docs.sensu.io/sensu-go/latest/guides/secrets-management/

## Goal

The purpose of this project is to provide portable configuration for a more
"turn-key" experience with Sensu Go. Users should be able to add individual
pipelines or the entire collection of pipelines using `sensuctl`.

### Examples

Add an individual pipeline via `sensuctl`:

```
$ sensuctl create -f https://raw.githubusercontent.com/sensu-community/monitoring-pipelines/master/metrics/influxdb.yml
```

Add the entire collection of pipelines:

```
$ git clone git@github.com:sensu-community/monitoring-pipelines.git
$ sensuctl create -f $(find ./monitoring-pipelines -name *.yml)
```

## Rules

1. Every Sensu user must be able to safely create all resources as
   defined in this project (from the project root).

2. All resource definitions must be written in YAML for consistency
   and comment support.

3. Handler, Filter, and Mutator resource names must be unique within the scope
   of this project.

   _NOTE: at this time we do not wish to enforce strict naming conventions. We
   will resolve naming conflicts on a case-by-case basis, which means resource
   names will be subject to change._

4. Resource definitions should NOT include a namespace.

5. All Assets used must be registered and hosted on
   [Bonsai](https://bonsai.sensu.io).

6. Asset resources must include a version reference in their name
   (e.g. sensu/sensu-email-handler:0.4.1).

## Contributing

For guidelines on how to contribute to this project and information
about what we require from project contributors, please see
[CONTRIBUTING.md](CONTRIBUTING.md).
