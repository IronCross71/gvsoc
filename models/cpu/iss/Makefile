COMPONENTS += cpu/iss/iss

COMMON_SRCS = cpu/iss/vp/src/iss_wrapper.cpp cpu/iss/src/iss.cpp cpu/iss/src/insn_cache.cpp cpu/iss/src/csr.cpp cpu/iss/src/decoder.cpp cpu/iss/src/trace.cpp cpu/iss/flexfloat/flexfloat.c

COMMON_CFLAGS = -DRISCV=1 -DRISCY -I$(CURDIR)/cpu/iss/include -I$(CURDIR)/cpu/iss/vp/include -I$(CURDIR)/cpu/iss/flexfloat -march=native -fno-strict-aliasing

ifdef USE_TRDB
COMMON_CFLAGS += -DUSE_TRDB=1
COMMON_LDFLAGS = -ltrdb -lbfd -lopcodes -liberty -lz
endif


define declare_iss_isa_build

$(VP_BUILD_DIR)/cpu/iss/iss_wrapper/$(1)_decoder_gen.hpp: cpu/iss/isa_gen/isa_riscv_gen.py cpu/iss/isa_gen/isa_gen.py
	./cpu/iss/isa_gen/isa_riscv_gen.py --source-file=$(VP_BUILD_DIR)/cpu/iss/iss_wrapper/$(1)_decoder_gen.cpp --header-file=$(VP_BUILD_DIR)/cpu/iss/iss_wrapper/$(1)_decoder_gen.hpp $(2)

$(VP_BUILD_DIR)/cpu/iss/iss_wrapper/$(1)_decoder_gen.cpp: cpu/iss/isa_gen/isa_riscv_gen.py cpu/iss/isa_gen/isa_gen.py
	./cpu/iss/isa_gen/isa_riscv_gen.py --source-file=$(VP_BUILD_DIR)/cpu/iss/iss_wrapper/$(1)_decoder_gen.cpp --header-file=$(VP_BUILD_DIR)/cpu/iss/iss_wrapper/$(1)_decoder_gen.hpp $(2)

endef

define declare_iss_build

IMPLEMENTATIONS += cpu/iss/iss_$(1)

cpu/iss/iss_$(1)_SRCS += $(COMMON_SRCS)
cpu/iss/iss_$(1)_CFLAGS += $(COMMON_CFLAGS)
cpu/iss/iss_$(1)_LDFLAGS += $(COMMON_LDFLAGS)

endef

$(eval $(call declare_iss_isa_build,riscy))

ifeq '$(iss_class)' ''
cpu/iss/iss_riscy_SRCS += $(VP_BUILD_DIR)/cpu/iss/iss_wrapper/riscy_decoder_gen.cpp
$(eval $(call declare_iss_build,riscy))
endif

ifeq '$(iss_class)' 'iss_riscy_single_regfile'
cpu/iss/iss_riscy_single_regfile_CFLAGS += -DISS_SINGLE_REGFILE
cpu/iss/iss_riscy_single_regfile_SRCS += $(VP_BUILD_DIR)/cpu/iss/iss_wrapper/riscy_decoder_gen.cpp
$(eval $(call declare_iss_build,riscy_single_regfile))
endif

ifeq '$(iss_class)' 'iss_riscy_v2_5_single_regfile'
cpu/iss/iss_riscy_v2_5_single_regfile_CFLAGS += -DISS_SINGLE_REGFILE -DPCER_VERSION_2
cpu/iss/iss_riscy_v2_5_single_regfile_SRCS += $(VP_BUILD_DIR)/cpu/iss/iss_wrapper/riscy_decoder_gen.cpp
$(eval $(call declare_iss_build,riscy_v2_5_single_regfile))
endif

ifeq '$(iss_class)' 'iss_riscy_v2_5'
cpu/iss/iss_riscy_v2_5_CFLAGS += -DPCER_VERSION_2
cpu/iss/iss_riscy_v2_5_SRCS += $(VP_BUILD_DIR)/cpu/iss/iss_wrapper/riscy_decoder_gen.cpp
$(eval $(call declare_iss_build,riscy_v2_5))
endif
