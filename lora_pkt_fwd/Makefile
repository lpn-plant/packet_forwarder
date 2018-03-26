### Application-specific constants

APP_NAME := lora_pkt_fwd

### Environment constants

ARCH ?=
CROSS_COMPILE ?=

OBJDIR = obj
INCLUDES = $(wildcard inc/*.h)

### External constant definitions
# must get library build option to know if mpsse must be linked or not

RELEASE_VERSION := `cat ../VERSION`

### Constant symbols

CC := $(CROSS_COMPILE)gcc
AR := $(CROSS_COMPILE)ar

CFLAGS := -O2 -Wall -Wextra -std=c99 -Iinc -I. -I$(LGW_PATH)/inc
VFLAG := -D VERSION_STRING="\"$(RELEASE_VERSION)\"" -D GLOBAL_CONF_PATH="\"$(GLOBAL_CONF_PATH)\""

### Linking options

LIBS := -lloragw -lmpsse -lrt -lpthread -lm

### General build targets

all: $(APP_NAME)

clean:
	rm -f $(OBJDIR)/*.o
	rm -f $(APP_NAME)

### Sub-modules compilation

$(OBJDIR):
	mkdir -p $(OBJDIR)

$(OBJDIR)/%.o: src/%.c $(INCLUDES) | $(OBJDIR)
	$(CC) -c $(CFLAGS) $< -o $@

### Main program compilation and assembly

$(OBJDIR)/$(APP_NAME).o: src/$(APP_NAME).c $(INCLUDES) | $(OBJDIR)
	$(CC) -c $(CFLAGS) $(VFLAG) $< -o $@

$(APP_NAME): $(OBJDIR)/$(APP_NAME).o $(OBJDIR)/parson.o $(OBJDIR)/base64.o $(OBJDIR)/jitqueue.o $(OBJDIR)/timersync.o
	$(CC) -L. $< $(OBJDIR)/parson.o $(OBJDIR)/base64.o $(OBJDIR)/jitqueue.o $(OBJDIR)/timersync.o -o $@ $(LIBS)

### EOF
