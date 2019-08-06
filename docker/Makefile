BUILDER_VERSION = $(shell builder/get_version.sh)
HAPI_LIMITED_REPOSITORY = mauri-opskins

DEPS_DIR=./tmp

.PHONY: build-hapi-image push-hapi-image

make_deps_dir:
	@mkdir -p $(DEPS_DIR)


get_hapi_eos: make_deps_dir
	if [ ! -d $(DEPS_DIR)/hapi-limited ]; then \
       cd $(DEPS_DIR) && \
       git clone -b hapi-limited git@github.com:$(HAPI_LIMITED_REPOSITORY)/eos.git --recursive hapi-limited; \
    else \
       cd $(DEPS_DIR)/hapi-limited && \
       git fetch --all --tags && \
       git checkout hapi-limited; \
    fi && \
    git submodule update --init --recursive


build-image: get_hapi_eos
	docker build \
        --build-arg deps_dir=$(DEPS_DIR) \
        -t waxteam/hapi .


push-image:
	docker tag waxteam/hapi waxteam/hapi:latest
	docker push waxteam/hapi:latest
