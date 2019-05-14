HUB ?= slarkin
PROJECT ?=

# Using same naming convention as istio/istio
VERSION ?= go1.11-bazel0.22-clang7
IMG ?= istio-proxy-builder

# Build a local image, can be used for testing with circleci command line.
image:
	docker build -t ${HUB}$(PROJECT)/${IMG}:$(VERSION) -f Dockerfile .

# Push the image to docker
push:
	docker push "${HUB}$(PROJECT)/${IMG}:$(VERSION)"

# Build istio-proxy (including envoy) inside the docker container
build-istio:
	cd $(TOP) && docker run -it --rm -it \
       -v $(PWD):$(TOP) -w $(TOP) \
       -v /tmp/istio-build:/home/circleci \
       --name $(IMG) \
       -e USER=$(USER) \
       -e BAZEL_BUILD_ARGS="--local_resources=12288,5,1" \
       -e BAZEL_TEST_ARGS="--test_env=ENVOY_IP_TEST_VERSIONS=v4only --test_output=all --local_resources=12288,5,1 --local_test_jobs=8" \
       ${HUB}$(PROJECT)/${IMG}:$(VERSION) /bin/bash -c 'make build_envoy'

.PHONY: image push latest
