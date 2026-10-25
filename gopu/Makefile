TAG ?= v0.1.0
REPO ?= etzba/gopu

all: base up test down


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

.PHONY: test
test:
	etz api --workers=3 --exec=executions/loc.yaml --values=values.yaml
	etz api --url=http://localhost:8080/locations --method=GET -d=3s -w=6 -r=6
	etz file --workers=3 --exec=executions/calc.yaml --values=values.yaml
	etz file --url=http://localhost:8080/docs --method=PUT --path=files/ -w=6 -r=6
	etz file --exec=executions/file.yaml -d=3s -w=1 -r=1
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
