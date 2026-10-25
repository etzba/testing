### Postgres tests

#### Run

to run tests use `make`

#### Setting

change docker image in `Makefile` under `TAG ?= <tag>`

seed database by using `seed.sql` file

database local secrets stored in `secrets.*` files

#### Dir structure

`executions/` dir is dedicated to store `etz` execution files using `pg` sub-command and `--exec=` arg

```sh
etz pg --workers=3 --auth=secret.json --exec=executions/executions.yaml --duration=3s
```

`general_config/` dir is dedicated to store `etz` general config files using `--config=` arg without sub-command

```sh
etz --config=general_config/config.yaml
```