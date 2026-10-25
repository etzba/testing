<p align="center">
  <img src="etzba.png" alt="Etzba"/>
</p>

# testing

A repository to store etz load testing use cases in yaml files

### overview

This repository created to test releases, edit docs of https://etzba.com and give examples for users who want to create load tests with `etz` CLI.

#### structure

The repository has directory for each service to test. Inside the directory there are several files and folder in the same structure:
- `Makefile`
- `docker-compose.yaml` 
- `executions/`
- `general_config/`

There is also a `results/` to store results in json. Use `--output=results/<file_name>.json` while executing a load test to store it there.

### prerequisites

- Install `etz` by following the instruction in https://etzba.com or https://github.com/etzba/etz

- Install `docker` and `docker-compose`

- If you test postgres, probably you'll need pg client `sudo apt install postgresql-client-16`



Have fun!