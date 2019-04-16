PYTHON=python3
SETUP=$(PYTHON) setup.py

EXCLUDES=.vscode
STYLE_TARGETS=trezorlib trezorctl setup.py
EXCLUDE_TARGETS=trezorlib/messages

all: build

build:
	$(SETUP) build

install:
	$(SETUP) install

dist: clean
	$(SETUP) sdist
	$(SETUP) bdist_wheel

clean: clean-generated clean-build clean-pyc clean-test ## remove all build, test, coverage and Python artifacts

clean-generated: ## remove generated files
	rm -f trezorlib/messages/*.py
	rm -f trezorlib/coins.json

clean-build: ## remove build artifacts
	rm -fr build/
	rm -fr dist/
	rm -fr .eggs/
	find . -name '*.egg-info' -exec rm -fr {} +
	find . -name '*.egg' -exec rm -f {} +

clean-pyc: ## remove Python file artifacts
	find . -name '*.pyc' -exec rm -f {} +
	find . -name '*.pyo' -exec rm -f {} +
	find . -name '*~' -exec rm -f {} +
	find . -name '__pycache__' -exec rm -fr {} +

clean-test: ## remove test and coverage artifacts
	rm -fr .tox/
	rm -f .coverage
	rm -fr htmlcov/
	rm -fr .pytest_cache

git-clean:
	git clean -dfx -e $(EXCLUDES)

style:
	black $(STYLE_TARGETS)
	isort --apply --recursive $(STYLE_TARGETS) --skip-glob "*/$(EXCLUDE_TARGETS)/*"
	autoflake -i --remove-all-unused-imports -r $(STYLE_TARGETS) --exclude "$(EXCLUDE_TARGETS)"

style_check:
	black --check $(STYLE_TARGETS)
	isort --diff --check-only --recursive $(STYLE_TARGETS) --skip-glob "*/$(EXCLUDE_TARGETS)/*"
	flake8

.PHONY: all build install clean style style_check git-clean clean-generated clean-build clean-pyc clean-test
