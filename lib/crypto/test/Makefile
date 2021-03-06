include $(ERL_TOP)/make/target.mk
include $(ERL_TOP)/make/$(TARGET)/otp.mk

# ----------------------------------------------------
# Target Specs
# ----------------------------------------------------

MODULES = \
	blowfish_SUITE \
	crypto_SUITE \
	old_crypto_SUITE

ERL_FILES= $(MODULES:%=%.erl)

TARGET_FILES= $(MODULES:%=$(EBIN)/%.$(EMULATOR))

SOURCE = $(ERL_FILES) $(HRL_FILES)

EMAKEFILE=Emakefile

# ----------------------------------------------------
# Release directory specification
# ----------------------------------------------------
RELSYSDIR = $(RELEASE_PATH)/crypto_test


# ----------------------------------------------------
# FLAGS
# ----------------------------------------------------
ERL_MAKE_FLAGS += 
ERL_COMPILE_FLAGS +=

EBIN = .
MAKE_EMAKE = $(wildcard $(ERL_TOP)/make/make_emakefile)


# ----------------------------------------------------
# Targets
# ----------------------------------------------------

# Backward compatibility, for R9B and earlier.

ifeq ($(MAKE_EMAKE),)

RELTEST_FILES = $(SOURCE) $(TARGET_FILES)
TEST_TARGET = tests

tests debug opt: $(TARGET_FILES)

else

RELTEST_FILES = $(EMAKEFILE) $(SOURCE) 
TEST_TARGET = make_emakefile


tests debug opt: make_emakefile
	erl $(ERL_MAKE_FLAGS) -make

make_emakefile:
	$(ERL_TOP)/make/make_emakefile $(ERL_COMPILE_FLAGS) -o$(EBIN) \
	$(MODULES) > $(EMAKEFILE)

endif
clean:
	rm -f $(EMAKEFILE)
	rm -f $(TARGET_FILES) $(GEN_FILES)
	rm -f core

docs:

# ----------------------------------------------------
# Release Target
# ---------------------------------------------------- 
include $(ERL_TOP)/make/otp_release_targets.mk

release_spec:

release_tests_spec: $(TEST_TARGET)
	$(INSTALL_DIR) "$(RELSYSDIR)"
	$(INSTALL_DATA) crypto.spec crypto.cover $(RELTEST_FILES) "$(RELSYSDIR)"
	chmod -R u+w "$(RELSYSDIR)"

release_docs_spec:

