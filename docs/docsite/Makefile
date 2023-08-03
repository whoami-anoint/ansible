# Set the build output directory for the main docsite if it's not already specified
BUILDDIR := _build
POTDIR := $(BUILDDIR)/gettext

OS := $(shell uname -s)
CPUS := $(shell nproc)

# Intenationalisation and Localisation
LANGUAGES ?=

# Paths to scripts
PYTHON := python
GENERATE_RST_SCRIPT := ../../hacking/build-ansible.py
TESTING_FORMATTER_SCRIPT := ../bin/testing_formatter.sh

# Fetch version from project release.py as a single source of truth
VERSION := $(shell $(PYTHON) ./version_helper.py --raw || echo error)
MAJOR_VERSION := $(shell $(PYTHON) ./version_helper.py --majorversion || echo error)

PLUGIN_FORMATTER_SCRIPT := $(GENERATE_RST_SCRIPT) docs-build
TESTING_FORMATTER_SCRIPT := ../bin/testing_formatter.sh
KEYWORD_DUMPER_SCRIPT := $(GENERATE_RST_SCRIPT) document-keywords
CONFIG_DUMPER_SCRIPT := $(GENERATE_RST_SCRIPT) document-config
GENERATE_CLI_SCRIPT := $(GENERATE_RST_SCRIPT) generate-man
COLLECTION_DUMPER_SCRIPT := $(GENERATE_RST_SCRIPT) collection-meta

.PHONY: all docs clean htmldocs singlehtmldocs coredocs core_htmldocs gettext_generate_rst generate-po linkcheckdocs htmlsingle webdocs

all: docs

docs: htmldocs

coredocs: core_htmldocs

# ...

# Add more targets and descriptions here

webdocs: docs

# ...

# ...

# ...

# Add target descriptions for other targets

clean:
	@echo "Cleaning $(BUILDDIR)"
	-rm -rf $(BUILDDIR)/doctrees $(BUILDDIR)/html htmlout module_docs
	-rm -f .buildinfo objects.inv
	-rm -rf *.doctrees
	@echo "Cleaning up minified css files"
	find . -type f -name "*.min.css" -delete
	@echo "Cleaning up byte compiled python stuff"
	find . -regex ".*\.py[co]$$" -delete
	@echo "Cleaning up editor backup files"
	find . -type f \( -name "*~" -or -name "#*" \) -delete
	find . -type f \( -name "*.swp" \) -delete
	@echo "Cleaning up generated rst"
	rm -f rst/playbooks_directives.rst
	rm -f rst/reference_appendices/config.rst
	rm -f rst/reference_appendices/playbooks_keywords.rst
	rm -f rst/dev_guide/collections_galaxy_meta.rst
	rm -f rst/cli/*.rst
	for filename in `ls rst/collections/` ; do \
		if test x"$$filename" != x'all_plugins.rst' ; then \
			rm -rf "rst/collections/$$filename"; \
		fi \
	done
	@echo "Cleaning up generated ansible_structure"
	find . -type l -delete
	@echo "Cleaning up legacy generated rst locations"
	rm -rf rst/modules
	rm -f rst/plugins/*/*.rst

# ...

# ...

# ...

# Add more targets and descriptions here

collections_meta: ../templates/collections_galaxy_meta.rst.j2
	$(COLLECTION_DUMPER_SCRIPT) --template-file=../templates/collections_galaxy_meta.rst.j2 --output-dir=rst/dev_guide/ $(EXTRA_COLLECTION_META_ARGS) ../../lib/ansible/galaxy/data/collections_galaxy_meta.yml

# ...

# ...

# ...

# Add more targets and descriptions here

gettext: gettext_structure gettext_generate_rst
	CPUS=$(CPUS) $(MAKE) -f Makefile.sphinx gettext
	# ...

generate-po:
ifeq ($(LANGUAGES),)
	@echo 'LANGUAGES is not defined. It is mandatory. LANGUAGES should be a comma separated list of languages to support. (Example: fr,es)'
else
	(cd docs/docsite/; sphinx-intl update -w 0 -d rst/locales -p "$(POTDIR)" -l $(LANGUAGES))
endif

needs-translation:
ifeq ($(LANGUAGES),)
	@echo 'LANGUAGES is not defined. It is mandatory. LANGUAGES should be a comma separated list of languages to support. (Example: fr,es)'
else
	(cd docs/docsite/; sphinx-intl stat -d rst/locales -l $(LANGUAGES) | grep -E ' [1-9][0-9]* (fuzzy|untranslated)' | sort)
endif

htmldocs: ansible_structure generate_rst
	CPUS=$(CPUS) $(MAKE) -f Makefile.sphinx html

core_htmldocs: core_structure core_generate_rst
	CPUS=$(CPUS) $(MAKE) -f Makefile.sphinx html

singlehtmldocs: ansible_structure generate_rst
	CPUS=$(CPUS) $(MAKE) -f Makefile.sphinx singlehtml

core_singlehtmldocs: core_structure core_generate_rst
	CPUS=$(CPUS) $(MAKE) -f Makefile.sphinx singlehtml

# ...

# ...

# ...

# Add more targets and descriptions here

webdocs: docs

# ...

# ...

# ...

# Add more targets and descriptions here
