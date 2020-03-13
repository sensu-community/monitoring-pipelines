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

## Guidelines

1. Every Sensu user must be able to safely create all resources as
   defined in this project (from the project root).

2. All resource definitions must be written in YAML for consistency
   and comment support.

3. All YAML files should use the '.yaml' extension.

4. Handler, Filter, and Mutator resource names must be unique within the scope
   of this project.

   _NOTE: at this time we do not wish to enforce strict naming conventions. We
   will resolve naming conflicts on a case-by-case basis, which means resource
   names will be subject to change._

5. Resource definitions should NOT include a namespace.

6. All Assets used must be registered and hosted on
   [Bonsai](https://bonsai.sensu.io).

7. Asset resources must include a version reference in their name
   (e.g. sensu/sensu-email-handler:0.4.1).

8. Take care to maintain secrets. If a resource makes use of a secret and the
   command supports using built-in enviornment variables for that secret,
   avoid exposing it unnecessarily with a command argument.  For example,
   the influxdb handler has arguments for the username (-u) and password (-p).
   It also supports specifying those as environment variables INFLUXDB_USER
   and INFLUXDB_PASSWORD, respectively. In this case the command should avoid
   using the arguments and instead use the environment variables.

9. Secrets should be created using the built-in env provider.

10. For alert and incident-management handlers avoid the use of filters that
    have highly subjective configuration options. By default, use the
    `is_incident` and `not_sileced` filters.

11. When defining resources, use the following order:
    * Top level resource being defined, [Handler|Mutator|Filter]
    * Secrets (if any)
    * Assets

12. Handler resources are defined under the [Handler Set][1] directories:
    * alert (general alert mechanisms such as email, slack, etc.)
    * deregistration (handlers that deregister from cloud providers,
      configuration management, etc.)
    * incident-management (PagerDuty, etc.)
    * metric-storage (metrics handlers such as InfluxDB, etc.)
    * remediation (handlers that do auto-remediation)

    **NOTE:** These are organized to line up with the handlers to be defined
    in the [monitoring-checks][2] repository.

13. Mutator and Filter resources are defined in the shared directory structure.

14. Resources defined require the following fields, even if they are blank or
    are the defaults:
    * Handler Sets
      * type
      * api_version
      * name
      * handlers
    * Pipe Handlers
      * type
      * api_version
      * name
      * command
      * filters
      * runtime_assets
      * timeout
      * env_vars
      * secrets
    * Socket Handlers
      * type
      * api_version
      * name
      * socket
      * filters
      * timeout
    * Filters
      * type
      * api_version
      * name
      * action
      * expressions
      * runtime_assets
    * Mutators
      * type
      * api_version
      * name
      * command
      * timeout
      * env_vars
      * secrets
      * runtime_assets

## Contributing

For guidelines on how to contribute to this project and information
about what we require from project contributors, please see
[CONTRIBUTING.md](CONTRIBUTING.md).

[1]: https://docs.sensu.io/sensu-go/latest/reference/handlers/#handler-sets
[2]: https://github.com/sensu-community/monitoring-checks#handler-list
