TOP=../..

include $(TOP)/configure/CONFIG
#----------------------------------------
#  ADD MACRO DEFINITIONS BELOW HERE

# Use typed rset structure (see 3.16.1 release notes)
USR_CPPFLAGS += -DUSE_TYPED_RSET

# xxxRecord.h will be created from xxxRecord.dbd
DBDINC += xxxRecord

# Install xxxSupport.dbd into <top>/dbd
DBD += xxxSupport.dbd

# Build an IOC support library
LIBRARY_IOC += streamdeviceSupport

# Compile and add code to the support library
streamdeviceSupport_SRCS += xxxRecord.c
streamdeviceSupport_SRCS += devXxxSoft.c

# Link locally-provided code into the support library,
# rather than directly into the IOC application, that
# causes problems on Windows DLL builds
streamdeviceSupport_SRCS += dbSubExample.c
streamdeviceSupport_SRCS += devstreamdeviceVersion.c
streamdeviceSupport_SRCS += streamdeviceHello.c
streamdeviceSupport_SRCS += initTrace.c

streamdeviceSupport_LIBS += $(EPICS_BASE_IOC_LIBS)

# Auto-generate a header file containing a version string.
# Version comes from the VCS if available, else date+time.
GENVERSION = streamdeviceVersion.h
# Macro name
GENVERSIONMACRO = streamdeviceVERSION

# Build the IOC application
PROD_IOC = streamdevice

# streamdevice.dbd will be created and installed
DBD += streamdevice.dbd

# streamdevice.dbd will include these files:
streamdevice_DBD += base.dbd
streamdevice_DBD += xxxSupport.dbd
streamdevice_DBD += dbSubExample.dbd
streamdevice_DBD += devstreamdeviceVersion.dbd
streamdevice_DBD += streamdeviceHello.dbd
streamdevice_DBD += initTrace.dbd
streamdevice_DBD += asyn.dbd
streamdevice_DBD += drvAsynUSBTMC.dbd
streamdevice_DBD += drvAsynIPPort.dbd
streamdevice_DBD += drvAsynSerialPort.dbd
streamdevice_DBD += calc.dbd
streamdevice_DBD += stream-scalcout.dbd
streamdevice_DBD += stream.dbd
streamdevice_DBD += as.dbd
streamdevice_DBD += asSupport.dbd


# streamdevice_registerRecordDeviceDriver.cpp derives from streamdevice.dbd
streamdevice_SRCS += streamdevice_registerRecordDeviceDriver.cpp

# Build the main IOC entry point where needed
streamdevice_SRCS_DEFAULT += streamdeviceMain.cpp
streamdevice_SRCS_vxWorks += -nil-

# Link in the code from our support library
streamdevice_LIBS += streamdeviceSupport
streamdevice_LIBS += asyn
streamdevice_LIBS += calc
streamdevice_LIBS += autosave
streamdevice_LIBS += stream

# To build SNL programs, SNCSEQ must be defined
# in the <top>/configure/RELEASE file
ifneq ($(SNCSEQ),)
    # Build sncExample into streamdeviceSupport
    sncExample_SNCFLAGS += +r
    streamdevice_DBD += sncExample.dbd
    # A .stt sequence program is *not* pre-processed:
    streamdeviceSupport_SRCS += sncExample.stt
    streamdeviceSupport_LIBS += seq pv
    streamdevice_LIBS += seq pv

    # Build sncProgram as a standalone program
    PROD_HOST += sncProgram
    sncProgram_SNCFLAGS += +m
    # A .st sequence program *is* pre-processed:
    sncProgram_SRCS += sncProgram.st
    sncProgram_LIBS += seq pv
    sncProgram_LIBS += $(EPICS_BASE_HOST_LIBS)
endif

# Link QSRV (pvAccess Server) if available
ifdef EPICS_QSRV_MAJOR_VERSION
    streamdevice_LIBS += qsrv
    streamdevice_LIBS += $(EPICS_BASE_PVA_CORE_LIBS)
    streamdevice_DBD += PVAServerRegister.dbd
    streamdevice_DBD += qsrv.dbd
endif

# Finally link IOC to the EPICS Base libraries
streamdevice_LIBS += $(EPICS_BASE_IOC_LIBS)

include $(TOP)/configure/RULES
#----------------------------------------
#  ADD EXTRA GNUMAKE RULES BELOW HERE

# Explicit dependency needed for generated header file
devstreamdeviceVersion$(DEP): $(COMMON_DIR)/$(GENVERSION)
