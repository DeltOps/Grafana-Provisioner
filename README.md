# Grafana Terraform Simplifier
This project aims at simplifying the deployment of Grafana objects.

This does not implement everything from the Terraform Provider for Grafana. Pull Requests are welcome !

## Usage
```
./setup [<config_file>]
    config_file (default: config.yaml)
```

Will generate a file (`generated.tf`) based on the config file. The generated file should be readable by terraform. Then you'll just need:
```
terraform init
terraform apply
```

## Documentation
You should find what you are looking for in the [docs/ directory](docs/).

## Overview
```
config.yaml.sample  <- Sample configuration file
dashboards/         <- A place to store dashboards json files
docs/               <- Documentation
README.md           <- This file
setup               <- The executable generating the terraform file
templates/          <- The jinja2 templates for terraform resources
```

