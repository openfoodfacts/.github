include .env

load_deps:
	@if [ -z "$$DEPS_DIR" ]; then export DEPS_DIR=$$PWD/deps; fi; \
	echo $$DEPS_DIR; \
	mkdir -p $$DEPS_DIR; \
	for dep in "project2" ; do \
		if [ ! -d $$DEPS_DIR/$$dep ]; then \
			git clone --filter=blob:none --sparse \
				https://github.com/openfoodfacts/$$dep.git $$DEPS_DIR/$$dep; \
		else \
			cd $$DEPS_DIR/$$dep && git pull; \
		fi; \
		cd $$DEPS_DIR/$$dep && make -e run; \
	done

run:
ifndef PROJECT1_RUNNING
	@export PROJECT1_RUNNING=true; \
	echo PROJECT1_VAR:$$PROJECT1_VAR; \
	$(MAKE) load_deps
endif

next:
	export PROJECT1_RUNNING=true; \
	for dep in "robotoff" "openfoodfacts-query" ; do \
		if [ ! -f ../$$dep ]; then \
			git clone --filter=blob:none --sparse https://github.com/openfoodfacts/$$dep.git ../$$dep; \
		fi; \
		echo $$dep; \
		#cd ../$$dep && make -e run; \
	done
