.PHONY : all clean rebuild test

TOP_PATH := $(realpath ..)
THE_PATH := $(notdir  $(realpath .))

COMMON_INC := $(TOP_PATH)/common/inc

SRC_DIR := src
INC_DIR := inc

TYPE_INC := h
TYPE_SRC := c
TYPE_OBJ := o
TYPE_DEP := dep

AR := ar
FLAGS_AR := -crs
CC := gcc
FLAGS_CC := -I $(INC_DIR) $(COMMON_INC)
ifeq ($(DEBUG),true)
FLAGS_CC := $(FLAGS_CC) -g
endif
MKDIR := mkdir

BUILD_DIR := $(TOP_PATH)/build
TARGET_DIR := $(BUILD_DIR)/$(THE_PATH)

DIRS := $(BUILD_DIR) $(TARGET_DIR)

SRCS := $(wildcard $(SRC_DIR)/*.$(TYPE_SRC))
OBJS := $(SRCS:.$(TYPE_SRC)=.$(TYPE_OBJ))
OBJS := $(patsubst $(SRC_DIR)/%,$(TARGET_DIR)/%,$(OBJS))

DEPS := $(OBJS:.$(TYPE_OBJ)=.$(TYPE_DEP))

OUTPUT := $(BUILD_DIR)/$(THE_PATH).a

vpath %.$(TYPE_INC) $(INC_DIR)
vpath %.$(TYPE_INC) $(COMMON_INC)
vpath %.$(TYPE_SRC) $(SRC_DIR)

include $(DEPS)


all : $(OUTPUT)
	@echo "OUTPUT ==> $(OUTPUT)"

$(DIRS): % :
	$(MKDIR) $@

$(OUTPUT) : $(OBJS)
	$(AR) $(FLAGS_AR) -o $@ $^
$(OBJS) : 
	$(CC) $(FLAGS_CC) -o $@ -c  $(filter %.$(TYPE_SRC),$^)


test : 
	@echo "OBJS ==> $(OBJS)"


$(TARGET_DIR)/%.$(TYPE_DEP) : $(SRC_DIR)/%.$(TYPE_SRC)
	@$(CC) -MM -E $(filter %.$(TYPE_SRC),$^) | sed 's,\(.*\)\.o[:]*,$(TARGET_DIR)/\1.o  $@ \: ,g' > $@

