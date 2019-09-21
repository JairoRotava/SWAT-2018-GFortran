# SWAT rev670 makefile for Gfortran GNU
#
# Use:
# make all	- create directories and build release
# make release	- build release version
# make debug	- build debug version
# make clean	- cleanup
# make prep	- create directories
#
#
# You may need to remove the -ffixed-line-length-none flag to compile some different swat versions.

#
# List of modules: names starting with mod
#
MODS:=$(wildcard mod*.f)
MOD_OBJS:=$(patsubst %.f,%.o,$(MODS))

#
# List of source files: files with f and f90 extensions.
#
SRCS:=$(wildcard *.f *.f90)
OBJS:=$(patsubst %.f,%.o,$(patsubst %.f90,%.o,$(SRCS)))

#
# Compiler/Linker program: should be gfortran.
#
#FC=x86_64-conda_cos6-linux-gnu-gfortran
FC=gfortran

#
# Program name
#
PROGRAM=swat

#
# Debug build settings
#
DBGDIR = debug
DBGEXE = $(DBGDIR)/$(PROGRAM)
DBGOBJS = $(addprefix $(DBGDIR)/, $(OBJS))
DBGMODS = $(addprefix $(DBGDIR)/, $(MOD_OBJS))
DBGCFLAGS = -c -O0 -static -ffixed-line-length-none -ffree-line-length-none -std=gnu -ffpe-trap=invalid,zero,overflow -fno-automatic  -fcheck=all -fbacktrace
DBGLFLAGS = -g

#
# Release build settings
#
RELDIR = release
RELEXE = $(RELDIR)/$(PROGRAM)
RELOBJS = $(addprefix $(RELDIR)/, $(OBJS))
RELMODS = $(addprefix $(RELDIR)/, $(MOD_OBJS))
RELCFLAGS = -c -O3 -static -ffixed-line-length-none -ffree-line-length-none -std=gnu -ffpe-trap=invalid,zero,overflow -fno-automatic
RELLFLAGS = -g

.PHONY: all clean debug prep release remake

#
# Default build
#
all: prep release


#
# Release rules
#
release: $(RELEXE)

# Linker: build program. Modules and objects are required
$(RELEXE): $(RELMODS) $(RELOBJS)
	$(FC) $(RELLFLAGS) -o $(RELEXE) $^

# Compiler: compile modules and source
$(RELDIR)/%.o: %.f
	$(FC) $(RELCFLAGS) -o $@ $<

$(RELDIR)/%.o: %.f90
	$(FC) $(RELCFLAGS) -o $@ $<


#
# Debug rules
#
debug: $(DBGEXE)

# Linker
$(DBGEXE): $(DBGMODS) $(DBGOBJS)
	$(FC) $(DBGLFLAGS) -o $(DBGEXE) $^

# Compiler
$(DBGDIR)/%.o: %.f
	$(FC) $(DBGCFLAGS) -o $@ $<

$(DBGDIR)/%.o: %.f90
	$(FC) $(DBGCFLAGS) -o $@ $<


#
# Clean rules
#
clean:
	rm -f $(RELEXE) $(RELOBJS) $(DBGEXE) $(DBGOBJS)

#
# prep rules: create release and debug directories
#
prep:
	@mkdir -p $(DBGDIR) $(RELDIR)

#
# Remake rule
#
remake: clean all

