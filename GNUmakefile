#
# GNUmakefile: used to generate the Manta Debugging Guide from the asciidoc
# source files.
#

#
# Variables related to the guide itself and its dependencies.
#
GUIDE_HTML             = debugging-guide.html
GUIDE_DEPS             = debugging-guide.adoc \
                         0000-intro.adoc \
                         0001-incident-response.adoc \
                         0003-quick-reference.adoc
ASCIIDOC_OPTIONS       = -b html5

#
# Variables related to final output directories. 
#
OUT_DIR                = build
OUT_GUIDE              = $(OUT_DIR)/$(GUIDE_HTML)

#
# Macros for command used in recipes
#
ASCIIDOC               = asciidoctor -o $@ $(ASCIIDOC_OPTIONS) $<
MKDIRP                 = mkdir -p $@

#
# Recipes
#
.DEFAULT_GOAL = all

#
# The "all" target builds all of the artifacts -- currently just the HTML
# version of the guide itself.
#
.PHONY: all
all: $(OUT_GUIDE)

$(OUT_GUIDE): $(GUIDE_DEPS) | $(OUT_DIR)
	$(ASCIIDOC)

$(OUT_DIR):
	$(MKDIRP)

#
# The "clean" target should bring the whole repo to the state of a fresh
# checkout.  For simplicity, we put everything into a separate "build"
# directory.
#
.PHONY: clean
clean:
	rm -rf $(OUT_DIR)
