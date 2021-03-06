 help : 
	@echo "This is the makefile of APDL-Mode $(VERSION) using $(EMACS_VERSION)"
	@echo "make MODE = A-M package files + tutorials"
	@echo "make EMACS_SRC = Emacs sources + A-M package files + tutorials"
	@echo "make EMACS = Windoze version of emacs with pre-configured ANSYS-Mode"
	@echo "make CONTI = Archive for my company"
	@echo "make HTML = update the html documentation"
	@echo "make TAGS = make the etags"
	@echo "make ELC = compile the el files"
	@echo "make ALL = MODE + EMACS + EMACS_SRC + TAGS"
	@echo "make CLEAN = clean the elc files"
	@echo "make TAG_RELEASE = "
	@echo "make PACK = Emacs tar package"
	@echo "make [help]  = this help"
	@echo "make -k = --keep-going, even if some targets can't be made.."

# Semantic versioning: MA.Mi.Pa
APDL_MAJOR := 20
APDL_MINOR := 1
APDL_PATCH := 0


# EMACS_VER needed for Emacs' windows installaion tree
EMACS_VER := 26.3
EMACS_VERSION := emacs-$(EMACS_VER)

# complete package names are comprised of

# apdl-mode-$(APDL_MAJOR).$(ANSYS_MINOR).$(MODE_VERSION)
# tutorials: A-M_xxx

##################################################

HOSTNAME := $(shell hostname)
DIR := $(shell pwd)
# the sources dir
S_DIR := "/home/dieter/ansys-mode/"

# use make in the "tags" release directories
#RELEASE := $(shell $$($(DIR)))

ifeq ($(HOSTNAME),ping)
 EMACS_DIR := /usr/local/src/emacs/
else
 EMACS_DIR := /appl/emacs/
endif

VERSION := $(APDL_MAJOR).$(APDL_MINOR).$(APDL_PATCH)

EMACS_PACKAGE := $(EMACS_VERSION)-bin-i686-mingw32.zip
# in the ftp.gnu.org/gnu folder is the gnu-keyring.gpg
# but it is not up-to-date, use keyserver!
KEYRING := http://ftp.gnu.org/gnu/gnu-keyring.gpg
EMACS_PACKAGE_SIG := $(EMACS_PACKAGE).sig
EMACS_SOURCE_PACKAGE := $(EMACS_VERSION).tar.xz
A-M_EMACS_SOURCE_PACKAGE := ansys-mode-$(VERSION).$(EMACS_VERSION).tar.xz
EMACS_SOURCE_PACKAGE_SIG := $(EMACS_SOURCE_PACKAGE).sig
ADDRESS := http://ftp.gnu.org/gnu/emacs/windows/$(EMACS_PACKAGE)
SIG_ADDRESS := http://ftp.gnu.org/gnu/emacs/windows/$(EMACS_PACKAGE_SIG)
SOURCE_ADDRESS := http://ftp.gnu.org/gnu/emacs/$(EMACS_SOURCE_PACKAGE)
SIG_SOURCE_ADDRESS := http://ftp.gnu.org/gnu/emacs/$(EMACS_SOURCE_PACKAGE_SIG)
# ftp://ftp.informatik.rwth-aachen.de/pub/gnu/
EMACS_EXE := $(EMACS_DIR)$(EMACS_VERSION)/src/emacs

.PHONEY : EMACS_SOURCE
EMACS_SOURCE : $(EMACS_SOURCE_PACKAGE) $(EMACS_SOURCE_PACKAGE_SIG)

.PHONEY : EXE
EXE : $(EMACS_EXE)

X11_FILES := ../X11/xGetClassicsWindow.cpp ../X11/xPasteToWin.cpp ../X11/xGetFocusWindow.cpp ../X11/xSendReturn.cpp

X11_EXE := $(X11_FILES:.cpp=)

.PHONEY : TEST
TEST : 
	@echo $(VERSION)

.PHONEY : ../X11
X11 : $(X11_FILES)
	$(MAKE) -C X11

PACKAGE := apdl-mode-$(VERSION).tar

# necessary el files
EL_FILES := ../apdl-config.el ../apdl-mode.el ../apdl-keyword.el ../apdl-mode-pkg.el \
	../apdl-initialise.el ../apdl-template.el ../apdl-process.el
# fontification.el is not included since it is not necessary for running A-M

../apdl-config.el : ../apdl-config.org
	$(EMACS_EXE) --batch --file $< \
	  --execute "(add-to-list 'load-path \"~/ansys-mode\")" \
	  --load "apdl-mode.el" --execute "(org-babel-tangle)"

ELC_FILES := $(EL_FILES:.el=.elc)

# compile Elisp files
.PHONEY : ELC
ELC : $(ELC_FILES)

FILES := ../LICENSE.org ../apdl-mode-readme.txt ../README.org ../TODO.org ../NEWS.org ../INSTALLATION.org
HELPER_FILES := ../doc/example.mac ../doc/example.anf ../doc/example.dat 

#	 ../doc/APDL_tutorial.ans

# ../doc/APDL_tutorial.ans : ../doc/A-M_in-depth_tutorial.org
# 	$(EMACS_EXE) --batch --eval "(add-to-list 'load-path \"..\doc")"  \
# 	   $< -eval "(require 'ob-tangle)" -eval "(org-babel-tangle nil "APDL_tutorial.ans")"

WORK_FILES := ../LICENSE.org ../README.org ../TODO.org ../NEWS.org \
		 example.mac example.anf example.dat # tools/ansys-mode/README.Conti

WORK_EL_FILES := $(EL_FILES)

TUTORIAL_SOURCES := ../doc/A-M_introductory_tutorial.org \
	            ../doc/A-M_in-depth_tutorial.org     \
	            ../doc/A-M_APDL_reference.org

TUTORIALS := ../doc/A-M_introductory_tutorial.pdf \
 ../doc/A-M_in-depth_tutorial.ans 	          \
 ../doc/A-M_in-depth_tutorial.pdf                 \
 ../doc/A-M_APDL_reference.pdf

MATLIB := ../matlib/

X11 := \
../X11/makefile \
../X11/xGetClassicsWindow \
../X11/xGetClassicsWindow.cpp \
../X11/xGetFocusWindow \
../X11/xGetFocusWindow.cpp \
../X11/xPasteToWin \
../X11/xPasteToWin.cpp \
../X11/xSendReturn \
../X11/xSendReturn.cpp \


# PACKAGE_FILES :=  $(X11) $(FILES) $(HELPER_FILES) $(EL_FILES) $(MATLIB) $(TUTORIALS) default.el
PACKAGE_FILES :=  $(FILES) $(HELPER_FILES) $(EL_FILES) $(MATLIB) 

default.el : makefile
	@echo ";;; Automatically created from APDL-Mode-$(VERSION)" > $@
	@echo "(let* ((dir (file-name-directory load-file-name))" >> $@
	@echo "       (file (concat dir \"/apdl-mode-$(VERSION)/apdl-config.el\")))" >> $@
	@echo "  (when (file-readable-p file) (load-file file)))" >> $@

P_DIR := apdl-mode-$(VERSION)

$(PACKAGE) : $(PACKAGE_FILES)  makefile
	@echo "Packaging GNU-Emacs package $@ ..."
	@echo
	@test -d $(P_DIR) || \
	    mkdir $(P_DIR)
	@cp -a $(FILES) $(P_DIR)
	@cp -a $(EL_FILES) $(P_DIR)
	@test -d $(P_DIR)/doc || \
	    mkdir $(P_DIR)/doc
#	@cp -a $(TUTORIALS) P_DIR/doc
	@cp -a $(HELPER_FILES) $(P_DIR)/doc
	@test -d $(P_DIR)/helper || \
	    mkdir $(P_DIR)/helper
	@test -d $(P_DIR)/matlib || \
	    mkdir $(P_DIR)/matlib
# @test -d P_DIR/X11 || \
#     mkdir P_DIR/X11
	@cp -rf $(MATLIB) $(P_DIR)
	tar -cf $@ $(P_DIR) # ../apdl-mode-readme.txt # default.el
	@echo
	@echo "... $@ done."
	@echo "------------------------------"


# MODE: this is the Ansys mode "installation" package
# the sources are built from GitHub automatically when tagged
.PHONEY : PACK
PACK : $(PACKAGE)

.PHONEY : CONTI
CONTI : A-M-$(VERSION).tar.xz

A-M-$(VERSION).tar.xz : $(PACKAGE) \ # tools/apdl-mode/README.Conti
	tools/bin/A-M-$(VERSION).sh \
			makefile $(X11)
#	mv tools/apdl-mode/README.Conti tools/
	rm -fr tools/apdl-mode/*
#	mv tools/README.Conti tools/apdl-mode/
	@test -d tools/apdl-mode/apdl-mode-$(VERSION) || \
	    mkdir tools/apdl-mode/apdl-mode-$(VERSION)
	@echo "syncing.."
	cd ..;rsync -aR --exclude '*~' $(WORK_EL_FILES:../%=%) $(FILES:../%=%) \
	    $(TUTORIALS:../%=%) X11/ \
	    $(MATLIB:../%=%) $(HELPER_FILES:../%=%) $(X11_EXE:../%=%)\
		helper/tools/apdl-mode/apdl-mode-$(VERSION)
	@pwd
	cd ./tools/apdl-mode; ln -sf apdl-mode-$(VERSION) current
	tar -cJvf $@ --group HPCUser_1 tools

tools/bin/A-M-$(VERSION).sh : makefile
	@test -d tools/bin || mkdir tools/bin
	@echo "#!/bin/sh " > $@
	@echo "# Automatically created from APDL-Mode-$(VERSION)" >> $@
	@echo "emacs -l /appl/tools/apdl-mode/apdl-mode-$(VERSION)/ansys-config.el \$$*" >> $@
	chmod +x $@

.PHONEY : EMACS_SRC
EMACS_SRC : $(A-M_EMACS_SOURCE_PACKAGE)

$(A-M_EMACS_SOURCE_PACKAGE) : $(PACKAGE_FILES) $(EMACS_SOURCE_PACKAGE) makefile
	test -d $(EMACS_VERSION)/site-lisp/ansys-mode-$(VERSION) || \
	    mkdir -p $(EMACS_VERSION)/site-lisp/ansys-mode-$(VERSION)
	cp $(EMACS_SOURCE_PACKAGE) $(A-M_EMACS_SOURCE_PACKAGE)
	cp -uvr $(PACKAGE_FILES) $(EMACS_VERSION)/site-lisp/ansys-mode-$(VERSION)
	tar --append -Jvf $(A-M_EMACS_SOURCE_PACKAGE) $(EMACS_VERSION)

HTMLs := ../index.html ../README.html ../NEWS.html ../TODO.html \
	../LICENSE.html ../A-M.html ../doc/A-M_APDL_reference.html \
	../doc/A-M_in-depth_tutorial.html ../doc/A-M_introductory_tutorial.html \
	../matlib/README.html ../INSTALLATION.html

ORG_FILES := $(HTMLs:.html=.org)

%.html : %.org
	$(EMACS_EXE) --batch -eval "(add-to-list 'load-path \"..\")" -l ansys-mode.el -l htmlize.el $< -eval "(require 'ox-html)" -eval "(setq org-export-htmlize-output-type 'css)" -eval "(org-html-export-to-html)"

.PHONEY : HTML
HTML : html.tar.xz

# only archive members which are newer than the existingp archive
html.tar.xz : $(HTMLs) makefile
	tar cJvf $@ $(HTMLs)
	rm $(HTMLs)

.PHONEY : ALL
ALL : MODE EMACS EMACS_SRC HTML TAGS

.PHONEY : CLEAN
CLEAN :
	rm $(ELC_FILES)

$(EMACS_SOURCE_PACKAGE_SIG) :
	wget $(SIG_SOURCE_ADDRESS)
	gpg $(EMACS_SOURCE_PACKAGE_SIG)

$(EMACS_EXE) : $(EMACS_SOURCE)
	cp $(EMACS_SOURCE_PACKAGE) $(EMACS_DIR)
	cd $(EMACS_DIR); tar -xJvf $(EMACS_SOURCE_PACKAGE)
	cd $(EMACS_DIR)$(EMACS_VERSION); ./configure && make
#	cd $(EMACS_DIR)$(EMACS_VERSION); ./configure  --with-gif=no &&	make

../doc/A-M_APDL_reference.pdf : ../doc/A-M_APDL_reference.org
	$(EMACS_EXE) --batch --file $< \
          --execute "(add-to-list 'load-path \"~/a-m\")" \
	  --load "ansys-mode.el"  --execute "(org-latex-export-to-pdf)"

../doc/A-M_in-depth_tutorial.ans : ../doc/A-M_in-depth_tutorial.org
	$(EMACS_EXE) --batch --file $< \
	  --execute "(add-to-list 'load-path \"~/a-m\")" \
	  --load "ansys-mode.el" --execute "(org-babel-tangle)"
	mv ../doc/A-M_in-depth_tutorial.ansys ../doc/A-M_in-depth_tutorial.ans

../doc/A-M_in-depth_tutorial.pdf : ../doc/A-M_in-depth_tutorial.org
	$(EMACS_EXE) --batch  --file $< \
	  --execute "(add-to-list 'load-path \"~/a-m\")" \
	  --load "ansys-mode.el" --execute "(org-latex-export-to-pdf)"

../doc/A-M_introductory_tutorial.pdf : ../doc/A-M_introductory_tutorial.org
	$(EMACS_EXE) --batch --file $< \
	--load "~/ansys-mode/helper/export.el" \
	--execute  "(org-beamer-export-to-pdf)"

../ansys-keyword.el : fontification.el ansys_dynprompt.txt ansys_elements.txt ansys_parametric_functions.txt ansys_get_functions.txt ansys_keywords.txt
	$(EMACS_EXE) --batch --load $<
	ln -f ansys-keyword.el ..

# this is for compiling Elisp files, we need to set the load-path for a pristine Emacs
# -f means --funcall
# -L means --directory prepend to load-path
%.elc : %.el
	$(EMACS_EXE) --batch -L $(S_DIR) -f batch-byte-compile $<

# This is Emacs for Windows packaged with Ansys mode
.PHONEY : EMACS
EMACS : $(EMACS_PACKAGE) $(PACKAGE)
	 test -d share/emacs/$(EMACS_VER)/site-lisp/ansys-mode-$(VERSION) || \
	    mkdir -p share/emacs/$(EMACS_VER)/site-lisp/ansys-mode-$(VERSION)
	 cp -uv $(FILES) share/emacs/$(EMACS_VER)/site-lisp/ansys-mode-$(VERSION)
	 cp -uv $(EL_FILES) share/emacs/$(EMACS_VER)/site-lisp/ansys-mode-$(VERSION)
	 cp -rf $(MATLIB) share/emacs/$(EMACS_VER)/site-lisp/ansys-mode-$(VERSION)
	 test -d share/emacs/$(EMACS_VER)/site-lisp/ansys-mode-$(VERSION)/doc || \
	    mkdir -p share/emacs/$(EMACS_VER)/site-lisp/ansys-mode-$(VERSION)/doc
	 cp -uv $(TUTORIALS) share/emacs/$(EMACS_VER)/site-lisp/ansys-mode-$(VERSION)/doc
	 cp -uv default.el share/emacs/$(EMACS_VER)/site-lisp
	 cp -uv $(EMACS_PACKAGE) ansys-mode-$(VERSION).$(EMACS_PACKAGE)
	 zip -urv ansys-mode-$(VERSION).$(EMACS_PACKAGE) \
	   share/emacs/$(EMACS_VER)/site-lisp/ansys-mode-$(VERSION)/*
	 zip -uv ansys-mode-$(VERSION).$(EMACS_PACKAGE) \
	   share/emacs/$(EMACS_VER)/site-lisp/default.el

##################################################
# getting the emacs source
##################################################

## !!! gnu-keyring is not current: use key-servers!!!!
# gnu-keyring.gpg : 
# 	wget $(KEYRING)
$(EMACS_SOURCE_PACKAGE) : 
	wget $(SOURCE_ADDRESS)

$(EMACS_PACKAGE) :  $(EMACS_PACKAGE_SIG)
	wget $(SIG_ADDRESS)
	wget $(ADDRESS)
	gpg $(EMACS_PACKAGE_SIG)

# need the org files for the versioning string
TAGS_FILES := makefile $(EL_FILES) $(FILES) $(TUTORIAL_SOURCES) fontification.el ../apdl-config.org

TAGS : $(TAGS_FILES)
	etags $(TAGS_FILES)

.PHONEY : TAG_RELEASE
TAG_RELEASE :
	git tag release-$(VERSION)
# echo "git tag release-$(VERSION)"
