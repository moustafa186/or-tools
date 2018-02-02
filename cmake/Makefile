help:
	@echo "usage:"
	@echo "make docker: generate docker images"
	@echo "make configure: cmake configure"
	@echo "make build: cmake build"
	@echo "make test: run unit tests"
	@echo "make install: cmake install"
	@echo "make test_install: configure a sample project against an installation"
	@echo "make clean: call cmake \"make clean\""
	@echo "make distclean: clean and also remove all docker images"

# Need to add cmd_distro to PHONY otherwise target are ignored since they don't
# contain recipe (using FORCE don't work here)
.PHONY: help all
all: build

PROJECT := ortools
#UID := $(shell id -u)
#GID := $(shell id -g)
#DOCKER_DEVEL_CMD := docker run --rm -it -v ${PWD}:/project -w /project --user ${UID}:${GID}
DOCKER_DEVEL_CMD := docker run --rm -it -v ${PWD}:/project -w /project
DOCKER_INSTALL_CMD := docker run --rm -it

# $* stem
# $< first prerequist
# $@ target name

# DOCKER
.PHONY: docker docker_alpine docker_ubuntu
docker: docker_alpine docker_ubuntu
docker_alpine: build/alpine/docker_devel.tar
docker_ubuntu: build/ubuntu/docker_devel.tar
build/%/docker_devel.tar: cmake/docker/%/Dockerfile cmake/docker/%/setup.sh
	mkdir -p build/$*
	docker image rm -f ${PROJECT}_$*:devel 2>/dev/null
	docker build --no-cache -t ${PROJECT}_$*:devel -f $< cmake/docker/$*
	docker save ${PROJECT}_$*:devel -o $@

# DOCKER BASH
.PHONY: bash_alpine bash_ubuntu
bash_alpine: build/alpine/docker_devel.tar
	${DOCKER_DEVEL_CMD} ${PROJECT}_alpine:devel /bin/sh
bash_ubuntu: build/ubuntu/docker_devel.tar
	${DOCKER_DEVEL_CMD} ${PROJECT}_ubuntu:devel /bin/bash

# CONFIGURE
.PHONY: configure configure_alpine configure_ubuntu
configure: configure_alpine configure_ubuntu
configure_alpine: build/alpine/configure.log
configure_ubuntu: build/ubuntu/configure.log
build/%/configure.log: build/%/docker_devel.tar CMakeLists.txt cmake patches
	@docker load -i $<
	${DOCKER_DEVEL_CMD} ${PROJECT}_$*:devel /bin/sh -c "cmake -H. -Bbuild/$*"
	@date > $@

# BUILD
.PHONY: build build_alpine build_ubuntu
build: build_alpine build_ubuntu
build_alpine: build/alpine/build.log
build_ubuntu: build/ubuntu/build.log
build/%/build.log: build/%/configure.log ortools examples
	${DOCKER_DEVEL_CMD} ${PROJECT}_$*:devel /bin/sh -c "cmake --build build/$* --target all"
	@date > $@

# TEST
.PHONY: test test_alpine test_ubuntu
test: test_ubuntu
test_alpine: build/alpine/test.log
test_ubuntu: build/ubuntu/test.log
build/%/test.log: build/%/build.log
	${DOCKER_DEVEL_CMD} ${PROJECT}_$*:devel /bin/sh -c "cd build/$* && ctest --output-on-failure"
	@date > $@

# INSTALL
.PHONY: install install_alpine install_ubuntu
install: install_ubuntu
install_alpine: build/alpine/docker_install.tar
install_ubuntu: build/ubuntu/docker_install.tar
.PRECIOUS: build/%/install
build/%/install: build/%/build.log
	${DOCKER_DEVEL_CMD} ${PROJECT}_$*:devel /bin/sh -c "cmake --build build/$* --target install -- DESTDIR=install"

build/%/docker_install.tar: cmake/docker/%/InstallDockerfile build/%/install test/config
	docker image rm -f ${PROJECT}_$*:install 2>/dev/null
	docker build --no-cache -t ${PROJECT}_$*:install -f $< .
	docker save ${PROJECT}_$*:install -o $@

# DOCKER BASH INSTALL
.PHONY: bash_install_alpine bash_install_ubuntu
bash_install_alpine: build/alpine/docker_install.tar
	${DOCKER_INSTALL_CMD} ${PROJECT}_alpine:install /bin/sh
bash_install_ubuntu: build/ubuntu/docker_install.tar
	${DOCKER_INSTALL_CMD} ${PROJECT}_ubuntu:install /bin/bash

# TEST INSTALL of ortoolsConfigs.cmake
.PHONY: test_install test_install_alpine bash_isntall_ubuntu
test_install: test_install_alpine test_install_archlinux test_install_ubuntu
test_install_alpine: build/alpine/test_install.log
test_install_ubuntu: build/ubuntu/test_install.log
build/%/test_install.log: build/%/docker_install.tar
	@docker load -i $<
	${DOCKER_INSTALL_CMD} -w /project ${PROJECT}_$*:install /bin/sh -c "cmake -H. -Bbuild; cmake --build build"
	@date > $@

# CLEAN
.PHONY: clean clean_alpine clean_ubuntu
clean: clean_alpine clean_ubuntu
clean_alpine: clean-alpine
clean_ubuntu: clean-ubuntu
clean-%:: build/%/docker_devel.tar
	@docker load -i $<
	${DOCKER_DEVEL_CMD} ${PROJECT}_$*:devel /bin/sh -c "cmake --build build/$* --target clean"
	@rm -f build/$*/test.log
	@rm -f build/$*/build.log
	@rm -f build/$*/configure.log

# DISTCLEAN
.PHONY: distclean distclean_alpine distclean_ubuntu
distclean: distclean_alpine distclean_ubuntu
	docker image prune -f
	rmdir build
distclean_alpine: distclean-alpine
distclean_ubuntu: distclean-ubuntu
distclean-%::
	${DOCKER_DEVEL_CMD} ${PROJECT}_$*:devel /bin/sh -c "rm -rf build/$*"
	docker image rm -f ${PROJECT}_$*:devel 2>/dev/null
	docker image rm -f ${PROJECT}_$*:install 2>/dev/null

FORCE:
