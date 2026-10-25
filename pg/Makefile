TAG ?= 16
REPO ?= postgres

all: base up prepare seed test down


.PHONY: base
base:
	etz version
	etz --version
	etz license
	etz help

up:
	docker-compose down
	sleep 3
	TAG=${TAG} docker-compose up -d

prepare:
	sleep 12

seed:
	PGPASSWORD=Pass1234 psql -h localhost -U etzba -d etzba < seed.sql

.PHONY: test
test:
	etz --config=general_config/config.yaml
	etz --config=general_config/config.yaml --values=values.yaml
	ETZBA_POSTGRES_DB=etzba ETZBA_POSTGRES_HOST=localhost ETZBA_POSTGRES_PORT=5432 ETZBA_POSTGRES_USER=etzba ETZBA_POSTGRES_PASSWORD=Pass1234 etz pg --workers=2 --exec=executions/executions.yaml --duration=3s
	etz pg --workers=3 --auth=secret.json --exec=executions/executions.yaml --values=values.yaml
	etz pg --workers=1 --auth=secret.json --exec=executions/executions.yaml --verbose --rps=1
	etz pg --workers=3 --auth=secret.json --exec=executions/executions.yaml --duration=3s
	etz pg --auth=secret.json --exec=executions/executions.yaml --duration=3s --values=values.yaml
	ETZBA_POSTGRES_DB=etzba ETZBA_POSTGRES_HOST=localhost ETZBA_POSTGRES_PORT=5432 ETZBA_POSTGRES_USER=etzba ETZBA_POSTGRES_PASSWORD=Pass1234 etz pg --workers=10 --auth=secret.yaml --exec=executions/executions.yaml --duration=3s
	etz pg --rps=100 --auth=secret.yaml --exec=executions/executions.yaml --duration=3s --values=values.yaml
	etz pg --workers=20 --auth=secret.yaml --exec=executions/executions.yaml --duration=10s --rps=50 --output=../results/$$(date +%Y%m%d_%H%M%S)_result.json
	etz pg --workers=200 --auth=secret.yaml --exec=executions/executions.yaml --duration=10s --rps=500 --output=../results/$$(date +%Y%m%d_%H%M%S)_result.json

.PHONY: down
down:
	docker-compose down

.PHONY: cleanup
cleanup:
	docker rm $$(docker stop $$(docker ps -a -q --filter ancestor=postgres:14 --format="{{.ID}}"))

.PHONY: run
run:
	docker run -t --rm ${REPO}:${TAG}
