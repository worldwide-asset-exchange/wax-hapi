VERSION = wax-1.8.1-1.0.0
HAPI_LIMITED_REPOSITORY = worldwide-asset-exchange/wax-hapi.git

DEPS_DIR=./tmp

.PHONY: build-image push-image

make_deps_dir:
	@mkdir -p $(DEPS_DIR)


get_hapi_eos: make_deps_dir
	if [ ! -d $(DEPS_DIR)/hapi-limited ]; then \
       cd $(DEPS_DIR) && \
       git clone -b hapi-limited git@github.com:$(HAPI_LIMITED_REPOSITORY) --recursive hapi-limited; \
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
	docker tag waxteam/hapi waxteam/hapi:$(VERSION)
	docker push waxteam/hapi:$(VERSION)
	docker tag waxteam/hapi waxteam/hapi:latest
	docker push waxteam/hapi:latest
