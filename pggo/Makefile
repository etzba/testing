TAG ?= v0.1.0
REPO ?= etzba/pggo
PG_VERSION ?= 16

all: base up prepare test down

.PHONY: base
base:
	etz version
	etz --version
	etz license
	etz help

up:
	docker-compose down
	sleep 3
	REPO=${REPO} TAG=${TAG} PG_VERSION=${PG_VERSION} docker-compose up -d

prepare:
	sleep 12

.PHONY: test
test:
	etz api --workers=3 --exec=executions/loc.yaml --values=values.yaml
	etz api --url=http://localhost:8080/locations --method=GET -d=3s -w=6 -r=6
	etz --config=general_config/config.yaml
	
.PHONY: down
down:
	docker-compose down

.PHONY: cleanup
cleanup:
	docker rm $$(docker stop $$(docker ps -a -q --filter ancestor=${REPO}:${TAG} --format="{{.ID}}"))

.PHONY: run
run:
	docker run -t --rm ${REPO}:${TAG}
