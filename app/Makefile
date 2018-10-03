include Rules.mk

GPU=1
CUDNN=0
OPENCV=0
OPENMP=0
DEBUG=1

APP := demo_app

ARGUS_UTILS_DIR := $(TOP_DIR)/argus/samples/utils

COMMON= -I../include
DARKNET_LIB=libdarknet.so

SRCS := \
	demo_app.cpp \
	$(wildcard $(CLASS_DIR)/*.cpp) \
	$(ARGUS_UTILS_DIR)/Thread.cpp

OBJS := $(SRCS:.cpp=.o)

CPPFLAGS += \
	-I"$(ARGUS_UTILS_DIR)"

LDFLAGS += \
	-lnveglstream_camconsumer -largus

ifeq ($(GPU), 1)
COMMON+= -DGPU -I/usr/local/cuda/include/
CPPFLAGS+= -DGPU
LDFLAGS+= -L/usr/local/cuda/lib64 -lcuda -lcudart -lcublas -lcurand
endif

all: $(APP)
	$(AT)mv $(APP) ../$(APP)

$(CLASS_DIR)/%.o: $(CLASS_DIR)/%.cpp
	$(AT)$(MAKE) -C $(CLASS_DIR)

%.o: %.cpp
	@echo "Compiling: $<"
	$(CPP) $(COMMON) $(CPPFLAGS) -c $< -o $@ -g

$(APP): $(OBJS)
	@echo "Linking: $@"
	$(CPP) $(COMMON) -o $@ $(OBJS) $(CPPFLAGS) $(LDFLAGS) $(DARKNET_LIB)

clean:
	$(AT)rm -rf $(APP) $(OBJS)
	$(AT)rm -rf ../$(APP)

test: $(OBJS)
	@echo $(SRCS)
