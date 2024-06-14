include .env

load_deps:
	@if [ -z "$$DEPS_DIR" ]; then export DEPS_DIR=$$PWD/deps; fi; \
	echo $$DEPS_DIR; \
	mkdir -p $$DEPS_DIR; \
	for dep in "project1" ; do \
		if [ ! -d $$DEPS_DIR/$$dep ]; then \
			git clone --filter=blob:none --sparse \
				https://github.com/openfoodfacts/$$dep.git $$DEPS_DIR/$$dep; \
		else \
			cd $$DEPS_DIR/$$dep && git pull; \
		fi; \
		cd $$DEPS_DIR/$$dep && make -e run; \
	done

run:
	@echo PROJECT2_VAR:${PROJECT2_VAR} PRIVATE_VAR:${PRIVATE_VAR} PROJECT1_RUNNING: ${PROJECT1_RUNNING} DPES_DIR: ${DEPS_DIR}
	@make load_deps

