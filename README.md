# Sensu Monitoring Pipelines

This project contains a number of Sensu monitoring pipeline
configurations to bootstrap effective monitoring with [Sensu
Go](https://sensu.io). These pipelines are intended to be a starting
point for Sensu Go users, some per-deployment modifications are to be
expected.

## Rules

1. Every Sensu user must be able to safely create all resources as
   defined in this project (from the project root).

2. All resource definitions must be written in YAML for consistency
   and comment support.

3. All Assets used must be registered and hosted on
   [Bonsai](https://bonsai.sensu.io).

4. Asset resources must include a version reference in their name.

## Contributing

For guidelines on how to contribute to this project and information
about what we require from project contributors, please see
[CONTRIBUTING.md](CONTRIBUTING.md).
