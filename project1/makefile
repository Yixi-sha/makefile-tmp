.PHONY : all clean rebuild test

INC_DIR := inc
SRC_DIR := src

vpath %.h $(INC_DIR)
vpath %.c $(SRC_DIR)

TARGET := app.out
TARGET_DIR := out
TARGET := $(addprefix $(TARGET_DIR)/,$(TARGET))


CC := gcc
CFLAGS := -I $(INC_DIR)


OBJ_DIR := obj
SRCS := $(wildcard $(SRC_DIR)/*.c)
SROS := $(SRCS:.c=.o)
SROS := $(patsubst  $(SRC_DIR)/%,$(OBJ_DIR)/%,$(SROS))


DEP_DIR := dep
DEPS := $(SRCS:.c=.dep)
DEPS := $(patsubst $(SRC_DIR)/%,$(DEP_DIR)/%,$(DEPS))

DIRS := $(OBJ_DIR) $(TARGET_DIR) $(DEP_DIR)


MKDIR := mkdir
RM := rm -rf

ifneq ("$(MAKECMDGOALS)","clean")
include $(DEPS)
endif



all : $(TARGET)

$(TARGET) : $(TARGET_DIR) $(SROS)
    ifeq ($(DEBUG),true)
		$(CC) -o  $@ -g $(SROS)
    else
		$(CC) -o  $@  $(SROS)
    endif



ifeq ("$(wildcard $(OBJ_DIR))", "")
$(OBJ_DIR)/%.o : %.c $(OBJ_DIR)	
else
$(OBJ_DIR)/%.o :  %.c
endif
	@echo "$@ : $^"
	$(CC) $(CFLAGS) -o $@ -c $(filter %.c,$^)



$(DIRS) : % :
	$(MKDIR) $@

clean : 
	$(RM) $(DIRS)

rebuild : 
	set -e ; \
	$(MAKE) clean ; \
	$(MAKE) all ; \




ifeq ("$(wildcard $(DEP_DIR))", "")
$(DEP_DIR)/%.dep : %.c $(DEP_DIR)
else
$(DEP_DIR)/%.dep : %.c
endif
	@echo "create dep file of $<"
	@$(CC) -MM -E $(filter %.c,$^) | sed 's,\(.*\)\.o[:]*,$(OBJ_DIR)/\1.o  $@ \: ,g' > $@

test :
	@echo " SROS := $(SROS)"