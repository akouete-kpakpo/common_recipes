-include ./testing_recipes/makefiles/.env.mk
export ./testing_recipes/makefiles/.env.mk
-include ./testing_recipes/makefiles/log.mk

SUBMODULE_FOLDER := testing_recipes
SUBMODULE_ROOT_FILES_PATH := ${SUBMODULE_FOLDER}/root_files

# ============================
#   ENV
# ============================
.PHONY: venv
venv: ## Create virtualenv
	$(PYTHON) -m venv $(VENV_NAME)
	@$(MAKE) $(MAKE_TAG) venv-activate
	$(PIP) install --upgrade pip
	$(PIP) install -r requirements/dev.txt
	$(PIP) install -e .


.PHONY: venv-activate
venv-activate: ## Activate virtual env
	$(VENV_ACTIVATE)


# ============================
#   TEST
# ============================

pytest:  ## Project pytest
	${PYTEST} -m "${mark}" ${var}
.PHONY: pytest


.PHONY: unit-test
unit-test:  ## Run a test
	$(MAKE) ${MAKE_TAG} echo-purple msg="+++unit-test:"
	@$(MAKE) ${MAKE_TAG} pytest var=${TEST_PATH} mark=ut



# ============================
#   SUBMODULE
# ============================
init-submodule: ## Add submodule 
	git submodule update --init --recursive
.PHONY: init-submodule


update-submodule:  ## Update submodule to the last version
	@$(MAKE) ${MAKE_TAG} echo-purple msg="+++update-submodule:"
	$(eval SUBMODULE_SHA=$(shell grep ref: ${CI_FILE} | cut -d '"' -f 2))
	cd ${SUBMODULE_FOLDER} && git fetch && git checkout --quiet $(SUBMODULE_SHA) && cd -
	@$(MAKE) ${MAKE_TAG} remove-root-files
	@$(MAKE) ${MAKE_TAG} get-root-files
.PHONY: update-submodule


remove-root-files:  ## remove configurations & requirements files from submodule
	@$(MAKE) ${MAKE_TAG} echo-purple msg="+++remove-root-files:"
	find ${SUBMODULE_ROOT_FILES_PATH} -type f | sed -e 's/testing_recipes\/root_files\///g' | xargs rm 2> /dev/null || true
.PHONY: remove-root-files


get-root-files:  ## Get configurations & requirements files from submodule
	@$(MAKE) ${MAKE_TAG} echo-purple msg="+++get-root-files:"
	cp -r ${SUBMODULE_ROOT_FILES_PATH}/. .
.PHONY: get-root-files
