.PHONY: default all clean moodykubie-ui image push

default: image
all: image push

REPO := weaveworks
NAME := moodykubie-ui
IMAGE := $(REPO)/$(NAME)
VERSION := $(shell ../build/version)
CURRENT_DIR := $(dir $(realpath $(firstword $(MAKEFILE_LIST))))

clean:
	-rm -f moodykubie-ui
	-docker rmi -f \
		$(IMAGE):$(VERSION) \
		$(IMAGE):latest

moodykubie-ui: main.go
	CGO_ENABLED=0 GOOS=linux go build -a -ldflags '-extldflags "-static"' -o moodykubie-ui main.go

image: moodykubie-ui
	docker build \
		--build-arg=version=$(VERSION) \
		-t $(IMAGE):$(VERSION) \
		-t $(IMAGE):latest \
		$(CURRENT_DIR)

push: image
	docker push $(IMAGE):$(VERSION)
	docker push $(IMAGE):latest
